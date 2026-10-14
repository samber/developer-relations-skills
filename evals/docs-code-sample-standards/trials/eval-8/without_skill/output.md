## The problem with the current block

That block has a **live secret key** (`sk_live_…`) hard-coded into a docs page, and "swap it for your own" is instruction, not enforcement — most readers copy-paste verbatim. A `sk_live_` string sitting in public docs is a live production credential exposed to the internet, indexable by any bot that scrapes docs pages for `sk_live_` patterns. Two separate problems, both need fixing: the specific block, and the pattern that let it happen.

**First, check whether `sk_live_9fKq2pRt7LwZ3xVn8MdE4cYb` is a real, active key.** If it is, rotate it now, independent of the docs fix — treat it as compromised the moment it was published, regardless of intent.

## Cleaned block

```bash
curl https://api.cindermoor.com/v2/payouts \
  -X POST \
  -H "Authorization: Bearer acct_test_51H8xQ2eZvKY" \
  -H "Content-Type: application/x-www-form-urlencoded" \
  -d amount=4500 \
  -d currency=usd
```

```json
{
  "id": "po_7731",
  "status": "pending",
  "amount": 4500,
  "currency": "usd"
}
```

> This is a **test-mode** key. It only runs against the sandbox and can't move real funds. Grab your own from Dashboard → Developers → API keys, and use a `_live_` key only once you're ready for production.

Changes made and why:
- `sk_live_…` → `acct_test_51H8xQ2eZvKY`: a well-formed, obviously non-functional value instead of a real credential.
- Dropped the trailing `...`: incomplete commands get copy-pasted incomplete, then readers file support tickets when they fail.
- Added `Content-Type` header: the original silently relied on curl's default, which isn't guaranteed across curl versions/platforms.
- Added the callout: tells the reader what to do, instead of leaving "swap the key" as tribal support-channel knowledge.
- Full JSON response instead of a fragment: matches what a reader actually gets back.

## Rules for the rest of the docs

1. Every credential shown in docs is a test/sandbox credential. Never a live one — "readers will swap it" is not a control.
2. Test credentials always carry the `_test_` marker (`acct_test_`, `sk_test_`, etc.). Never `_live_` in a public-facing example, ever.
3. Generate docs example keys from a dedicated sandbox account with zero production access. Never copy a key from a real dashboard session into a docs PR.
4. Use realistic-looking dummy values, not `<angle-bracket>` placeholders. They copy-paste cleanly and show the correct format without being functional.
5. Never truncate a command with `...`. Show the full request and the full response, or split into a "minimal" and "with options" example.
6. Run a secret scanner (gitleaks or equivalent) in CI on every docs PR. Catch a real key before merge, not after a reader reports it.
7. Treat any live key that reaches a merged docs page as compromised. Rotate it immediately, don't wait to confirm exploitation.
8. Add one line stating where to get a real key and when to switch to `_live_`. Don't leave that step to support-channel tribal knowledge.