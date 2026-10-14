No CI exists today. Per skill rule: no CI + zero engineering hours deletes both Run and Compile-only tier from menu right now. All 4 samples are **unverified**, not passing, until a pipeline exists. Tiers below are target tiers — what CI enforces once you build it.

**P0 flag first** (per skill: report P0 immediately, not at end): sample (c), `kubectl delete namespace voltmesh-dev`, uses a real-looking namespace name with no guard. That is an unguarded destructive command — P0 harm class regardless of tier or CI. Fix costs zero engineering hours, no pipeline needed:

1. Replace `voltmesh-dev` with placeholder `<YOUR_DEV_NAMESPACE>`.
2. Add warning line above block: check `kubectl config current-context` before running.
3. Add `--dry-run=client` variant as the copy-paste default; real delete stays a separate, clearly labeled step.

**Tier per sample:**

**(a) Quickstart auth call — tier: Run**
- Why: primary adoption path (quickstart, auth, first call). Skill baseline: 100% of these must be Run and green.
- CI enforces (once built): full execution + output diff against real response shape.
- Needs: dedicated test account, secrets from CI secret store not sample, deterministic teardown, scheduled run (catches server-side auth drift, not just doc edits).
- This is the one Run-tier investment worth the standing job. Everything else below stays capped at Compile-only no matter how much engineering time you free up.

**(b) Transcription job, $0.90/min — tier: Compile-only**
- Why: billing sample. Rule: bills → no Run tier ever, whatever the pipeline can do.
- CI enforces: build/lint only, never executed (executing it in CI spends real money on every run).
- Anatomy fix, zero pipeline needed: put the per-minute cost in the sample's top comment so a reader isn't surprised when they run it themselves.

**(c) `kubectl delete namespace` — tier: Compile-only**
- Why: destructive command. Same no-Run rule.
- CI enforces: syntax/dry-run lint only (`kubectl apply --dry-run=client` style), never actual execution against a real cluster.
- Guard fix above is separate and urgent — do that regardless of tier work.

**(d) POST to partner webhook — tier: Compile-only**
- Why: third-party dependency. Same no-Run rule — also a compliance issue, not just a resource one: firing real traffic at another company's endpoint on every CI run needs their terms for automated traffic, which you don't have.
- CI enforces: build/lint only — type-check request construction and signature verification, never fire the call.
- Anatomy fix: show the signature-verification failure path (policy rule: show one failure path per API that fails in normal operation).

**Sequencing for 3 engineers, 6-week freeze:**

1. Now, zero engineering hours: fix the (c) guard, add cost disclosure to (b), add failure-path note to (d). Editing, not CI.
2. Week 6+, first available engineering time (~1 week): build one shared Compile-only batch job — lint/type-check pass covering (b), (c), (d) and most of the other 160 snippets. Cheapest, highest-leverage move per skill (a week to stand up, near-zero per sample after).
3. After that lands, invest in the Run harness for (a) only. That's the standing job (fixtures, credentials, teardown, scheduled re-run) — do not attempt it before step 2 exists.

Next: send the (c) namespace-placeholder fix to whoever owns that doc page today — no pipeline required to ship it.