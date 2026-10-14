# Hollowmere Robotics — code sample audit fix queue

Per `docs-code-sample-standards`, severity anchors are: **P0** = harms the reader who copies it, **P1** = does not work as written, **P2** = works but violates policy. Ordering strictly by severity gives an unambiguous first split (P0 → P1 → P2); within a tier I broke ties by monthly readers affected, since severity alone doesn't distinguish two P1s or two P2s and "readers affected" is the one input everyone in the room will already agree on.

## Ranked queue (worst first)

**1. F3 — P0 — auth setup page embeds a live-format API key**
Evidence: auth setup page, ~9,000 monthly readers.
Fix: replace with a placeholder credential and rotate the exposed key. ~20 min + rotation.
Why #1: P0 is defined as "harms the reader who copies it" — a live-format key is the one finding here that actively damages someone who trusts the page, not just a broken example. Per the skill, P0 always leads "on harm, not efficiency"; its tiny fix cost is irrelevant to the ranking, only to how fast it can close.

**2. F2 — P1 — busiest guide calls a renamed method**
Evidence: batch-ingest sample, method renamed two releases ago, ~18,000 monthly readers.
Fix: update the call to the current method name. ~30 min.
Why #2: highest-reach P1. Every reader who copies this sample today hits a runtime/compile error immediately.

**3. F4 — P1 — C++ quickstart first-call sample has never been compiled**
Evidence: first-call quickstart sample, C++ SDK, no C++ build in CI, ~700 monthly readers.
Fix: stand up a C++ compile-only CI job for the SDK samples. ~3 engineer-weeks.
Why #3: lower-reach P1, ranks below F2 on readers alone.

**4. F5 — P2 — 210 samples corpus-wide with no documented expected output**
Evidence: corpus-wide, ~120,000 monthly readers combined.
Fix: scripted pass to add expected-output blocks; ~6 engineer-days.
Why #4: highest-reach P2 — outranks F1 within the P2 tier on readers affected, even though its total fix cost is larger.

**5. F1 — P2 — 61 SDK reference samples missing a fence language identifier**
Evidence: SDK reference, ~40,000 monthly readers.
Fix: scripted pass adding the language tag to each fence. ~2 hours.
Why #5: lowest-severity finding, so it sits last regardless of its reach or its low fix cost.

**Order: F3 → F2 → F4 → F5 → F1**

## One flag before the team locks this in

The skill's own ranking method (workflow step 9) is *not* a strict severity sort — it ranks below P0 by `(readers affected × severity) ÷ fix effort`, explicitly so cheap, high-reach fixes don't wait behind expensive, low-reach ones. Under that method F2 (72,000/hr) and F1 (20,000/hr) beat F5 (2,500/hr) and F4 (~12/hr) by a wide margin, giving F3 → F2 → F1 → F5 → F4 — F1 and F4 swap ends of the queue entirely.

F4 is also the one line item worth naming explicitly regardless of which method wins the argument: it's the literal "quickstart, first call" sample the skill's pass threshold names as the primary adoption path, and it has never been compiled by anything — not stale, never verified once. The skill's own guidance is to hand-promote exactly this case ("that ratio starves the low-traffic sample that needs a whole harness built to fix it; promote it by hand when it sits on the adoption path"), and the 3-week estimate buys only a compile-only job — the cheap tier the skill prefers, not a full run harness with fixtures and credentials. Worth deciding on purpose, not by which formula happened to be applied.