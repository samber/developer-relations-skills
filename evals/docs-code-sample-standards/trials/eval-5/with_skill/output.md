# Kestrel Analytics — Documentation Sample Pipeline Redesign

Following `docs-code-sample-standards`. Two artefacts below: the diagnosis (why the current job produced four months of red-but-ignored CI while readers still hit broken samples), and the redesigned pipeline. Three named methods carry the redesign — **Inventory before audit** (NN/g) for the corpus census, **Docs as Tests** (Manny Silva / Go blog) for why execution belongs in CI at all, and **Google Cloud's ranked sample principles** wherever two rules collide.

Six inputs the interview would normally pin down aren't in your message — language mix, real per-sample census numbers, current freshness window, current ownership model, whether the monorepo build already produces a versioned/published SDK artifact, and deprecated-API rate. Each is flagged inline as **[NEEDS INPUT]** rather than invented; do not let a placeholder in this doc read as a measured number.

## 1. Diagnosis

Your one job conflates four separate failure modes the skill tracks separately. Fixing the CI job without fixing all four just moves where it breaks next:

| Symptom | Root cause | Skill's name for it |
|---|---|---|
| CI went flaky in March | No fixture teardown; leftover test projects accumulated until they hit a quota | "Untended fixtures are the usual reason a sample suite gets disabled six months in" |
| Set to `continue-on-error`, red for 4 months, nobody acted | The red mark stopped blocking anything, so it stopped being information | "Docs build stays green while samples are red → the red mark gets ignored" |
| Readers open issues even when the job *was* green | Builds the SDK from the monorepo **working tree**, not what a reader actually installs | "Samples pass CI but fail for readers → CI installs from the working tree; install the published artefact instead" |
| All 240 samples run the same way on every PR against staging | No execution-tier split — everything defaulted to the most expensive, most fragile claim (`Run`) regardless of whether that sample needed it | Untiered corpus; "Split hermetic from integration" never happened |

These compound: because the job was untiered, *every* doc PR — including a typo fix — paid the full cost of 240 live staging calls. Because there was no fixture hygiene, that volume burned quota. Because it was `continue-on-error`, the resulting flakiness never surfaced as a blocking signal, so nobody fixed the fixtures. And underneath all of it, even a fully green run was testing the wrong artifact, so readers kept filing issues the whole time. Four separate defects, one visible symptom.

## 2. Execution-tier reclassification (replaces "run all 240 the same way")

Default every sample to `Run` until reclassified — untiered samples default to `Run`, not to skip, so silence is never an excuse. Then move samples down deliberately:

- **Stays Run:** the primary adoption path — install, auth, first call — and any other sample whose "copy button" telemetry shows high paste volume. Skill baseline: **100% of primary-adoption-path samples tier Run and green**, non-negotiable.
- **Moves to Compile-only:** everything else that isn't destructive, doesn't bill, and doesn't depend on a third party's own rate limits. Compile-only catches the entire P1 class — removed symbols, missing imports, renamed methods, code that never compiled — for zero fixtures and zero quota. This is almost certainly most of your 240; a reference-page sample demonstrating one API call rarely needs a live staging round-trip to prove it isn't broken.
- **Forced to Compile-only regardless of traffic:** anything that bills, destroys a resource, or calls a third-party dependency Kestrel doesn't control.
- **Illustrative, excluded from CI, visibly labelled on the page:** pseudocode fragments and deliberate anti-patterns, if any exist in the 240 — audit will surface these.

Run `scripts/sample-audit.py docs/ --format summary` (from the skill) against the real corpus to get the actual census — file, language, tier-as-currently-marked, flags. **[NEEDS INPUT: real output]** — the split above is the *rule*, not a guess at your actual counts; don't publish a Run/Compile-only ratio until the script has run against the real tree.

## 3. Redesigned pipeline: two lanes, not one

The current design tries to be both a doc-PR gate and a staging-integration monitor in one job. Split it — this is verification-patterns.md's CI shape applied directly:

### Lane A — Hermetic (blocks every docs PR, fast, no quota risk)

```yaml
on: pull_request

jobs:
  hermetic:
    steps:
      - install: <published SDK artifact>   # last released version, or the
                                             # PR's own release-candidate build
                                             # if the PR is inside the SDK repo —
                                             # never the raw monorepo working tree
      - run: sample-audit.py docs/ --format json > lint.json   # mechanical
                                                                # census + secret scan,
                                                                # 100% of corpus, every PR
      - fail-if: lint.json has any credential-shaped-literal / insecure-switch /
                 destructive-command flag                       # P0s block immediately
      - for-each-sample-tier(run, compile-only):
          build-or-typecheck-only                                # no execution, no network
      - required-check: true
      - continue-on-error: false
```

This is what a prose-only doc PR (typo fix, wording change) actually touches — it never reaches the network, never spends quota, and its failure blocks the merge instead of being ignored. This is also what closes the "readers still see broken samples" gap: it installs the artifact a reader actually gets, not the in-progress working tree.

