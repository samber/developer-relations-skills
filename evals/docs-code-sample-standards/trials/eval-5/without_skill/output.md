# Kestrel Analytics — Docs Sample CI Redesign

## Root cause (not "flaky CI")

Four separate failures got collapsed into one job, then masked with `continue-on-error`:

1. **Resource lifecycle bug** — test projects created by sample runs aren't torn down, so a shared quota fills up over time. This is a leak, not flakiness.
2. **No isolation between infra failures and sample failures** — a quota error and a genuinely broken code sample produce the same red X, so nobody can tell them apart from the CI log alone.
3. **`continue-on-error` deleted the signal** — for four months the job's result has meant nothing. It's not a flaky gate anymore, it's a job that always reports green regardless of outcome. That's why readers keep hitting broken samples that CI "passed."
4. **Wrong trigger, wrong blast radius** — building the whole SDK from the monorepo working tree and running all 240 samples on every PR to a *docs* repo means a docs typo fix pays the full cost (and quota risk) of a full SDK-vs-samples regression run.

Fix the leak and split the concerns. Don't tune retries around a broken signal.

## Design goals

- A red CI check must always mean "something is actually broken," never "infra hiccup."
- Docs PRs get fast, cheap, relevant feedback — not a 240-sample gauntlet for every typo.
- Quota exhaustion becomes structurally impossible, not something absorbed by wider limits.
- Readers get a **live, sample-level** signal independent of the PR-gate job, since the two audiences (PR author, reader hitting a broken snippet) need different things.

## Architecture

### 1. Fix the leak first — before anything else

- Every sample run creates its ephemeral test project with a TTL and an owning-run label.
- Add a scheduled cleanup job (hourly) that deletes any staging project past its TTL, independent of whether the run that created it succeeded, crashed, or was cancelled.
- Add a quota-usage metric/alert at 70% so this becomes visible weeks before it becomes an outage again.

This alone likely resolves the March incident. Everything below is about making sure a *class* of incident like it can't silently persist for four months again.

### 2. Split one job into three, by cost and audience

| Stage | Trigger | Scope | Budget |
|---|---|---|---|
| **Lint/compile gate** | Every PR to docs repo | Static check: does each sample parse/typecheck against the SDK's public API surface (no staging calls) | seconds, no quota use |
| **Smoke gate** | Every PR to docs repo | Run only the samples touched by the diff (path-filtered), plus a fixed core smoke set (~20 samples covering auth, init, most-copied snippets) | minutes, small quota footprint |
| **Full matrix** | Nightly + on-demand label (`run-full-samples`) | All 240 samples, sharded across parallel runners | full run, off the PR critical path |

A docs typo fix never touches stage 3. A change to a shared code fence or SDK version bump can be forced through the full matrix with the label.

### 3. Make the PR gate a real gate again

- Remove `continue-on-error` from the smoke gate. It blocks merge on real failure.
- Classify failures at the step level: infra error (staging 5xx, quota, timeout) vs assertion failure (sample output wrong). Infra errors get one automatic retry with backoff; assertion failures never retry — retrying a real bug just hides it.
- If a step can't classify itself confidently (e.g., ambiguous staging error), it fails loud rather than defaulting to pass. Ambiguity should never resolve to green.

### 4. Give the full matrix a flake-quarantine lane, not a mute button

- A sample that fails intermittently across 3 consecutive nightly runs gets auto-labeled `quarantined` and moves to a separate tracked list — visible in the nightly summary, not deleted from it.
- Quarantine has an owner and an age. Anything quarantined >14 days pages the docs-eng owner; it doesn't get to sit invisibly the way this job did for four months.
- Quarantining a sample never means "stop testing it" — it keeps running, just doesn't block merges while under investigation.

### 5. Give readers a signal independent of the PR pipeline

This is the part the old design never had, and it's why issues kept coming in from readers even though "CI" existed.

- Publish a per-sample status badge (last-verified timestamp + pass/fail) generated from the nightly full-matrix run, rendered next to each sample in the docs site.
- If a sample has been failing for >48h, show an inline "known issue, tracked in #NNN" banner instead of a silently stale example — this is strictly better than what readers get today (a confidently rendered, quietly broken sample).
- Auto-file (not just label) a GitHub issue the first time a sample enters quarantine, linked from that banner, so "readers still open issues" becomes redundant rather than the only detection mechanism.

### 6. Decouple SDK build from sample execution

- Build the SDK from the monorepo once per relevant SDK change (not per docs PR), publish it as a versioned artifact/package to an internal registry.
- Docs-repo pipelines consume that pinned artifact instead of rebuilding the SDK from a live working tree on every run. This removes a whole class of "docs PR fails because monorepo tip is broken" noise, and makes it explicit which SDK version each sample run is actually validating.

## Rollout

1. Ship the cleanup job + quota alert alone, same day — stops the bleeding.
2. Split lint/smoke/full-matrix stages; keep `continue-on-error` off only on the new smoke gate initially.
3. Turn on failure classification and quarantine lane once a week of nightly data exists to validate the retry/quarantine thresholds.
4. Ship the public per-sample status badge last — it's the most visible change and should launch once the underlying signal is actually trustworthy, not before.
5. Remove `continue-on-error` repo-wide once stages 2–3 have run clean for a week.

## What this gets Kestrel

- PR authors get a fast, trustworthy gate instead of a 240-sample lottery.
- The quota incident class is closed structurally (TTL + cleanup + alerting), not papered over.
- Readers stop being the detection mechanism for broken samples — the nightly matrix + status badges catch it first.
- "CI is green" goes back to meaning something, which is the actual prerequisite for ever re-enabling it as a merge gate.