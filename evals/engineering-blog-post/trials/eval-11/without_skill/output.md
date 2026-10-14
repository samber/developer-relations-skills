# Wavelength SDK v2: A New Auth Model, a Full Migration Guide, and Your First Integration

Wavelength SDK v2 is generally available today, across Node.js, Python, and Go. It's the first major version bump since we launched three years ago, and it touches nearly every part of the developer experience — how you authenticate, how you call the API, and how errors and pagination behave.

This post does three things. First, it tells the real story of why we rebuilt authentication from the ground up — it wasn't a cosmetic change, it came out of a security incident we take responsibility for. Second, it's a complete migration guide for anyone running v1 in production today. Third, if you've never touched Wavelength before, it's a step-by-step walkthrough that takes you from zero to a working integration in about fifteen minutes.

Read the section you need. If you're migrating, skip to "Everything That Broke." If you're starting fresh, skip to "Your First Integration." If you want the context behind the biggest change, keep reading.

## Why We Redesigned Auth

Every Wavelength v1 integration started the same way: you generated an API key in the dashboard, dropped it into an environment variable, and sent it as a plain header on every request. It was simple, and simple was the point — we optimized hard for time-to-first-request in the early days, and a static bearer token is about as fast as auth gets.

It's also, in hindsight, a design that ages badly, and one specific week in March made that impossible to ignore.

A customer's key showed up in a public GitHub repository — not because they were careless, but because a CI script echoed environment variables into a build log for debugging, and that log got attached to a public issue when someone filed a bug report. The key had been live for eleven months. It had full access to every endpoint: sending messages, reading message history, managing webhooks, even inviting new team members to the workspace. We caught it through routine anomaly monitoring — a sudden spike in send volume from an IP range the account had never used — and revoked it within twenty minutes of the spike. No customer data was exfiltrated as far as our logs show, and we disclosed the incident to the affected customer the same day.

But twenty minutes is only fast relative to how bad it could have been. The real problem surfaced when we sat down to fix it: revoking that one key meant regenerating the *only* key the customer had, which meant redeploying every service that used it, because v1 keys weren't scoped and there was no way to say "just cut off the key that leaked, the other three services are fine." One key, one blast radius: your entire workspace.

That's the failure mode static, unscoped, non-expiring credentials always converge on eventually. It's not a hypothetical — long-lived secrets end up in logs, in shell history, in support tickets pasted into chat, in old CI configs nobody remembers exists. The fix isn't "tell developers to be more careful." The fix is a credential design where a leak is contained and cheap to recover from by default.

We set three requirements for v2 auth:

- **A leaked credential should expire on its own.** No incident should ever again depend on someone noticing a spike in time.
- **Scope should be enforceable, not just documented.** A key used only to send transactional messages shouldn't be *able* to read message history or manage webhooks, regardless of what the dashboard UI suggests.
- **Rotation should never require a deploy.** Revoking one compromised credential should never force you to touch the four other services that don't share it.

What we shipped is a standard OAuth 2.0 client-credentials flow. You still generate a credential in the dashboard, but now it's a client ID and client secret pair, scoped at creation time to one or more permission sets (`messages:send`, `messages:read`, `webhooks:manage`, and so on). That pair is never sent on API requests directly — it's exchanged for a short-lived access token (one hour, signed, scoped down to exactly what the client ID was granted) that *is* sent on requests. If a client secret leaks, the blast radius is one hour of tokens minted from it, all carrying only the scopes you assigned, not your whole account. Revoking the secret is instant and independent of every other credential in the workspace.

The SDK handles the token exchange and caching transparently — you initialize a client with a client ID and secret, and the library fetches, caches, and refreshes access tokens behind the scenes. You don't write any token-handling code yourself; that's covered in the quickstart below. For customers with stricter compliance requirements, v2 also supports mutual TLS as an additional layer on top of the token flow, configurable per workspace.

The honest tradeoff: this adds a network round trip the first time a client mints a token, and again on refresh, which a single static header never required. We mitigated this by caching tokens for their full lifetime and refreshing proactively a few minutes before expiry, so in steady state it costs nothing per request. We measured p99 latency impact in production traffic during the beta and it was within noise. Given what it buys — no more single leaked string owning an entire account — it was an easy trade to make.

## Everything That Broke

If you're upgrading a v1 integration, here's the full list. None of these are cosmetic — every one requires a code change before v2 will work.

| Area | v1 | v2 |
|---|---|---|
| Package | `wavelength-sdk` | `@wavelength/sdk` |
| Auth | Static API key, `Authorization: Bearer <key>` | Client ID + secret, SDK exchanges for short-lived token automatically |
| Client init | `new Wavelength(apiKey)` | `new Wavelength({ clientId, clientSecret })` |
| Sending | `client.send(to, body)` | `client.messages.send({ to, body })` |
| Reading history | `client.history(limit, offset)` | `client.messages.list({ limit, cursor })` |
| Pagination | Offset-based | Cursor-based |
| Response shape | Flat object | `{ data, meta }` envelope |
| Errors | Single `WavelengthError` | Typed hierarchy: `AuthError`, `RateLimitError`, `ValidationError`, `ApiError` |
| Webhook signatures | HMAC-SHA1 over the raw body | HMAC-SHA256 over `timestamp.body`, with a 5-minute replay window |
| Minimum runtime | Node 12+, Python 3.6+, Go 1.16+ | Node 18+, Python 3.9+, Go 1.21+ |
| Default request timeout | 60s | 30s (configurable) |

