# Sample audit — Northbeam SDK docs (Python / TypeScript / Go / Java / Ruby), 2026-09-23

Scope check: this is a sample-corpus audit plus a parity/verification plan, not a quickstart or tutorial rewrite — in scope for this skill. Methods applied: **inventory-before-audit** (NN/g) for the census, **ROT triage** as the first cut on the messy languages, **Docs as Tests** for the verification design, and **Google Cloud's ranked sample principles** to settle the Go/Java rewrite question below.

One honesty note before the numbers: there's no real Northbeam docs tree in this environment, so `scripts/sample-audit.py` never ran against actual files. Everything below is built from the counts you gave me. Treat it as a **reported** census, not a **measured** one, until someone points the script at the real corpus — that's item 0 in the queue.

## 1. Where you actually stand

Structure: 6 documented scenarios × 5 languages = 30 possible samples. Python, TypeScript, Go, Java are the committed (tier-1) surface — 24 cells. Ruby is Community — 6 cells, held to a different bar by design.

| Language | Tier | Tested & green in CI | Gap | Tested share |
|---|---|---|---|---|
| Python | Tier 1 | 6 / 6 | 0 | 🟢 100% |
| TypeScript | Tier 1 | 5 / 6 | 1 | 🟢 83% |
| Go | Tier 1 | 4 / 6 | 2 | 🟠 67% |
| Java | Tier 1 | 2 / 6 | 4 | 🔴 33% |
| Ruby | Community | 1 / 6 | — | ⚪ not gated |

**Parity coverage (tier-1 only)** = T-cells ÷ (scenarios × tier-1 languages) = (6+5+4+2) / (6×4) = **17/24 = 71%**.

That's one number, and it's the one to repeat externally — not "Go and Java are behind," which is true but not recomputable next quarter.

### Gate scorecard against this skill's pass thresholds

| # | Gate | Status | Verdict |
|---|---|---|---|
| 1 | Deprecated-API rate < 43% (measured comparator, Zhou & Walker) | Unknown — needs the actual scan | ⚠️ can't score |
| 2 | Zero P0, 100% of primary-adoption-path samples tier Run + green | Unknown which of the 6 scenarios *is* the adoption path | ⚠️ can't score, but if it's one of Java's 4 gaps, this fails automatically |
| 3 | Compile/run pass rate ≥90%, else stop new authoring | Can't compute without the P (exists, untested) vs M (doesn't exist) split on the 7 gap cells | ⚠️ can't score — see §5 |
| 4 | ≥80% of the governed corpus tier Run/Compile-only, CI-executed | 17/24 = 71% | 🔴 fail |
| 5 | Parity coverage ≥90% across tier-1 languages | 71% | 🔴 fail |

Two of five gates fail outright on the numbers you have; two more can't be scored yet because "everything else exists untested or does not exist at all" conflates two different problems with two different fixes. Resolving that split is the first thing to do, before anything gets rewritten.

## 2. Fix the plan before you fix the code

The plan as stated — Go and Java as line-for-line equivalents of Python, same variable names, same helper structure, diff showing nothing but syntax — violates this skill's own tie-breaker. Google Cloud's ranked principles are **clarity → idiomaticity → consistency → simplicity → portability → maintainability**, higher wins on conflict, and this plan explicitly optimizes for consistency at idiomaticity's expense. That's the one trade the policy says never to make.

Concretely:

- The parity definition in this skill is explicit: parity is "never about line-by-line translation." What must match across languages is the **sample identifier, the scenario, and the step order** — not the code shape. "A Python sample that reads like transliterated Java satisfies a diff and fails the reader" is the exact anti-pattern description, and it's your plan.
- The rubric scores idiomaticity 0/2 for "transliterated from another language" and 2/2 for "reads as a native of the language." A deliberate port that keeps Python's variable names and helper structure will land Go and Java at 0/2 on that axis by construction — across every one of the six scenarios, in two entire language surfaces, on day one of the rewrite. You'd close the coverage gap and open a corpus-wide idiomaticity gap in the same motion.
- Idiomatic shape genuinely differs where it matters: Go's explicit `if err != nil` per call vs. Python's implicit exception propagation, Java's checked exceptions and try-with-resources vs. Python's context managers, Go's short receiver names and exported-identifier casing vs. Python's snake_case. Forcing identical helper structure means either fighting the language or writing bad Go/Java on purpose.
- The failure-modes table in this skill names your exact symptom and its fix: "Same scenario behaves differently per language → Fix parity by scenario and outcome, not by translating one language's code."

What you actually want, restated correctly: a reader who switches from the Go page to the Java page recognizes the same scenario, in the same order, tested the same way. That's real, it's achievable, and it's most of what "diff shows nothing but syntax" was reaching for — just applied at the scenario/anatomy layer, not the variable-name layer.

Split the thing you were calling "consistency" into two layers, because one of them is correct and one isn't:

- **Anatomy — should be identical.** Every sample has the same skeleton: imports, explicit client setup, top comment stating what it does and what it assumes, one code path, expected output. This part of your instinct was right; it's just anatomy, not syntax.
- **Idiom — must not be identical.** Naming, error handling, helper structure. Native to each language, reviewed by a native speaker of that language, formatted by that language's own formatter.

