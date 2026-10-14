# Fernhill Cloud code-sample testing plan — reaching 80% tested

Built with the **docs-code-sample-standards** skill: inventory-before-audit (NN/g) and ROT triage at corpus scale, Docs-as-Tests (Manny Silva) at page scale, Google Cloud's ranked principles (clarity → idiomaticity → consistency → simplicity → portability → maintainability) to settle rule conflicts. "80% tested" is this skill's own self-set baseline (tier Run or Compile-only, executed by CI) — leadership's target and this skill's default happen to match exactly, which is useful: the plan below is not inventing a new bar, it is hitting a documented one.

## 0. Assumptions standing in for the interview

The skill interviews before classifying. No live session, so these are stated assumptions — correct them before Phase 1 locks in:

- **Product class:** hosted API with per-language SDKs (`client.buckets.get(id)` reads as an object-storage-style resource API). *(Q2)*
- **Tier-1 languages:** assume 5 (Python, TypeScript/Node, Go, Java, + REST/cURL as the reference baseline) until real download/telemetry data picks the actual five. *(Q3)*
- **Nothing is executed today** — 700 hand-written one-liners "years old" and 120 copy-pasted auth blocks are both hallmarks of pure hand-maintained Markdown, no CI. *(Q4)*
- **An API spec (OpenAPI/protobuf) exists** behind the 700 reference one-liners — confirm this; it's the single fact that determines whether Segment A is cheap or expensive to fix. *(Q8)*
- **This is a standing-enforcement mandate**, not a one-off cleanup — it's on your objectives, so it recurs every release. *(Q10)*
- **Date:** "this year" — treated as ~2–3 quarters of runway, so the plan front-loads the cheapest, highest-leverage segments. *(Q9)*

## 1. Inventory (census) — run this first, for real

The 1,400/700/120 figures in the brief are a given starting count, not a measured census. Before anything else:

```
scripts/sample-audit.py docs/ --format summary
scripts/sample-audit.py docs/ --format json --ext .md,.mdx > inventory.json
```

This gets you, mechanically, on 100% of the corpus: language distribution, per-sample line count, existing (probably absent) tier markers, and every P0-shaped flag — `credential-shaped-literal`, `insecure-switch`, `destructive-command` — before a single human opinion enters the process. Report any `credential-shaped-literal` hit immediately; it doesn't wait for a finished report.

Given the corpus's age profile (self-reported "years old"), also run a symbol-existence pass against the current SDK surface before triaging anything else. Zhou & Walker (FSE 2016) measured 43% deprecated-API rate in Stack Overflow's Java posts — that's the gate this skill sets: if Fernhill's 700 old one-liners come in above 43% dead-symbol rate, a deprecation-detection gate becomes the first build item, ahead of everything below, because the corpus has a detection problem rather than an authoring one.

## 2. ROT triage — this is where the plan gets cheap

The brief already hands over two segments pre-triaged. Treat both as findings, not as "the corpus," and resolve each by its ROT category:

### Segment B — 120 auth snippets = **Redundant**

Policy decision #6 exists for exactly this: *"Keep auth setup in one place that other samples link to, rather than repeating it with variations."* Reconcile to one canonical, tested sample per (auth method × tier-1 language). Assuming API-key + OAuth across 5 languages: **10 canonical samples kept, ~110 deleted** and replaced with a link to the canonical page.

- These 10 sit on the primary adoption path (auth is step one for every reader), so they're forced to tier **Run**, unconditionally — this segment isn't subject to the ratio-ranked queue below.
- This single move removes ~110 samples from the corpus and eliminates the "small variations" defect class outright — every variation was a future divergence waiting to happen.

### Segment A — 700 API-reference one-liners = **not trivial-to-delete, but generate-don't-hand-test**

These look like ROT's "trivial" category (single API call, no scenario), but per-endpoint reference coverage is the expected contract of a reference page (Stripe, Twilio, AWS all do this) — cutting them costs endpoint discoverability, which readers do need. The lever here isn't deletion, it's **source of truth**.

