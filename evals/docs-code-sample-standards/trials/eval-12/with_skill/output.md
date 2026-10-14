# Sample audit — Marlowe Freight docs tree, 2026-09-23

Method: Inventory-before-audit (NN/g) for the census, Docs-as-Tests (Silva) for verification design, Google Cloud's clarity → idiomaticity → consistency → simplicity → portability → maintainability ranking for any rule conflict. Named here so your team can check the source, not my word.

**Scope check:** this is a corpus audit, not a quickstart/tutorial/IA task — in scope for this skill.

🔴 **Before anything else — the 63 P0 from last quarter are neither confirmed nor cleared.** Don't file this report under either "63 P0 open" or "0 P0, false alarm." Correct status today: **63 flagged, 0 confirmed, adjudication pending on 2 pages** — see §2. That pending state is itself this quarter's finding, not a stall, and it's reported first because a real P0 outranks every efficiency ratio in the fix queue.

## 1. Environment ruling — what today's audit can and can't claim

Two different constraints, two different consequences:

- **No network/registry, no Java/Ruby/Go toolchain, only Python installed** → Run and Compile-only are both unreachable today, for all four languages, including Python (its samples likely import an SDK you can't `pip install` air-gapped). Per the skill's own tiering rule: *"A team with no CI has no Run tier and no Compile-only tier: every sample in that corpus is unverified until a pipeline exists."* Nothing below should be read as "passing" — only as "clean on mechanical lint, unverified on execution."
- **The mechanical census needs none of that.** `scripts/sample-audit.py` is stdlib-only (`argparse`, `json`, `os`, `re`, `sys`) and never executes a sample — it regexes the fenced text in your Markdown. It runs today, air-gapped, over all four languages, at 100% coverage:

```
python3 scripts/sample-audit.py docs/ --format summary
python3 scripts/sample-audit.py docs/ --format json --ext .md,.mdx > inventory.json
```

I read the script's source rather than running it against your tree in this session — I don't have your `docs/` directory mounted here. Run the command above and hand me (or your team) the output for the real census numbers; everything else in this report doesn't depend on it.

Two axes, kept separate for the rest of this report:

| Axis | Status today |
|---|---|
| **Target tier** (a policy claim: what a sample is supposed to be) | Assignable now, see §5 |
| **Verified status** (did anything actually execute) | **Unverified**, corpus-wide, until a toolchain + registry access exist |

An untiered fence defaults to *claiming* Run — the strictest tier — precisely so silence never reads as "fine." That's why every sample here gets marked unverified rather than quietly assumed passing.

## 2. The inherited 63 P0 — confirm, don't accept

Rule from the skill, verbatim: *"Treat every flag it raises as a candidate to confirm, not a verdict; a page about credential rotation legitimately contains a key-shaped string."* Your scanner almost certainly never applied that rule — it looks like it published every `SECRET`/`LONG_LITERAL`/`INSECURE` regex hit as-is. Run against two pages *built to contain exactly those patterns on purpose*, that produces a flood indistinguishable from a real breach. The team stopping reading was the rational response to an unconfirmed report, not a discipline failure.

Adjudication protocol — apply this to every one of the 63, not just the two named pages:

| Case | Verdict | Action |
|---|---|---|
| Real-format/live-shaped secret, **wherever it appears** — including on the rotation/hardening pages | **P0 — deliberate intent does not exempt it.** A reader skimming for "what the key format looks like" can still copy it. | Report the moment it's found, rotate/replace now |
| Key-shaped or `--insecure` example illustrating a concept, using an unambiguous non-functional placeholder (reserved-range, angle-bracket — not a real provider's live-key shape), under a visible "anti-pattern / do not run" label | **Not a P0.** | Tier Illustrative, add an Exceptions row (§3), leave as-is |
| Same pedagogical intent, but still shaped like a real key/host, or missing the visible label | **Real defect, wrong severity class.** Nothing here is a currently-copyable hazard — it's a labeling/format gap. | File **P1**, fix the placeholder format and add the label, no rotation needed |
| Any flag **outside** the two named pages | Default rule applies: assume real until shown otherwise | Confirm and report immediately, not batched |

I have not been given the actual text of the key-rotation guide or the security-hardening page in this session, so I'm not going to certify a number I haven't read line-by-line — that would be exactly the "trust the report" problem you're trying to fix. Pull just those two pages' flags and run the three-way split above:

```
python3 scripts/sample-audit.py docs/security/ --format json
```

Columns to work the confirmation sheet: `path:line | flag | pattern matched | real-format or reserved/placeholder | visibly labeled anti-pattern? | verdict`.

## 3. Why this keeps happening — governance, not a bigger scanner

*"A validator whose output nobody sees is documentation of a problem, not a gate against it."* Last quarter's 63-item list was a document, not a gate — nothing failed a check, nothing blocked a merge, so nobody was forced to triage it before it went stale enough to ignore.

Fix at the root:

1. Once §2's confirmation pass runs once, write **Exceptions** rows (policy template §8) for every confirmed-safe finding on those two pages: sample, rule waived, reason, **expiry date**. An exception without an expiry is a silent policy edit — don't leave one open-ended.
2. Wire the scanner to a **non-zero exit that fails a required check**, posted where reviewers actually see it (PR status, not a report nobody opens).
3. After that, next quarter's run only surfaces *new* flags — the two known pages stop regenerating the same 63.

## 4. Tier policy for the corpus (target, not verified status)

Given the environment, Run and Compile-only are deleted from what can be *enforced* today — that's a statement about this environment, not a permanent ranking. Target tiers, independent of today's execution capability:

- **Primary adoption path** (install, auth, first call) → target **Run**, whichever languages you name tier 1.
- **Anything billing, destructive, or hitting a third party** → target **Compile-only**, permanently — this doesn't change once the air gap lifts.
- **Key-rotation walkthrough, hardening anti-pattern demos** → target **Illustrative**, permanently, with a visible label. These are supposed to never run, air gap or not.
- **Everything else** → Compile-only once any toolchain exists, else Run.

## 5. Language parity — open question, don't guess it

Matrix shape is ready; I have no download/support-ticket data to tier Python/Java/Ruby/Go correctly. Per the parity rule, tiers come from *"actual download/telemetry share and support-ticket volume,"* never team preference — most corpora over-serve the language the API was written in and under-serve the one readers actually arrive with. Give me that data (or Q3 from the interview: which language do readers arrive with most?) before I fix tier 1 vs. tier 2 vs. community. Parity coverage (`T` cells ÷ scenarios × tier-1 languages) is uncomputable until the scenario list exists — send it and I'll fill the matrix.

## 6. Findings

- 🔴 **P0 — 0 confirmed, up to 63 pending** (§2). Top of the queue, ratio-exempt.
- 🟠 **P1 — corpus-wide execution status is unverified**, which is a disclosed fact, not an assumed pass. Every sample's real-world correctness is currently unknown.
- 🟢 **P2 — no fence carries `tier=` metadata anywhere in the corpus** (assumption pending your census run) — cheap to fix now, and it's what lets the eventual pipeline route each sample on day one instead of everything defaulting to the strict Run claim.

## 7. Fix queue (ratio-ordered below the P0 exemption)

1. **Confirm the 63 on the 2 named pages** — this week, owner: docs + security co-review. P0 candidates go first on harm, never on efficiency.
2. **Run the census command against the real tree**, get actual corpus size, language mix, flag counts — replaces every placeholder number in this report.
3. **Add `tier=` to every fence** — near-zero cost, unlocks routing before any pipeline exists.
4. **Stand up a compile-only gate**, Python first (your only local runtime), Go/Java/Ruby as toolchains get approved — compile-only catches the whole P1 class (removed symbols, missing imports) for no fixtures and no credentials.
5. **Write the Exceptions table** for the two named pages, each row with an expiry.
6. **Fill the six policy decisions** (coverage, anatomy, safety, parity, verification, ownership) into `sample-policy-template.md` and keep it in the docs repo, not a wiki.

## 8. Verification plan for when the air gap lifts

- **Source of truth, ranked**: extract-from-tested-source for anything on the adoption path (highest reliability, highest setup cost); test-what's-embedded elsewhere (cheapest to adopt on an existing corpus); generate-from-spec for single-call reference snippets only. Pick one per surface, write it into the policy — don't leave samples hand-pasted into prose, which belongs to none of the three.
- **CI shape**: install the *published* artifact, not the working tree. Split hermetic (every PR) from integration (scheduled — samples break when the service changes, not when the docs repo changes). Fail a required check, not an advisory one. Report per sample: which sample, which page, which line.
- **Fixtures**: dedicated test accounts per language, deterministic resource naming (prefix + run id), teardown in-test plus a scheduled sweeper, secrets from the CI store — never from the sample itself.

## 9. Pass threshold — read against the environment, not the corpus

| Gate | Reading today |
|---|---|
| Deprecated-API rate < 43% (Zhou & Walker, the one gate with a measured comparator) | Unmeasurable without execution |
| Zero P0, 100% adoption-path samples tier Run + green in CI | Fails by construction — nothing is in CI yet |
| Compile/run pass rate ≥ 90% | Unverified, not failing |
| ≥ 80% of corpus tier Run/Compile-only, CI-executed | 0% today, because of the environment, not the corpus — don't let this get misread as a quality verdict |
| Parity coverage ≥ 90% | Uncomputable without the scenario list |

None of these numbers judge Marlowe Freight's samples. They judge what this environment can execute today. Re-run every one of them the day a toolchain and registry access exist — that re-run is the real audit; this one is the honest placeholder for it.

## 10. What closes the loop

- The real `docs/` path, or the pasted output of the census command, for actual per-file numbers.
- Read access (or pasted text) to the key-rotation guide and the security-hardening page specifically, so §2 doesn't sit pending for another quarter.
- The scenario list, one row per identifier, to compute real parity coverage.
- Usage/telemetry data to set language tiers instead of guessing them.