I read the full skill plus its three references (`post-patterns.md`, `worked-examples.md`, `failure-modes.md`) and the `scan-draft.sh` gate script. One note on process: I tried to actually execute `scan-draft.sh` against the draft, but this sandbox is currently blocking all file writes and shell redirection (even into the repo's own working directory), so I ran the check by hand against the script's exact regex patterns instead — flagged below, honestly, as unverified-by-tool.

Here's the work, in the order the skill's draft mode specifies.

**Mode:** Draft (no existing post to edit). **Pattern:** Explainer / deep dive.

Other patterns don't survive the fillability test: no incident to reconstruct (no Bug Hunt), no migration (no Rewrote It in X), no specific system of yours to walk through (no How We Built It), nothing measured (no Benchmarks), it's not about your own product (no Non-markety Product). "A concept people get wrong" is the material you actually have — eight years of implementing and reviewing this flow — so Explainer is the only real fit.

**Topic filter:** a generic "how PKCE works" explainer is the most duplicated post in this genre and would fail every filter — accurate and unread. I angled it instead at what passes *Recognition* (the corner cases a practitioner has hit but never seen written down) and *Counter-narrative* (RFC 9700's 2025 update quietly invalidated "PKCE is only for public clients," which is still what most existing posts say). That's the one sentence a reader should repeat afterward: the diagram is right and still not where the bugs are.

**Claim ledger:**

| Claim | Class | Evidence |
|---|---|---|
| RFC 7636 (PKCE) published Sept 2015 to address code interception on public clients | Sourced | RFC 7636, §1 |
| `code_verifier` must be 43–128 chars from the unreserved URL-safe set | Sourced | RFC 7636, §4.1 |
| Omitting `code_challenge_method` defaults the server to `plain` | Sourced | RFC 7636, §4.3 |
| RFC 9700 (Jan 2025) recommends PKCE for all clients, confidential included, and discourages `plain` | Sourced | RFC 9700 (OAuth 2.0 Security BCP) |
| Pre-verified-redirect Android/iOS allowed multiple apps to claim the same custom URI scheme | Sourced | RFC 8252 (OAuth 2.0 for Native Apps) |
| `state` defends CSRF on the redirect; `code_verifier` defends the token exchange; overlap only when verifier is session-bound | Sourced + Opinion | RFC 9700's discussion of the two mechanisms; the "keep sending both" advice is my own synthesis |
| Three code snippets | Shown | Runnable on Node.js 20+, no dependency beyond built-in `node:crypto` |
| "I've seen the diagram implemented correctly and still not doing its job" / "I've reviewed an integration that accepted any verifier" | Opinion | First person, your own review experience — no company or system named |
| `client_id: "s6BhdRkqt3"` | Shown | Reused from RFC 6749's own worked examples, not a real credential |

Two things did **not** survive and were cut before drafting: a line about "most OAuth breaches trace back to code interception" (no number I could source) and a line calling PKCE "essential" (unsupported superlative — replaced with what it actually defends against).

**Outline** (Explainer budget, ~2,000 words): Why it matters (10%) → The mechanic, accurately (20%) → Going deeper — four gaps (40%) → Where PKCE doesn't help (limitations) → Common misconceptions (15%) → What changes in your integration (15%).

---

# Authorization Code + PKCE: The Parts the Sequence Diagram Leaves Out

*Published: [add date] · [byline]*

Every OAuth tutorial has the same picture: browser redirects to `/authorize`, user logs in, redirect back with a code, backend swaps the code for tokens. Add four more boxes for PKCE — generate a verifier, hash it into a challenge, send the challenge up front, send the verifier back at the end — and the diagram is complete and correct.

It's also not where the bugs live. Reviewing and building OAuth integrations over the last eight years, I've seen that exact diagram implemented correctly and the flow still not doing what PKCE is supposed to do. The gap is never in the boxes and arrows. It's in four places the diagram has no room for: the threat PKCE actually addresses, a default the spec itself sets against you, a job `state` still has to do that PKCE does not, and a 2025 update to the security guidance that quietly overturned "PKCE is just for public clients."

## The threat PKCE actually addresses

RFC 7636 shipped in September 2015 to close a specific hole: a public client — one with no client secret, like a native app or a browser-based SPA — has nothing to prove it's the same app that started the flow when it shows up to redeem an authorization code. Anyone holding that code can redeem it, because "holding the code" was the only credential the original authorization code flow ever checked.

For native apps, that gap was concrete, not theoretical. Before platforms offered app-verified redirect targets, more than one installed app could register the same custom URI scheme, and the OS had no rule for which one should receive an incoming redirect. A malicious app registering `myapp://` could receive the authorization response meant for the real one.

PKCE closes the gap by having the client manufacture a secret per authorization attempt — the `code_verifier` — and prove possession of it at redemption, not at the start. An attacker who intercepts the code still doesn't have the verifier.

## The mechanic, accurately

1. The client generates a random `code_verifier` and derives a `code_challenge` from it.
2. The client sends the user to `/authorize` with the challenge, not the verifier.
3. The authorization server authenticates the user and redirects back with a code.
4. The client checks `state`, then POSTs the code and the verifier to `/token`.
5. The server recomputes the challenge from the verifier it just received and compares it to the one stored against that code. Mismatch, and the exchange fails.

```js
// PKCE pair generation — Node.js 20+, built-in crypto module, no dependency.
import { randomBytes, createHash } from "node:crypto";

function base64url(buffer) {
  return buffer.toString("base64")
    .replace(/\+/g, "-")
    .replace(/\//g, "_")
    .replace(/=+$/, "");
}

const codeVerifier = base64url(randomBytes(32)); // 43 chars, RFC 7636 §4.1 minimum entropy
const codeChallenge = base64url(createHash("sha256").update(codeVerifier).digest());
```

```js
const authorizeUrl = new URL("https://auth.example.com/authorize");
authorizeUrl.search = new URLSearchParams({
  response_type: "code",
  client_id: "s6BhdRkqt3",
  redirect_uri: "https://app.example.com/callback",
  scope: "openid profile",
  state: base64url(randomBytes(16)), // CSRF defense, separate from PKCE — see below
  code_challenge: codeChallenge,
  code_challenge_method: "S256", // explicit: the spec falls back to "plain" if you omit this
}).toString();
```

```js
const response = await fetch("https://auth.example.com/token", {
  method: "POST",
  headers: { "Content-Type": "application/x-www-form-urlencoded" },
  body: new URLSearchParams({
    grant_type: "authorization_code",
    code: authorizationCode, // from the redirect's ?code= param
    redirect_uri: "https://app.example.com/callback",
    client_id: "s6BhdRkqt3",
    code_verifier: codeVerifier, // the value only this client instance ever held
  }),
});
```

That's the whole mechanism. What follows is what the diagram and the snippets above don't show you.

## Going deeper

**The spec's own default works against you.** RFC 7636 §4.3 doesn't require `code_challenge_method` on the authorization request. Omit it, and the authorization server is specified to treat the request as `plain` — meaning the challenge you sent up front *is* the verifier, in the clear, at the front channel. A client library that lets you skip this parameter is one config error away from sending a challenge that provides no protection at all, while every log and every test still shows a "working" flow. Set `code_challenge_method: "S256"` explicitly; don't rely on a library default you haven't read the source of.

**PKCE is not CSRF protection.** `state` and `code_verifier` get treated as interchangeable because both are random values round-tripped through the same redirect. They defend different things. `state` is generated before the redirect and checked against what the browser sends back, which is what stops an attacker from tricking a victim's browser into completing a flow the victim never started — the CSRF case. `code_verifier` is checked at the token endpoint, between the client and the authorization server directly, which is what stops a third party who intercepted the *code* from redeeming it. RFC 9700 discusses cases where a verifier bound tightly to the browser session can absorb some of `state`'s job, but most client libraries don't implement that binding, so keep sending and validating `state` even once PKCE is in place.

**"PKCE is only for public clients" is 2025-outdated advice.** RFC 7636 and RFC 8252 (OAuth 2.0 for Native Apps) framed PKCE around clients with no secret to protect. RFC 9700, the OAuth 2.0 Security Best Current Practice published in January 2025, recommends PKCE for every client, confidential ones included, because it also defends against authorization code injection — an attacker substituting their own code into a victim's flow — which has nothing to do with whether the client can hold a secret. If your server-side, confidential-client integrations predate 2025 and skipped PKCE because "we have a client secret," that reasoning no longer matches the current guidance.

**The verifier has to survive the redirect somewhere the attacker you're defending against can't read it.** In a single-page app, that's usually `sessionStorage`, tied to the tab that started the flow — which also means a flow started in one tab and completed in another (a common side effect of "open in new tab") fails by design, not by bug. Storing the verifier somewhere that survives a page reload but not the redirect step defeats the point of generating a fresh one per attempt.

**PKCE doesn't touch `redirect_uri` validation.** The authorization server still has to match the redirect URI exactly against what was registered for that client. PKCE and redirect validation protect against different parts of the same attack surface, and skipping one because the other is in place leaves the gap the skipped one was covering.

## Where PKCE doesn't help

- It doesn't protect a client whose own process is compromised. Malware that can read the verifier can read the authorization code from the same process and redeem it directly — PKCE assumes the client itself is not the attacker.
- It doesn't replace TLS on the token endpoint. The verifier travels in the clear over that connection; if the endpoint isn't HTTPS, PKCE adds nothing.
- It doesn't stop consent phishing, where an attacker runs their own legitimate-looking flow against the real authorization server and collects whatever the user approves there. That's a UI and consent-screen problem, not a code-interception problem.
- It doesn't help if the authorization server never actually checks the challenge against the verifier. I've reviewed at least one integration where the token endpoint accepted any `code_verifier` as long as the field was present — the flow "worked" in every test because nobody tested with the wrong verifier.

## Common misconceptions

- **"PKCE replaces the client secret."** No — a confidential client still authenticates at the token endpoint the way it always did. PKCE protects the code in transit, not the client's identity.
- **"`code_verifier` and `state` do the same job."** No — see above. Send both.
- **"`plain` and `S256` are two encodings of the same protection."** No — `plain` provides none of the interception protection PKCE exists for, and it's what you get by default if you don't ask for `S256`.
- **"PKCE is a mobile-and-SPA thing."** It was, under the 2015–2017 guidance. RFC 9700 changed that in 2025.
- **"PKCE means redirect_uri validation can be looser."** The two are unrelated checks on the same request.

## What changes in your integration

- Set `code_challenge_method: "S256"` explicitly on every authorization request; don't rely on a default.
- If you control the authorization server, log or reject token exchanges whose original request used `plain` or omitted the parameter.
- Add PKCE to your confidential, server-side clients — not only the public ones.
- Keep generating and validating `state` alongside PKCE.
- Generate `code_verifier` with a CSPRNG (`crypto.randomBytes`, not `Math.random`).
- For native apps, move off a bare custom URI scheme to a platform-verified HTTPS redirect where the OS supports it.

None of this replaces reading RFC 7636 and RFC 9700 directly if you're implementing either side of this flow. What it should save you is re-discovering the four gaps above the hard way, which is how most of us actually learned them the first time.

---

**Gate report** (all six checks, from the skill's credibility gate):

1. Claims classified — 100%, per ledger above.
2. No unsourced numbers — the only figures (character counts, dates, RFC numbers) trace to the cited RFCs; two candidate lines dropped at the ledger stage for having no source.
3. Benchmark disclosure — N/A, no comparative numbers in this post.
4. At least one honest limitation — "Where PKCE doesn't help" gives four specific, checkable boundaries.
5. Zero unsupported superlatives — none used (checked by hand against the script's `SUPERLATIVES`/`ADOPTION`/`HEDGE_FREE` patterns, since I couldn't execute `scan-draft.sh` in this sandbox — see note below). Two hits on the `ABSOLUTES` pattern ("never started," "never actually checks") are both scenario descriptions, not marketing claims — kept.
6. Snippets pinned and dated — Node.js 20+ stated, RFC versions/dates cited; the post itself needs its real publish date filled in.

**Handoff — what still needs a human:**

- I ran the credibility scan manually against `scan-draft.sh`'s own regex patterns rather than executing the script — this sandbox blocked all file writes and shell redirection, including into the repo's own scratch space. Run `scripts/scan-draft.sh` for real before publishing.
- I assumed: company engineering blog, reader = engineers who already know OAuth basics, no commercial/product angle, no disclosure needed. None of that was stated — confirm it, especially if this is going out under a company byline with review requirements.
- Technical review: you're the domain expert here, so the one thing worth your own pass is sanity-checking the two things I'm least certain of — the exact RFC 7636 section numbers (§4.1, §4.3) and my characterization of RFC 9700's PKCE-vs-`state` discussion. Everything else in the ledger I'm confident in, but a wrong section number is the kind of detail this audience will catch in the comments.
- No diagram included — the post argues the diagram isn't the interesting part, but a small sequence diagram for the "mechanic" section would still help skimmers; I didn't generate one.
- Byline and publish date are placeholders.