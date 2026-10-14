**Edit mode, engineering-blog-post skill.** No draft file was attached or found in this repo, so what follows is the complete punch list the skill produces at this stage — the "skip to the credibility gate, return a fix list" step — sized to what you described (3 code blocks, 2 benchmark tables, raw command output, Contentful). Run it against the actual text; feed me the draft if you want it applied line-by-line.

## Already covered
- Technical accuracy — done, by a colleague. That's one of the skill's two required reads (engineer who worked on the system).
- Missing: the second read — someone outside the team, checking it survives without insider vocabulary. Not the same reviewer, don't skip it.

## Before the prose pass

1. **Build the claim ledger.** Technical accuracy ≠ evidence classification. Every assertion needs measured/shown/sourced/opinion. The tables and command output cover the numeric claims — check the surrounding narrative for anything unclassified (adoption claims, unsourced performance adjectives).
2. **Classify the benchmark**, then check disclosure: benchmarketing (vs. a named competitor) gets the most scrutiny, subtle benchmarketing (measuring your own product) less, community service least. Confirm near the tables, not in an appendix:
   - hardware/instance type
   - versions of everything compared, dated
   - workload/dataset
   - config/tuning applied per side (flag if one side is untuned — say so, call the result indicative)
   - run count and variance
   - link to the scripts
   - absolute values next to any ratio ("3x" alone hides ms vs. hours)
   - at least one chart — two tables is prose-read, this pattern is visual-read
3. **Limitations section** — confirm it exists and is specific (workload it's wrong for, cost paid, what's untested), not just implied by hedges elsewhere.
4. **Snippet check**, per code block: runnable as shown or explicit about elisions, imports included, expected output shown, real names not `foo`/`bar`, ~20 lines, version pinned, post dated.
5. **Run the scanner**, numbers suppressed since they're already in the ledger:
   `./scripts/scan-draft.sh <draft>.md --no-numbers`
   Grep pass, not a verdict — review every hit, keep what the post already backs.

## The prose pass itself

6. Register check first: first person, what was hard, what got cut, what's still ugly. Then the humanizer pass for hollow transitions, uniform sentence rhythm, significance inflation, tidy triples.
7. Preserve exactly, untouched, through that pass: the three code blocks, the raw command output, both benchmark tables, every version number, the title. Humanizing those loses precision for no gain.

## Re-gate before shipping
All six, binary, no partial credit: claims 100% classified · no unsourced numbers · benchmark disclosure complete · ≥1 concrete limitation · zero unbacked superlatives · snippets pinned and dated.

## SEO/metadata — after preview, not before
The skill's own order is preview first, metadata second, because tables and code blocks are what breaks in the CMS, not in the markdown draft.

- **Preview in Contentful itself** before writing any metadata. With 2 tables + 3 code blocks + raw output, specifically verify against this space's actual content model — don't assume:
  - does the Rich Text field render tables natively, or do they need to go in as an embedded entry/asset instead
  - do code blocks keep monospacing and highlighting, or degrade to plain paragraphs
  - does wide command output wrap and mangle, or stay preformatted
- **Title tag** — if the content type has a separate SEO-title field from the display title/H1, write to that field explicitly; don't assume Contentful reuses the H1.
- **Meta description** — one sentence a searcher acts on. Pull it from whatever you'd tell a colleague to repeat after reading — not a restatement of the H1.
- **Slug** — short, matches the primary phrase, nothing time-sensitive that'll look stale (no dates/ticket-style IDs).
- **Alt text** — every image, including any chart exported from the benchmark tables: describe what it shows, not "chart" or "screenshot."
- Same Ship step, worth clearing while you're in there: internal/external link targets resolve, tags are set.

## Still open after all of that
- The outside-the-team reviewer pass — not done yet based on what you described.
- Whether both benchmark sides were tuned fairly — confirm before publish, or label the comparison indicative in the post itself.
- Legal/security read — only if the benchmark names a competitor, or the post touches customer/security detail. Flag if either applies; skip otherwise.