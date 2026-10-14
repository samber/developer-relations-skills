Two things in what you've described directly hit warnings this skill sets out explicitly, so I adjusted them rather than writing the report as specified — details below, then the full report.

**The stat is on the skill's own "never repeat" list.** `published-findings.md` names this exact line — "94% of developers consider API documentation quality a key factor," attributed to SlashData — as unconfirmed in any SlashData report despite being everywhere. I swapped it for sourced figures from the same reference file.

**The audit-method.md file's negative example is your process, close to verbatim:** "Opened the twelve most-visited docs pages, read every code block, fixed four broken ones. Reported: 'sample quality is good, minor issues on the storage pages.'" It's flagged as biased (most-visited = most-maintained, so it's the healthiest slice of the corpus), unmechanical (nothing was compiled, so only eyeball-visible defects were caught), and a corpus-wide conclusion drawn from a convenience sample. I've kept your actual findings but reframed the conclusion as what a spot-check can honestly support, and turned the gap into the budget ask itself — a "we don't yet know the state of the other 95% of pages" case is a stronger resourcing argument than "everything's fine."

---

## Code Sample Quality — Spot-Check Findings & Audit Proposal

**Halyard Data Docs | [date] | Prepared by Docs**

### Summary

On Thursday I reviewed every code block on our twelve most-visited docs pages by hand and fixed four that were broken on the page. That check is useful signal, but it's a convenience sample of our best-maintained pages, not a measurement of the corpus — traffic correlates with maintenance attention, and nothing was compiled or executed, so only defects visible on read caught. I can't respon­sibly conclude "samples are in good shape" from it, and I want to flag that rather than round it up.

What I can say: on the highest-traffic surface, 4 of the reviewed samples were broken (fixed same day), concentrated on storage pages. What I can't say yet: whether that rate holds, is better, or is worse across the rest of the corpus — the API reference and the examples repo weren't in scope for this pass at all.

### What we found (12 pages, manual read)

| Page area | Code blocks reviewed | Broken, fixed |
|---|---|---|
| Storage | — | 3 |
| Other (query, auth, ingestion, etc.) | — | 1 |

All four were fixed same-day. None were flagged as credential leaks or destructive commands (P0-class); they read as compile/output-class breaks (P1-class) — stale flags, renamed methods, that shape of drift.

*(Fill in exact block counts per page when you have them — I don't have your page inventory, so I've left the totals row open rather than estimate.)*

### Why this isn't a corpus verdict

- **Coverage**: 12 pages out of what's presumably hundreds across the docs site, API reference, and examples repo. We don't have a count.
- **Selection bias**: most-visited pages get the most eyes and the most incidental fixes already. A representative read needs pages stratified by traffic *and* by age and language, including the ones nobody's opened in a year.
- **Detection method**: reading catches wording and obviously-wrong calls; it does not catch "compiles today, silently returns the wrong thing" or "compiled six months ago, broken since the last SDK release." Only running or building the sample catches that class.

### What "in good shape" would actually take to say

A real answer needs, at minimum:
1. A mechanical inventory of every sample — file, line, language, surface, last-commit date — across docs site, API reference, and examples repo.
2. A compile/run pass over the full inventory (not a sample), plus a secret scan.
3. A scored read (idiomaticity, error handling, credential handling) on a *stratified* sample — by language, product area, and age — not the top-traffic pages again.

Published baselines for context, not a claim about our own numbers: Uddin & Robillard's 2015 survey of 323 developers found ambiguity, incompleteness, and incorrectness the most severe documentation problems, with six of ten problem types rated outright blockers by respondents. Postman's State of the API found "better examples in the documentation" the top requested improvement from API producers, ahead of standardization. Neither is a number about Halyard — they're the reason sample correctness is worth budget at all, independent of what our own audit turns up.

### The ask

Fund the audit above: engineering time to stand up a compile/secret-scan gate over the sample corpus, and docs time to run the stratified read. Until that exists, every "our samples are fine" claim — including this report's — is a guess dressed as a finding. That gap is the deliverable this budget line buys us out of.

### Immediate fix queue (from this week's pass)

1. Storage-page breaks (3) — root-cause and confirm no sibling pages share the same stale call. Owner: docs, this sprint.
2. Remaining break (1) — same treatment.
3. Schedule the full inventory pass (item above) before the next release cycle.