If an API spec exists (confirm Q8): route these through verification pattern 3, **generate from spec** — per-language request/response snippets generated from the API description. This is the one pattern in `verification-patterns.md` built for exactly this shape: *"near-zero marginal cost per language"* for *"single calls on reference pages."* Hand-authoring 700 compilable files is the expensive path; generating them from the spec that already defines every endpoint is not.

- Default all 700 to tier **Compile-only** first (build/lint against the generated SDK call — ships in about a week, satisfies the near-date constraint).
- Promote idempotent reads (`get`, `list`, `head`) to tier **Run** against a sandbox project once fixtures exist, in the nightly integration sweep.
- Leave writes/deletes/billing calls at **Compile-only** permanently — Run has no tier for anything that bills, destroys, or calls a third party, whatever the pipeline can do.

If no spec-backed generation is feasible, fall back to pattern 1 (migrate into one compilable examples project, transclude by region tag) — more setup, same end tier assignment.

### Segment C — remaining ~580 samples

`1,400 − 110 (deleted duplicates) = 1,290` corpus after triage. `1,290 − 700 (Segment A) − 10 (Segment B) = 580` (Segment C): quickstarts, guides, tutorials, multi-step SDK examples. This is the segment that needs actual judgment — stratify by language × product area × age per the audit method, score a sample against the six-dimension rubric, and rank the rest by the fix-queue ratio.

## 3. Execution tier plan and the 80% math

| Segment | Count | Tier | Mechanism |
|---|---|---|---|
| Auth duplicates, deleted | ~110 | n/a | ROT-redundant, replaced with a link |
| Auth canonical | 10 | **Run** | hand-written, single-sourced, on adoption path — forced, not ranked |
| API one-liners | 700 | **Compile-only** now, **Run** for reads once fixtures exist | generated from spec |
| Guides/tutorials — forced subset | ~20–40 (est.) | **Run** | quickstart, first-call, error-handling, webhook-verification pages — adoption path, forced per baseline #2/#3 |
| Guides/tutorials — remainder | ~540–560 | ranked queue | (readers affected × severity) ÷ fix effort |

Target corpus after triage: **1,290 samples**. Baseline #4 requires ≥80% at tier Run or Compile-only: `1,290 × 0.80 = 1,032`.

Already covered by Segments A + B: `700 + 10 = 710`.

**Segment C needs 322 of its ~580 samples (≈55%) promoted to Run or Compile-only this year** to close the gap. That's the actual number behind the objective — not "test everything," a specific, sized remediation sprint. The forced adoption-path subset (~20–40) counts toward this 322 and is exempt from the ratio ranking; the rest of the 322 gets picked by (readers affected × severity) ÷ fix effort, using copy-button data where the docs platform exposes it — high-copy samples earn Run first.

Everything in Segment C not reached this cycle stays honestly labelled: **Illustrative** if it's a fragment/anti-pattern, or **unverified** (never "passing") if it's a real sample nobody has run yet. Silence is not a status — an untiered sample defaults to a claimed Run and fails loudly if CI doesn't back that claim.

## 4. The six policy decisions

