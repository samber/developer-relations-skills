No industry body publishes agreed-upon percentages for these three specific things, and I'm not going to hand you fabricated numbers with fake citations to put in front of your board — that's a real risk here worth flagging before the deliverable itself. Here's the honest state of the field, organized the way you asked:

## 1. "% of code samples that should compile/run"

**No standards body (ISO, IEEE, W3C) publishes a number for this.** What exists instead is a *practice*, not a benchmark:

- **Doctest-style enforcement** is the closest thing to a real convention: Rust's `rustdoc --test` runs every fenced code block in doc comments by default and fails the build on error; Python's `doctest` module does the same; Elixir's `ExUnit.doctest` likewise. Companies that adopt this tooling are implicitly enforcing **100% compile-clean**, because a broken sample fails CI — there's no partial-credit number, the target is binary (all green) or the build doesn't ship.
- Companies known for rigorous docs engineering (Stripe, Twilio, Google's API docs team) run example code through CI as part of their docs pipeline, but none of them have published a target percentage — it's "all examples are tested" as an internal engineering policy, not a benchmarked external figure.
- If your slide needs a number, "100% of published code samples pass automated compilation/execution in CI" is the only defensible bar, and the source for that claim is *tooling behavior* (rustdoc, doctest, CI gating), not a research study.

## 2. "% of code samples covered by tests"

Same issue. There's no published research or standards document establishing a benchmark like "70% of doc examples should have accompanying tests." The nearest analogue people sometimes reach for is **general code coverage conventions** (e.g., SonarQube's default quality-gate threshold of 80% line coverage on new code), but:

- That's a coverage target for **production code**, not documentation samples specifically.
- It's a **tool default**, not an empirically derived industry standard — SonarQube picked 80% as a reasonable heuristic, not from a study establishing it as "the" number.

Using it on a slide as "the industry standard for docs" would be misapplying a production-code convention to a different artifact class. I'd avoid stating it as a docs benchmark.

## 3. "Accepted cross-language parity number"

I don't have this. I searched my own knowledge and couldn't find a metric by this name published anywhere — no standards body, no major docs-tooling vendor (Mintlify, ReadMe, Docusaurus), and no DX/DevRel research (Write the Docs surveys, the Good Docs Project) defines or benchmarks "cross-language parity" as a percentage. It's possible you're thinking of something specific — e.g., "every code sample should exist in every SDK language we support" — but that's a policy choice teams set for themselves, not an externally accepted figure. I don't want to invent a number here.

## What I'd actually put on the slide

Given there's no third-party standard to cite, the credible move for a board deck is to **present it as your team's own bar, not an external benchmark**:

- "100% of code samples pass automated CI (compile/execute) — enforced via [rustdoc/doctest/CI gate], not a claim of industry consensus."
- Drop the "% covered by tests" framing — fold it into the same 100%-CI-gated bar, since a sample that's compiled-and-executed in CI *is* tested.
- Drop "cross-language parity" unless you can define precisely what you mean (e.g., "N of N supported SDK languages have this example") — then report it as a coverage ratio against your own SDK matrix, sourced internally, not as an industry figure.

Note: I did not access `skill-wip/` for this, per your instruction — nothing above depended on it. Also, my WebSearch tool wasn't authorized in this session, so the above is from my own knowledge rather than a live check; if you want me to verify nothing's changed recently (e.g., a 2026 Write the Docs report), grant WebSearch and I'll re-check before Tuesday.