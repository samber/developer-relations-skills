# Authorization Code Flow with PKCE, Explained

OAuth's authorization code flow was designed in an era of confidential clients: a server-side app with a client secret it could keep hidden from the browser. Mobile apps and single-page apps broke that assumption — you can't ship a secret inside an APK or a bundle.js and expect it to stay secret. PKCE (RFC 7636, pronounced "pixy") closes the gap that opened up when public clients started using a flow built for confidential ones.

## The problem PKCE solves

Without PKCE, the authorization code flow looks like this:

1. The client redirects the user to the authorization server.
2. The user authenticates and consents.
3. The authorization server redirects back to the client's redirect URI with a `code`.
4. The client exchanges that `code` for tokens at the token endpoint.

Step 3 is the weak point. The redirect happens through the user's browser, and on mobile, the redirect URI is usually a custom scheme (`myapp://callback`) or, on newer platforms, an app link. Custom schemes aren't exclusive — more than one installed app can register the same one. A malicious app that registers your app's scheme can intercept the redirect, grab the `code`, and hit the token endpoint before your app does. Since public clients have no secret, nothing in step 4 proves the code redeemer is the same party that started the flow in step 1.

`state` doesn't fix this — `state` protects against CSRF (binding the callback to the browser session that started the request), not against a different app entirely stealing the code from an OS-level redirect.

## How PKCE fixes it

PKCE binds the code exchange to the party that initiated the request, without needing a shared secret. The client proves it holds a value only it generated, by presenting a hash of that value up front and the raw value later.

**Before the redirect**, the client generates a random `code_verifier`:

```javascript
function generateCodeVerifier() {
  const bytes = crypto.getRandomValues(new Uint8Array(32));
  return base64url(bytes); // 43-128 chars, per RFC 7636 §4.1
}
```

It derives a `code_challenge` from it:

```javascript
async function generateCodeChallenge(verifier) {
  const digest = await crypto.subtle.digest(
    "SHA-256",
    new TextEncoder().encode(verifier)
  );
  return base64url(digest);
}
```

It sends the challenge — not the verifier — in the authorization request:

```
GET /authorize?
  response_type=code
  &client_id=abc123
  &redirect_uri=myapp://callback
  &state=xyz
  &code_challenge=E9Melhoa2OwvFrEMTJguCHaoeK1t8URWbuGJSstw-cM
  &code_challenge_method=S256
```

The authorization server stores the challenge alongside the issued code and returns the code to the redirect URI as usual. At this point, an attacker who intercepts the code still can't redeem it, because redemption now requires the verifier — which never left the client.

At the token endpoint, the client sends the original verifier instead of a client secret:

```
POST /token
Content-Type: application/x-www-form-urlencoded

grant_type=authorization_code
&code=SplxlOBeZQQYbYS6WxSbIA
&redirect_uri=myapp://callback
&client_id=abc123
&code_verifier=dBjftJeZ4CVP-mB92K27uhbUJU1p1r_wW1gFWFOEjXk
```

The server hashes the received `code_verifier` with the stored `code_challenge_method`, compares it to the `code_challenge` it stored at the authorization step, and rejects the exchange on mismatch. An attacker holding a stolen code has no way to produce a verifier that hashes to the challenge the server already committed to — that's the entire security property, and it rests on SHA-256 preimage resistance rather than secrecy of the client.

## S256 vs. plain

RFC 7636 defines two challenge methods: `S256` (SHA-256 then base64url) and `plain` (the challenge equals the verifier, unhashed). `plain` exists only for clients that can't compute SHA-256, which in practice means none — every modern platform has a crypto library. `plain` gives no protection against an attacker who can read the authorization request itself (e.g., through a poorly isolated in-app browser), since challenge and verifier are identical. Use `S256`. If you're implementing an authorization server, consider rejecting `plain` outright rather than merely supporting `S256` as an option — a downgrade to `plain` should not be a silent, negotiable fallback.

## PKCE isn't just for public clients anymore

The original spec framed PKCE as protection for public clients, but the current guidance (RFC 9700, and OAuth 2.1's draft consolidation) recommends PKCE for confidential clients too. It's a second, independent defense against authorization code injection — including cases where the code leaks through referrer headers, browser history, or server logs rather than through redirect URI hijacking. `client_secret` proves the client's identity; PKCE proves the code redeemer is the same party that started this specific flow. They cover different threats and are cheap to run together.

## Implementation notes that cause real bugs

- **Generate a fresh verifier per authorization request**, never per client. Reusing one across requests reintroduces the binding weakness PKCE exists to remove.
- **Use a CSPRNG for the verifier**, not `Math.random()` or an unseeded RNG. The RFC's 43-character minimum only matters if the entropy behind it is real.
- **Store the verifier tied to the specific in-flight request** (e.g., session storage keyed by `state`), not in a variable that could survive across concurrent flows — a user opening two login attempts in two tabs is enough to expose this bug.
- **The verifier is single-use.** Don't cache it for retry logic across a new authorization request; generate a new verifier/challenge pair per attempt.
- **`code_challenge_method` is not optional in practice**, even though the RFC allows omitting it (defaulting to `plain`). Send `S256` explicitly and reject servers that silently accept `plain` instead.

## Where this leaves you

PKCE doesn't replace `state`, redirect URI validation, or TLS — it closes one specific gap: a stolen authorization code being redeemable by anyone who intercepts it in transit through a browser or OS redirect. Pair it with strict redirect URI matching (exact match, not prefix match) and you've closed the two most common ways authorization codes get hijacked in practice.