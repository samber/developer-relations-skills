# Doc-Sample Test Tiers — CI Enforcement Plan

Snippets are tiered by **blast radius** (who/what gets hit) and **reversibility**, not by topic. That's what determines whether CI can safely run them for real.

## Tier 1 — Execute live, every run (safe, free, reversible)
Read-only or trivially-reversible calls against your own test account. No cost, no destruction, no third party involved.

**(a) Quickstart auth call** → **Tier 1**
- CI enforcement: run for real on every docs change against a sandbox/test account, assert 2xx + expected token shape. Revoke/expire the token after.
- No CI pipeline needed to start this — a single script on a cron is enough. This is the only tier you can stand up before the 6-week eng freeze lifts.

## Tier 2 — Execute on a schedule, cost-capped (billable, non-destructive)
Real money changes hands, but nothing is destroyed and no third party is touched.

**(b) Transcription job, $0.90/min** → **Tier 2**
- CI enforcement: never run per-commit — cost scales with every doc edit. Run weekly (or on release only), against the shortest possible fixture (a few-second clip) to cap spend at cents per run. Hard-fail on unexpected job duration; alert on cost drift.
- Needs someone to own a billing alert — flag as blocked on eng time, not urgent.

## Tier 3 — Never execute live, dry-run/lint only (destructive, own infra)
Irreversible mutation of real infrastructure. Blast radius is internal but permanent.

**(c) `kubectl delete namespace voltmesh-dev`** → **Tier 3**
- CI enforcement: **never** run for real against anything but a disposable, purpose-built sandbox cluster — and building that sandbox is itself eng work you don't have for 6 weeks. Until then: syntax/dry-run validation only (`kubectl delete namespace voltmesh-dev --dry-run=client`), plus mandatory human review on any diff to this snippet.
- Treat "namespace name looks like a real env" as a standing red flag — rename in the doc to something unambiguously fake (`example-namespace`) so a copy-paste accident doesn't hit a real cluster.

## Tier 4 — Never execute live, mock/contract only (external third party)
Even a non-destructive call causes real noise on a system you don't own — partner sees spam traffic, rate limits trip, their alerts fire.

**(d) POST to partner webhook** → **Tier 4**
- CI enforcement: **never** hit the real partner URL from automation. Point the test at a mock endpoint you control (local stub or request-bin style) and validate payload shape, headers, and auth only. Any real end-to-end check happens manually, rarely, with the partner's knowledge — not something CI should ever trigger unattended.

## Rollout given your constraints
- Start now, no eng time required: Tier 1 only, via a standalone script — not a CI pipeline.
- Everything else (Tiers 2–4) stays manual-review-only until sandbox infra, billing alerts, and a mock server exist — don't promise automated coverage on (b), (c), (d) before the 6 weeks are up.
- Of the 160 snippets, sort the rest into these same four tiers before deciding what "test suite" even means for this repo — most of the remaining count should land in Tier 1 if the docs are mostly read/auth/list calls.