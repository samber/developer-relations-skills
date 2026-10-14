# Testing 1,400 Fernhill Cloud Code Samples — Plan

## 1. The real shape of the problem

1,400 isn't one population — it's three, with wildly different test economics:

| Tier | Count | % of total | What it is |
|---|---|---|---|
| A — Reference one-liners | ~700 | 50% | Single API calls, one per endpoint |
| B — Auth snippet copies | ~120 | 8.6% | Same snippet, small variations, pasted across pages |
| C — Everything else | ~580 | 41.4% | Tutorials, quickstarts, multi-step guides, error handling |

Tier C is where real product risk lives — multi-step state, less-common paths, the samples someone actually copy-pastes into a working integration. Tiers A and B are high-count but low-complexity. Treating all 1,400 as one uniform testing problem wastes effort on Tier A/B and starves Tier C. Confirm the ~580 figure against the actual doc inventory before committing to numbers below — it's inferred, not given.

## 2. Definition of "tested" (lock this first)

Before any work starts, get sign-off on what counts, or the 80% number will be contested later:

- **Executable sample** (Tier A, most of Tier C): runs against a sandbox tenant on the current SDK version, exits without error, response matches documented shape.
- **Non-executable sample** (config files, CLI output, UI screenshots): manually walked through against current product behavior by a human, dated.
- Every tested sample stores: pass/fail, SDK version tested against, date. Without this metadata, "80% tested" is a snapshot that silently rots the next time the SDK ships a breaking change.

## 3. Tier A — 700 one-liners: automate, don't hand-test

- Build a snippet-extraction harness that pulls code blocks straight from the docs source (markdown/MDX), not the rendered site, so it can run in CI pre-deploy.
- Stand up a disposable sandbox tenant per language/SDK with seed fixtures (dummy bucket IDs, keys). For destructive one-liners (`delete`, `revoke`), generate the resource immediately before the call, don't run them against shared state.
- Assert: no exception, expected status code, response schema matches what the page claims.
- Wire the harness into doc-build CI once, so it re-verifies on every deploy — this is what keeps the 80% figure from decaying the day after you hit it.
- **Cost shape**: high upfront (harness + fixtures), near-zero marginal per sample. Expect ~2-3 weeks for one engineer to cover most CRUD-style endpoints.

## 4. Tier B — 120 auth copies: dedupe before you test

Don't execute 120 near-identical scripts. Diff first:

1. Cluster all 120 by language + auth flow (OAuth exchange, API key header, JWT refresh). Expect ~5-10 real clusters.
2. Pick one canonical sample per cluster, test it for real against the sandbox.
3. Diff every other member of the cluster against its canonical version. Anything that differs beyond known-safe variables (placeholder IDs, formatting) gets flagged for manual review.

That last step is the actual point of this tier: the risk in 120 copy-pasted snippets isn't "does auth work," it's "did someone's copy-paste silently drift" — a stale field name, an outdated scope, a token format nobody updated. The diff catches that; re-executing 120 identical scripts doesn't. Budget 3-4 days including outlier triage.

## 5. Tier C — 580 substantive samples: this is the real work

Don't work this queue in page order. Rank by risk signal, pulled from data, not guessed:

- Doc analytics: top pages by traffic and by support-ticket citation
- Recency: samples untouched since the last API surface change
- Blast radius: destructive/stateful/billing operations rank above read-only examples
- Anything already flagged in a support ticket or GitHub issue

Test in that order. Script the API calls where possible; for the non-scriptable parts (UI steps, CLI output, screenshots), a human walks through against sandbox. Where the budget runs out, the *untested remainder is the lowest-risk tail by construction* — that's the point of ranking first, and it's a defensible thing to say to leadership if you don't clear all 580.

## 6. The 80% math, and how to report it honestly

- 80% of 1,400 = 1,120 samples.
- Tiers A + B fully done = 820 (58.6%) — real testing, not padding, but not sufficient alone.
- The remaining 300 have to come out of Tier C: **~52% of the 580 substantive samples**, not the easy ones.

That's actually good news buried in the math: the target can't be hit by automating only the cheap tiers. You're forced into meaningful Tier C coverage regardless.

Still, don't publish one blended number. Present two, side by side, in every status update:

- **Overall: X / 1,400 tested** — satisfies the objective as written.
- **Tier C only: Y / 580 tested** — shows whether the content most likely to break, and costliest when it does, actually got covered.

Flag this framing to whoever owns the objective *before* the first report, not after. A single 80% figure invites the reading "they tested the easy stuff and stopped" even when that's not what happened — the tier breakdown heads that off at zero extra cost, since the classification is already required to build the plan.

## 7. Infrastructure (build once, reuse everywhere)

- Disposable sandbox tenant per environment/language, safe to hit repeatedly, including destructive calls
- Snippet extraction from doc source, not rendered HTML
- Containerized per-language runners pinned to the SDK version the docs claim to document
- Sandbox credentials in CI secrets, never echoed into test logs
- Retry/backoff policy — 700+ one-liners hitting live endpoints will trip rate limits without one

## 8. Risks that could blow up the estimate

- **Tier C composition is assumed, not confirmed.** If a chunk of the 580 needs enterprise/paid-tier features unavailable in sandbox, cost jumps — verify before committing to a timeline.
- **Doc source inconsistency.** If code blocks aren't cleanly tagged by language/page, extraction tooling costs more than planned.
- **SDK drift.** A passing sample today can silently fail on the next SDK major bump. The CI hook must stay wired in permanently — this is not a one-sprint deliverable, or "80% tested" is true for exactly one day.
- **Tier B dedup assumption.** If the "small variations" turn out to be semantically meaningful per page rather than copy-paste drift, Tier B's cost roughly triples — the first diff pass validates or kills this assumption fast, do it early.

## 9. Timeline (one dedicated engineer)

| Weeks | Work | Milestone |
|---|---|---|
| 1-2 | Build Tier A harness + fixtures; start Tier B diff in parallel | — |
| 3 | Finish Tier B triage; wire Tier A into CI | 820 tested (58.6%) |
| 4 | Pull analytics/support-ticket data; rank Tier C | — |
| 5-10 | Work Tier C queue in priority order | +300 tested (target: 1,120 / 80%) |

Roughly 2.5-3 months end to end. Tier A/B don't parallelize past one engineer building the harness; adding headcount only helps once Tier C ranking is ready — put extra hands there.

## 10. Do this week

1. Pull the actual doc inventory and confirm the ~580 Tier C count — don't build the plan on an assumption.
2. Provision sandbox/test-tenant access now — this is usually the real bottleneck, not the harness code.
3. Pull page analytics + support-ticket citations to rank Tier C before writing a single test against it.
4. Get sign-off on the "tested" definition (§2) and the two-number reporting format (§6) before the first status update.