1. **Coverage** — every tier-1 scenario named once, matching the parity-matrix identifier. Reference pages keep one-per-endpoint coverage (via generation, not hand-authoring). Guides prefer multi-step scenarios; no new single-call snippet gets hand-written once generation exists.
2. **Anatomy** — full imports, explicit client setup, one code path, expected output block, top comment stating what it does and what it assumes. Applies immediately to the 10 canonical auth samples and to every Segment C fix; generated Segment A snippets inherit anatomy from the generator template, set once.
3. **Safety** — script-enforced: no prompt characters, angle-bracket placeholders, no credential-shaped literals, no disabled TLS, destructive commands carry a guard. Run on 100% of the corpus every PR, not sampled.
4. **Parity** — 5 tier-1 languages (pending real telemetry), each scenario identifier identical across them, everything else free to be idiomatic. Compute coverage as `T cells ÷ (scenarios × tier-1 languages)` once the matrix is built from the post-triage inventory.
5. **Verification** — pattern 3 (generate from spec) for Segment A; pattern 1 (compilable source + transclusion) for Segments B and C; hermetic checks on every PR, integration sweep nightly against a dedicated sandbox project.
6. **Ownership and freshness** — see below. Freshness window: left to the team to set against release cadence (the skill deliberately doesn't supply a number); propose 90 days as a starting point tied to SDK release frequency, adjust after one cycle.

## 5. Verification design

- **Segment A (generated):** CI installs the released SDK package (never the working tree — a suite built from source passes on code no reader can install), runs the generator against the current spec, diffs output, fails the docs build on drift. Reads execute against a dedicated sandbox project nightly; writes/deletes stay Compile-only.
- **Segment B (canonical auth):** hermetic test per language, runs on every PR — auth setup is small and fast, no reason to defer it to a nightly sweep.
- **Segment C:** hermetic subset on PR, full integration nightly/weekly (samples break when the service changes, not when the docs repo changes). Dedicated test accounts per language, deterministic resource naming (`fernhill-test-<runid>-*`) so a scheduled sweeper can safely tear down what the test itself missed.
- Every job reports **per sample** (page, line, owner), not per suite — that's what makes a red build routable.

## 6. Ownership

One model per surface, not one for the whole corpus:

| Surface | Model | Fix SLA |
|---|---|---|
| API reference (Segment A, generated) | Product/platform engineers who own the spec — the generator makes docs-as-done automatic | same sprint as the API change that broke it |
| Auth canonical (Segment B) | Docs team, code-owner gated | this week (adoption-path sample) |
| Guides/tutorials (Segment C) | DevRel with named-expert review, backstopped by contracted capacity if the backlog grows — "community-maintained" is a funding model, not a free one | sprint, P0 same day |

Bug fixes and security issues (P0/P1) take priority over new sample authoring in every surface's queue — the rule teams skip, which is exactly how 1,400 samples accumulated with no owner.

## 7. Sequencing (migration order, cheapest step first)

1. **Weeks 1–2:** real census + secret scan (`scripts/sample-audit.py`), report any P0 immediately, don't wait for the full plan.
2. **Weeks 1–2, parallel:** auth dedup — kill ~110 duplicates, stand up 10 canonical Run-tier samples. Cheapest, highest-visible win.
3. **Weeks 3–8:** generation harness for Segment A. This is the single highest-leverage investment in the whole plan — it closes 700 of the 1,290-sample corpus (54%) for one build job's cost.
4. **Remainder of the cycle:** Segment C remediation sprint targeting the 322-sample gap, forced adoption-path subset first, then ratio-ranked.
5. **Ongoing:** code-owner gates, metadata schema validation, secret scanning wired as blocking checks — not advisory — so the 80% number doesn't erode next release.

## 8. Re-measurement

Re-run the census after each phase rather than declaring the objective met from the fix list:

- Zero P0, forced adoption-path samples 100% Run — checked continuously, not ranked.
- Compile/run pass rate ≥90% on whatever is currently tested; below that, stop new authoring and run remediation first.
- ≥80% of the 1,290-sample corpus at Run/Compile-only — the objective's actual gate, `1,032` samples.
- Parity coverage ≥90% across the 5 tier-1 languages once the matrix exists.

If Phase 3's harness underdelivers and Segment C can't reach 322 this cycle, cut lower-value Segment C samples rather than lower the bar — a deleted sample costs a reader nothing, an untested one that claims to work costs them an afternoon.

## 9. KPIs to track per release, not per audit

- Tested-sample share and parity coverage.
- Mean time from a red CI sample to a merged fix — the real test of whether the ownership table above is more than a document.
- Copy-button clicks per sample (if the docs platform exposes it) — feeds the Segment C ranking every cycle, not just this one.
- Sample age distribution — count exceeding the freshness window.
- Rubric score per dimension for the Segment C stratified sample, tracked over time; report the weakest axis, not the average.