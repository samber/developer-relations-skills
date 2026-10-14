## Sign-off: reject the "imports omitted for brevity + `...`" proposal

**Verdict: No-go.** The proposal papers over the defect with a label instead of fixing it. It still ships a fragment that cannot run — it just apologizes for it first.

### Root cause

This is a **verification/extractor defect, not a prose problem.** The region boundary moved to "first line after imports," so every published sample now excludes required setup. Per `references/verification-patterns.md`: *"a region that starts after the imports publishes a sample the reader cannot run."* That's this bug, described exactly. It was introduced by the extractor change, so it belongs on whoever owns the extraction pipeline, not the docs-writing team's queue.

### Why the docs team's proposal doesn't fix it

Checked against the shipped policy (`references/sample-policy-template.md`) and audit checklist:

- **Anatomy §3:** *"All imports and dependencies, explicitly. No elided setup in the published artefact."* Adding a note doesn't restore the imports — the artefact is still elided.
- **Safety §4:** *"Omissions marked with a comment in the language's own syntax — never `...` or `…`, and never inside a click-to-copy block."* The proposal is the exact thing this rule forbids: an ellipsis, inside the copyable block, standing in for real code.
- **Severity math gets worse, not better.** Per `audit-checklist.md`, a missing import is a **P1** ("does not work as written"). An ellipsis inside a click-to-copy block is a separate **P2**. The proposal leaves the P1 in place and adds a P2 on top of it. Net effect: two policy violations where there was one, with a label that makes the P1 *look* intentional — which is worse for triage, since a reader (or future auditor) may now read "brevity" as "the writer chose this," making the defect harder to find and re-flag than a silent one was.

The doc-team's framing — "readers said imports were noise" — is a real, separate complaint, but it's a *readability* problem. What actually broke is a *runnability* contract. Solving the second by degrading further on the first axis (a literal non-compiling token) doesn't address either one honestly; it trades a support ticket that says "the sample doesn't run" for one that says "the sample doesn't run and now also tells me it's my fault for not knowing what to fill in."

### What to do instead

The policy already names the correct pattern for this exact tension (`references/verification-patterns.md`, "Hiding setup without breaking runnability"): *"Readers want the three interesting lines; the test needs the whole file. Every workable scheme separates the two: the tested artefact is the complete file, the published artefact is a region or a filtered view of it."* Rustdoc's `#`-prefixed hidden lines are the reference implementation — compiled and copied, not rendered.

Concretely:

1. **Revert the extractor's region start to the top of the file** (or to before the import block) as an immediate interim step. This is the fastest way to stop the P1 bleed while the real fix ships — every sample currently published is non-runnable, so this is corpus-wide, not per-page.
2. **Introduce a two-region marker convention**, per the "snippet tagging" mechanism in `references/governance-and-ownership.md`: a `setup` region (imports + client construction) and a `body` region, both real code, no placeholders. Identifiers stay snake_case, product-area-prefixed, identical across languages, exactly as the parity rules already require.
3. **Render `setup` collapsed by default** (a `<details>`/"Show imports" disclosure, or equivalent in the docs platform) so the page reads clean — this is what actually answers the readers' original complaint about noise.
4. **Make the copy-to-clipboard action assemble `setup` + `body` concatenated**, regardless of collapsed/expanded display state. This is the load-bearing detail: what renders and what copies are allowed to differ, what copies and what CI runs are not.
5. **Add a snippet-sync assertion in CI** (`governance-and-ownership.md`, enforcement layer): re-run the extractor against source on every PR and fail on drift, so a future extractor change that silently truncates a region again fails a required check instead of reaching readers.
6. **Re-run the corpus audit after the fix ships.** Every sample tiered Run or Compile-only was, by definition, claiming a guarantee it wasn't meeting the moment the extractor changed — confirm the compile/run pass rate is back at the ≥90% policy threshold before calling this closed, not from the fix diff alone.

### One-line summary for the doc team

Don't label the gap — close it. Collapse the imports for readability, but keep them real, present, and inside what the copy button copies; that gets the noise complaint and the "doesn't run" complaint fixed by the same change instead of trading one for the other.