Walking through the ones most likely to bite you:

**Auth and client initialization.** This is the change with no shortcut. Static API keys are deactivated for v1 traffic starting six months from today (see timeline below) but are being deprecated immediately for v2 — you cannot pass an old-style API key to the v2 client at all. Generate a client ID/secret pair from **Dashboard → API Access → Create Credential**, choose the scopes that pair actually needs (don't default to "all" — that defeats the reason we did this), and pass both to the constructor:

```typescript
// v1
import Wavelength from "wavelength-sdk";
const client = new Wavelength(process.env.WAVELENGTH_API_KEY);

// v2
import { Wavelength } from "@wavelength/sdk";
const client = new Wavelength({
  clientId: process.env.WAVELENGTH_CLIENT_ID,
  clientSecret: process.env.WAVELENGTH_CLIENT_SECRET,
});
```

**Method namespacing.** v1's flat method calls (`send`, `history`, `webhook`) are now grouped under resource namespaces (`messages.send`, `messages.list`, `webhooks.create`). This wasn't change for its own sake — v1's flat namespace was already colliding as we added features, and every new resource would have made it worse. If you have a large call site, a straightforward find-and-replace covers most of it: `client.send(` becomes `client.messages.send(`, and so on. Full mapping is in the migration reference doc linked at the end.

**Pagination.** Offset-based pagination on `history()` is gone. `messages.list()` now returns a cursor in the response envelope's `meta` field; pass that cursor back in to get the next page. This is a real behavior change, not just a rename — offsets drift when new messages arrive between page fetches, so if your integration paginates through history while messages are actively being sent, cursor pagination will also fix a class of duplicate/skipped-row bugs you may not have noticed you had:

```typescript
// v1
const page1 = await client.history(50, 0);
const page2 = await client.history(50, 50);

// v2
const page1 = await client.messages.list({ limit: 50 });
const page2 = await client.messages.list({ limit: 50, cursor: page1.meta.nextCursor });
```

**Error handling.** v1 threw one error class for everything, and you branched on `error.message` string matching — which is exactly as fragile as it sounds. v2 throws a typed hierarchy so you can branch on `instanceof`:

```typescript
try {
  await client.messages.send({ to, body });
} catch (err) {
  if (err instanceof RateLimitError) {
    await sleep(err.retryAfterMs);
  } else if (err instanceof ValidationError) {
    console.error("Bad request:", err.fields);
  } else if (err instanceof AuthError) {
    console.error("Credential problem:", err.message);
  } else {
    throw err;
  }
}
```

**Webhook signature verification.** If you verify inbound webhooks, this is a silent-failure risk if you miss it: v1 signed the raw body with HMAC-SHA1; v2 signs `${timestamp}.${body}` with HMAC-SHA256 and rejects anything outside a 5-minute window to prevent replay. Your existing verification code will not error — it will just always return false, which usually gets "fixed" by disabling verification entirely. Don't do that; use `client.webhooks.verify(payload, signatureHeader)`, which the SDK implements correctly for you.

**Migration timeline:**

- **Today**: v2 is stable and generally available. v1 remains fully supported.
- **+3 months**: v1 moves to security-fixes-only. No new features land on it.
- **+6 months**: v1 static API keys stop authenticating. Existing v1 integrations must have moved to v2 credentials by this date.
- **+9 months**: v1 endpoints return `410 Gone`.

We know "just migrate" is easy for us to say and harder to schedule. If you need more runway or hit something the migration guide doesn't cover, our support channel is the fastest way to reach us, and we'll work with you directly rather than let a deadline break your integration.

## Your First Integration

If you're new to Wavelength, here's the fifteen-minute path to sending your first message and verifying your first delivery event, on v2, from scratch.

**Prerequisites**

| Requirement | Version | Check |
|---|---|---|
| Node.js | 18+ | `node --version` |
| npm | 9+ | `npm --version` |
| A Wavelength account | — | [wavelength.dev/signup](https://wavelength.dev/signup) |

You should be comfortable with basic async/await JavaScript and reading JSON responses. Nothing else is assumed.

### Step 1: Create a credential

In the dashboard, go to **API Access → Create Credential**. Name it something that identifies where it'll run (e.g., `local-dev`), and grant it the `messages:send` scope only — you don't need `messages:read` or `webhooks:manage` for this tutorial, and scoping tightly from the start is the whole point of the new model. Copy the client ID and client secret shown; the secret is only displayed once.

### Step 2: Install the SDK

```bash
mkdir my-wavelength-app && cd my-wavelength-app
npm init -y
npm install @wavelength/sdk
```

### Step 3: Store your credentials

Create a `.env` file in the project root:

```
WAVELENGTH_CLIENT_ID=your_client_id_here
WAVELENGTH_CLIENT_SECRET=your_client_secret_here
```

Add `.env` to `.gitignore` now, before you forget — this is exactly the mistake the auth redesign above exists to make less catastrophic, but it's still worth not making.

```bash
echo ".env" >> .gitignore
npm install dotenv
```

### Step 4: Initialize the client and send a message

Create `index.js`:

```javascript
require("dotenv").config();
const { Wavelength } = require("@wavelength/sdk");

const client = new Wavelength({
  clientId: process.env.WAVELENGTH_CLIENT_ID,
  clientSecret: process.env.WAVELENGTH_CLIENT_SECRET,
});

async function main() {
  const response = await client.messages.send({
    to: "+15551234567",
    body: "Hello from my first Wavelength integration!",
  });

  console.log("Sent:", response.data.id, response.data.status);
}

main().catch((err) => {
  console.error("Send failed:", err.message);
  process.exit(1);
});
```

Run it:

```bash
node index.js
```

**Checkpoint.** You should see something like:

```
Sent: msg_8f2a1c accepted
```

🎉 That's a real message, sent through v2, with the SDK handling the OAuth token exchange entirely behind the scenes — you never touched a token directly, which is exactly the point.

### Step 5: Handle errors properly

Real integrations need to handle rate limits and validation failures, not just the happy path. Update the `main` function:

```javascript
const { RateLimitError, ValidationError, AuthError } = require("@wavelength/sdk");

async function main() {
  try {
    const response = await client.messages.send({
      to: "+15551234567",
      body: "Hello from my first Wavelength integration!",
    });
    console.log("Sent:", response.data.id, response.data.status);
  } catch (err) {
    if (err instanceof RateLimitError) {
      console.error(`Rate limited, retry after ${err.retryAfterMs}ms`);
    } else if (err instanceof ValidationError) {
      console.error("Invalid request:", err.fields);
    } else if (err instanceof AuthError) {
      console.error("Check your client ID/secret and their scopes:", err.message);
    } else {
      throw err;
    }
  }
}
```

Try it once with an intentionally malformed phone number (`to: "not-a-number"`) to see `ValidationError` fire — it's worth seeing each error type trigger at least once before you're debugging it in production at 2am.

### Step 6: Verify a delivery webhook

Messages are sent asynchronously; delivery status arrives as a webhook. Add a `webhooks:manage`-scoped credential (or add that scope to your existing one), then register an endpoint:

```javascript
await client.webhooks.create({
  url: "https://your-app.example.com/wavelength/webhook",
  events: ["message.delivered", "message.failed"],
});
```

On the receiving end, verify every inbound payload before trusting it — this is the HMAC-SHA256-with-timestamp scheme from the migration section above, and the SDK implements it for you:

```javascript
const express = require("express");
const app = express();
app.use(express.raw({ type: "application/json" }));

app.post("/wavelength/webhook", (req, res) => {
  const signature = req.headers["wavelength-signature"];
  const isValid = client.webhooks.verify(req.body, signature);

  if (!isValid) {
    return res.status(401).send("Invalid signature");
  }

  const event = JSON.parse(req.body);
  console.log("Delivery event:", event.type, event.data.id);
  res.status(200).send("ok");
});

app.listen(3000, () => console.log("Listening on http://localhost:3000"));
```

**Checkpoint.** Send another message from Step 5, and within a few seconds you should see a `Delivery event: message.delivered ...` line in this server's log. That's a complete, end-to-end integration: authenticated send, error handling, and verified webhook delivery.

### Troubleshooting

**`AuthError: invalid_client`** — Your client ID or secret is wrong, or the credential was deleted. Regenerate it from the dashboard and double check `.env` was actually loaded (`console.log(process.env.WAVELENGTH_CLIENT_ID)` to confirm).

**`AuthError: insufficient_scope`** — Your credential doesn't have the scope the call needs. Go back to **API Access** and add the missing scope; you don't need to regenerate the secret, scopes can be edited on an existing credential.

**Webhook signature always invalid** — Almost always one of two causes: you're reading `req.body` after it's been parsed as JSON (the raw bytes are required for the signature check, hence `express.raw()` above, not `express.json()`), or your server's clock has drifted outside the 5-minute replay window — check `date` on the host.

### Next steps

- Full API reference and every SDK method: [docs.wavelength.dev](https://docs.wavelength.dev)
- Detailed migration reference with the complete v1→v2 method mapping: [docs.wavelength.dev/migrate-v2](https://docs.wavelength.dev/migrate-v2)
- Questions while migrating: our support channel is monitored daily, and for anything time-sensitive tied to the sunset timeline, say so up front and we'll prioritize it.

That's v2 — a smaller blast radius when something leaks, a clearer API surface, and hopefully a faster fifteen minutes to your first message than v1 ever was.