### Lane B — Integration (scheduled, isolated fixtures, owns the quota)

```yaml
on:
  schedule: nightly                     # decoupled from doc-PR volume entirely —
                                         # samples break when the *service* changes,
                                         # not when someone fixes a typo

jobs:
  integration:
    env:
      project: kestrel-docs-ci-<run-id>  # dedicated, deterministic-prefixed
                                          # project per run — never a shared
                                          # human/staging account
    steps:
      - install: <published SDK artifact>
      - for-each-sample-tier(run):
          execute-against-staging
          assert-documented-output
      - teardown: delete resources matching prefix    # in-run cleanup
      - always: sweep resources older than 24h matching kestrel-docs-ci-*
                                          # runs even if the suite crashed —
                                          # this line alone is the March fix
      - report: per-sample pass/fail, routed by code-owner, not one giant
                red job
      - block-docs-publish: only if a *primary-adoption-path* sample failed
                                          # a broken quickstart sample halts
                                          # publish; a broken secondary
                                          # reference sample files a ticket
```

The quota incident happened because nothing owned cleanup independent of the suite's own success — a suite that dies mid-run leaks by construction. `always: sweep` decouples teardown from the suite's exit code, and a dedicated per-run project with deterministic naming means the sweeper can identify orphaned resources safely instead of guessing.

## 4. Ownership — the Friday-afternoon question, answered concretely

Pick the model per surface, not once for the whole corpus:

| Surface | Owner | Fix SLA |
|---|---|---|
| Quickstart / auth / first call (tier Run, primary path) | SDK/product engineering — the team that ships the endpoint | Same business day; blocks docs publish, so this is forcing, not aspirational |
| Reference samples, tier Run (secondary) | Docs engineering triages, routes to the owning product team via code-owners | 5 business days (one sprint) |
| Compile-only samples | Docs engineering | 5 business days — usually a one-line import/symbol fix |
| Illustrative | Nobody; excluded from CI | N/A |

Encode this as `CODEOWNERS` on the sample directories, not a wiki page — a PR touching a tier-Run sample can't merge without that surface's owner. Write the AWS rule into the policy explicitly: **red-sample fixes and secret leaks outrank new sample authoring**, every time — this is the rule Kestrel's four months of silence violated.

## 5. Rollout order — do not flip the switch in one step

Reversing steps 2 and 5 below (assigning owners before fixtures are stable, or blocking before the quota leak is fixed) is the documented failure pattern — it just produces a second red, ignored job.

1. **Census + secret scan now, reported immediately.** Run the script against the real 240 today; any P0 (credential-shaped literal, destructive command) gets reported the moment it's found, not bundled into this rollout.
2. **Fix the fixture leak first.** Dedicated CI-owned project(s), deterministic naming, in-run teardown, always-on sweeper. Nothing downstream is trustworthy until this lands — re-enabling a blocking gate on top of leaking fixtures just recreates March.
3. **Stand up Lane A (hermetic) as a required check**, installing the published artifact, `continue-on-error` removed. This alone fixes the "readers see broken samples despite green CI" complaint, independent of anything else.
4. **Tier the 240 samples** using the script's `tier=` fence metadata plus judgement, primary-path samples locked to Run.
5. **Stand up Lane B (integration) on a nightly schedule**, decoupled from PR events, blocking publish only for primary-path failures.
6. **Assign code owners and SLAs** now that both lanes are stable enough to own.
7. **Re-measure against §6 below and report honestly** — a baseline, not a victory lap, on the first pass.

## 6. Thresholds to hold afterward (skill baselines — override with Kestrel's own data where noted)

- Zero P0s at all times; **100% of primary-adoption-path samples tier Run and green in CI**.
- Compile/run pass rate ≥90% — below that, stop authoring new samples and run a remediation sprint before anything else.
- ≥80% of the corpus tier Run or Compile-only and CI-executed (not hand-verified, not "looks fine").
- Deprecated-API rate: if the census turns up above 43% of samples referencing removed/deprecated calls (Zhou & Walker, FSE 2016 — the one gate here with a measured comparator), build a deprecation-detection check before anything else in this backlog. **[NEEDS INPUT: real rate]**
- Freshness window (max age of a sample's last *successful* Run before the page flags it stale): tie it to your SDK release cadence rather than a fixed number — this is deliberately a Kestrel decision, not a skill default. **[NEEDS INPUT: pick this explicitly and put it in the policy; an unset freshness window is how a corpus goes stale silently]**
- Parity coverage ≥90% across tier-1 languages, once language tiers are published. **[NEEDS INPUT: language mix / download telemetry to set tier-1 list]**

Track KPIs per release, not per audit: tested-sample share, mean time from red sample to merged fix (the actual measure of whether ownership holds), issues quoting a sample verbatim, and sample age distribution against the freshness window. Report the rubric per dimension, not a single total — a total would have hidden that this corpus's specific failure was fixture hygiene and artifact provenance, not sample quality on the page.