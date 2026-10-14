## Where we stand

4 owned languages (Python, TypeScript, Go, Java) × 6 scenarios = 24 slots. 17 filled and in CI today:

- Python: 6/6. Done.
- TypeScript: 5/6. 1 gap.
- Go: 4/6. 2 gap.
- Java: 2/6. 4 gap.
- Ruby: 1/6. Unowned, community-contributed. Not part of parity math.

7 slots missing before 4-language parity true: 1 TypeScript, 2 Go, 4 Java. "Exists untested" and "doesn't exist" both count as gap — same fix needed, write + wire into CI, since done means tested and running, not just present.

## Risk in plan as stated

Plan says: rewrite Go/Java "same variable names" as Python. Literal read breaks idiom. Python uses snake_case. Go and Java use camelCase by convention, enforced by gofmt/checkstyle-type gates. Copying literal Python names into Go/Java fails those gates or reads as foreign code to native reviewers.

Fix target: structural parity, not string parity. Same scenario order, same step sequence, same helper decomposition, same comments — casing transliterated per language convention (snake_case → camelCase). Side-by-side diff still shows only syntax differences under this rule; "same variable name" becomes "same variable identity, idiomatic casing."

## Missing info before rewrite starts

Don't know which 4 of 6 scenarios Go covers, or which 2 of 6 Java covers. Can't plan rewrite-vs-new-build per scenario without that map. First step is an audit, not code.

## Plan

1. Audit Go's 4 covered scenarios and Java's 2 covered scenarios against Python's 6. Confirm which are structural matches already, which need full rewrite, which are net-new.
2. Close TypeScript's 1 gap. Small, isolated, doesn't block Go/Java work. Same CI-wired bar as Python.
3. Rewrite Go to full Python parity (6/6): fix structure on 4 existing scenarios, build 2 new ones, wire all into CI. Go first — smaller gap, proves the parity pattern before applying it to the harder case.
4. Rewrite Java to full Python parity (6/6): fix structure on 2 existing scenarios, build 4 new ones, wire all into CI. Apply the pattern proven in step 3.
5. Lock a parity gate: script or checklist that checks scenario order and helper-name correspondence across Python/Go/Java/TypeScript. Prevents future scenario additions from landing in one language only.

One scenario = one PR. Small, reviewable, matches conventional-commit style already in use.

Ruby: no work assigned. Community-owned, no commitment to bring to parity.

**Next action:** audit Go's 4 covered scenarios against Python's 6 scenario list, name the 2 missing. Under 15 minutes once scenario list is in hand.