Practical instruction for whoever does the Go/Java work: use the excellent Python samples as the **extraction source of truth for scenario and step order only** (verification-patterns.md's "extract from tested source" pattern), then have a Go engineer and a Java engineer author each sample natively against that scenario list — not translate the Python file. Review against the idiomaticity rubric, target 2/2 ("reads as a native of the language"), not 1/2 ("passes the formatter").

## 3. The six policy decisions, filled in from what you've told me

| # | Decision | Answer |
|---|---|---|
| 1. Coverage | 6 scenarios, all 4 committed languages must cover all 6 at tier 1. No new (7th) scenario gets authored in any language until the 24-cell matrix clears the 90% gate — you're below it, so this is a remediation sprint, not a backlog item. |
| 2. Anatomy | Standard checklist, applied uniformly per language: imports, client init with reuse/cleanup note, top comment, one code path, expected output block, language identifier on the fence. |
| 3. Safety | Standard checklist. Run this as a P0 sweep on Go and Java *first*, independent of the rewrite — "a mess" and "worse than a mess" are exactly the conditions under which a real-format credential or a disabled-TLS switch hides. |
| 4. Parity | Tier 1: Python, TypeScript, Go, Java. Community: Ruby. Identical across tier 1: sample identifier, scenario, step order, testing approach. Free to differ: everything else. |
| 5. Verification | Extract-from-tested-source for all 4 tier-1 languages — Python already proves this pattern works here; TS and Go partially do. Don't invent a second pattern for Java; match what's already validated. |
| 6. Ownership | Per-language owner with a fix SLA, product-engineers-co-own model (fits an SDK/library product — see §5). Ruby: community model, explicitly labelled unowned, no SLA, no coverage obligation. |

One thing not yet decided and not derivable from what you've told me: which of the six scenarios sits on the primary adoption path (first connect/auth, first read, first write). That scenario must hit tier Run, green, in all four committed languages regardless of where it lands in the effort-ranked queue — pin it down before ranking the rest.

## 4. Language tiers

Python / TypeScript / Go / Java = Tier 1, on the strength of "the four languages we commit to." That's a legitimate basis for a tier decision, but the matrix guidance is specific that tiers should come from download/telemetry share and support-ticket volume, not team preference — "most corpora over-serve the language the API was first written in... and under-serve the one most readers arrive with." Worth a one-time check: if Go turns out to be the highest-arrival language among the four, its "mess" is more urgent than its ranking-by-effort below suggests, and that should override the ratio.

Ruby = Community: present, unowned, labelled untested, no SLA. Make sure the rendered docs actually say that on the page — an unlabelled community sample reads as neglect, not as a stated decision.

## 5. Ranked fix queue

**0 — this week, before anything else.** Two things that cost nothing to start and that everything else depends on:
- Point `scripts/sample-audit.py` at the real docs tree. This resolves the P-vs-M split on the 7 gap cells (1 TS, 2 Go, 4 Java) — right now "untested" and "doesn't exist" are two different fixes wearing the same label, and you can't size the Java/Go work without separating them.
- Secret scan + destructive-command sweep on Go and Java specifically. P0 goes first on harm, not on efficiency, and "worse than a mess" is the description of a corpus that hasn't had this check run on it.

**1 — Go, once the census confirms the split.** Smallest gap (2 of 6), and a CI harness for the other 4 already exists — extending it is cheap. Two moves in parallel: (a) fill the 2 missing/untested scenarios using Python's scenario/order as the source, written natively; (b) re-review the 4 already-green Go samples against the idiomaticity rubric — "a mess" while passing CI usually means duplication or non-idiomatic shape that compiles fine, i.e. an ROT problem (redundant/trivial) as much as a coverage one. Triage those before touching them further.

**2 — Java, same census gate, higher effort.** 4 of 6 missing/untested, and unlike Go there's no confirmed existing harness to extend cheaply for the new ones. Same rule: author natively from Python's scenario/order, review to 2/2 idiomaticity, don't transliterate. This is the one that "needs a whole harness built" that the effort ratio in this skill explicitly says to starve by default — promote it by hand anyway if the adoption-path scenario turns out to be among its gaps (§3, open item).

**3 — TypeScript's single gap.** Smallest, cheapest, no reason to defer once Go/Java work is moving — same team can likely close it in the same sprint as item 1.

**4 — Re-run the audit, recompute the 71%.** Not "declare victory from the fix list" — the skill is explicit that the number gets re-measured after each batch, not assumed.

Ruby stays out of this queue entirely; it's community-owned and the policy's job there is the label, not the fix.

## 6. Governance

Product-engineers-co-own fits Northbeam better than a dedicated examples team or a vetted-community model — it's a single SDK/library product, not a catalogue, and Stripe's version of this model ("docs part of the definition of done") is the closest real-world match. Assign one named owner per committed language with a fix SLA; write "bug fixes and sample breakage outrank new sample authoring" into the policy explicitly — that's the rule teams skip, and it's exactly how a corpus gets to "Java is worse than a mess" in the first place.

Migration order, since none of this is standing infrastructure yet for Go/Java: census + secret scan (item 0) → automated compile/run gates on whatever's already testable → move samples into compilable, tagged source published by transclusion (this is the actual fix for "mess" — hand-pasted Markdown is what produces drift) → metadata + schema validation → code owners + SLAs. Don't assign owners before the samples live in compilable source; that's responsibility with no mechanism to discharge it.

## 7. What's still open

- The P/M split on the 7 gap cells (blocks sizing Go vs. Java precisely).
- Which scenario is the primary adoption path (blocks the 100%-Run gate and the queue override).
- Real usage/telemetry to confirm the tier-1 set, not just the "we commit to" statement.
- Freshness window (deliberately left to the team in this skill — pick one before the parity matrix goes stale silently).
- Whether Java's 2 currently-green scenarios are actually clean, or green-but-low-quality — the rubric scoring pass (§audit-method, stratified sample) hasn't run on them yet.

Re-run this audit for real once the script has an actual corpus to point at — everything above is a plan built on your reported numbers, not a verified one.