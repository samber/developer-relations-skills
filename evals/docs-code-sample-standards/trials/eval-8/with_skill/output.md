## Sample audit — Cindermoor Pay "create a payout" block

**Findings** (checklist format: evidence → fix)

```
docs/<page>:<line> - fence has no language identifier, so no syntax highlighting
  or lint coverage applies to it. Fix: tag it ```bash.

docs/<page>:<line> - command line starts with `$`, a shell prompt character copied
  verbatim by anyone who selects the whole block. Fix: drop the `$`.

docs/<page>:<line> - `sk_live_9fKq2pRt7LwZ3xVn8MdE4cYb` is a real-format LIVE-mode
  secret key, unbracketed, indistinguishable from a working credential. This is
  why support keeps telling readers to swap it - nothing in the block marks it
  as a placeholder. [P0]

docs/<page>:<line> - trailing `...` truncates the request body inside a
  click-to-copy block; the reader can't see what parameter(s) it's hiding, and
  can't run the command as shown. [P1]

docs/<page>:<line> - response JSON is glued directly under the request in the
  same fence, so both get copied together and the reader has to hand-delete the
  response before running the command. [P2]

docs/<page>:<line> - no top-level context: nothing states this call moves real
  money once run with a live key, and nothing states the unit `amount` is in
  (minor units/cents vs. major units). Ambiguity and incompleteness are the two
  defects Uddin & Robillard's documentation survey rated most severe. [P2]
```

**⚠️ Conflict with your requested placeholder.** You asked for `acct_test_51H8xQ2eZvKY` in place of angle brackets. The skill's Safety rule is explicit: *"No plausible-looking fake values a reader could paste verbatim; no real-format credentials, account IDs or endpoints"* — and the checklist's own failure-mode table names this exact scenario: *"Reader pastes a placeholder verbatim → use angle brackets and reserved-range hosts, never a plausible fake value."*

That's not a style preference, it's the direct fix for the support ticket you're describing. `acct_test_51H8xQ2eZvKY` is shaped exactly like a real key — nothing in the block would tell a skimming reader it isn't one, so they paste it verbatim and it fails silently (or worse, if it happens to collide with a real-looking pattern). Swapping `sk_live_...` for a differently-prefixed but equally plausible string doesn't remove the defect, it relocates it. I used bracketed placeholders below instead. If you still want the literal string after reading this, say so and I'll swap it back — but do it with brackets around it (`<acct_test_51H8xQ2eZvKY>`), not bare.

**Tier:** Run. This is your highest-copy sample and sits on the primary adoption path (first API call) — the policy promotes exactly this case to Run regardless of the general effort ranking. Source it from a real test-mode fixture account, execute nightly (payout creation is service-side, not repo-side, so it drifts on the provider's schedule, not on doc edits), and assert the response shape rather than the literal `id`.

---

## Cleaned sample

Create a payout. Replace `<YOUR_API_KEY>` with your **test-mode** secret key from your dashboard — test-mode keys never move real money, so it's safe to run this exactly as written. `amount` is in the smallest currency unit (e.g. cents for USD).

```bash
curl https://api.cindermoor.com/v2/payouts \
  -X POST \
  -H "Authorization: Bearer <YOUR_API_KEY>" \
  -d amount=4500
```

Add any other fields your integration requires (e.g. currency, destination) before running this — check the Payouts reference for the full parameter list; don't truncate a docs example with `...`.

Expected response:

```json
{ "id": "po_7731", "status": "pending" }
```

Never paste a `sk_live_` key into a doc, script, or shared terminal — a live key runs against real funds.

---

## Rules — stop the rest of the docs looking like this

**Anatomy — every sample must have:**
- One top-line sentence stating what it does and what the reader needs before running it (key, account, prior step).
- Command and response in separate fenced blocks — never glued into one copy-paste target.
- A language identifier on every fence; use `plaintext` if nothing fits.
- No `...`, no `…`, no elided parameters. Show every field, or name what's missing in prose above the block.
- A stated unit for every ambiguous numeric field (amount, timestamp, duration).

**Safety — copy-paste and credentials:**
- No shell prompt characters (`$`, `#`, `PS>`) inside a command block.
- Placeholders always in angle brackets (`<YOUR_API_KEY>`), explained in the sentence above the block. Never a plausible-looking fake value, real-format account ID, or endpoint — that's what trains readers to paste it unchanged.
- Every auth example uses a test-mode key format, never live-mode. State this rule once, on the auth setup page, and link every other sample to it instead of repeating the warning.
- Any call with a real financial or destructive side effect (payouts, transfers, deletes) gets an explicit one-line warning above the block, not just a safe-looking placeholder.

**Ownership:** put this page on the same freshness window and CI check as the rest of the quickstart path — a red build on this specific block should page whoever owns the Payouts API, since it's your most-copied sample and a break here is a break for the most readers at once.