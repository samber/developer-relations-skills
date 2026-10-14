## Edit mode — "How Kestrel Cut Our Index Build Time"

**Pattern this post is playing as:** subtle benchmarketing (you're measuring your own product's speedup) wearing a How-We-Built-It voice. That combination is exactly what pushes the credibility gate's benchmark-disclosure and limitations checks to "must fix" rather than "nice to have."

Per the skill's edit-mode rule: no full rewrite. Below is the gate report and a tiered fix list with the offending lines quoted. Only the passages you approve get rewritten.

### Credibility gate — current state: 0/6 pass

| # | Check | Status | Why |
|---|---|---|---|
| 1 | Claims classified, 100% | ❌ | "fastest available today," "12x faster," "thousands of teams" have no evidence class — not measured, shown, sourced, or marked opinion |
| 2 | No unsourced numbers | ❌ | "12x faster" has no run, methodology, or absolute values behind it |
| 3 | Benchmark disclosure complete | ❌ | A bar chart with no hardware, versions, workload, config, or run count |
| 4 | At least one honest limitation | ❌ | No limitations section exists anywhere in the post |
| 5 | Zero unsupported superlatives | ❌ | "the fastest columnar index builder available today" is unbacked |
| 6 | Snippets pinned and dated | ❌ | 40-line snippet, no imports, calls a method renamed in 2.0 |

### Blocking (fix before Thursday)

1. **Intro** — *"Kestrel is the fastest columnar index builder available today."* An unsourced superlative with implicit competitor comparison and no data in the post. Cut it, or replace with the one number you can actually defend (e.g., "our index builds are now Xs for a Y-row table") and drop the "fastest ... today" framing entirely — that claim needs a cross-vendor benchmark this post doesn't have.

2. **Results section** — *"index builds are 12x faster"* next to a bar chart, nothing else. Per the skill's benchmark discipline: methodology must precede results, and ratios need absolute values alongside them (12x of 12 minutes→1 minute reads completely differently than 12x of 1.2s→0.1s). Add, right next to the chart: hardware/instance type, Kestrel version on each side, dataset size and shape, number of runs and variance. Without this, the chart is decoration, not evidence.

3. **Code snippet** — 40 lines, no imports, calls `kestrel.build_index()`, which was renamed in 2.0. A reader who copies it gets an import error on line one and an attribute error on whichever line calls the old method. Three separate fixes, not one: add the imports, swap in the current 2.0 method name, and cut the block to ~20 lines or fewer (one concept), linking a repo for the rest. Pin the Kestrel version the snippet targets and date the post.

4. **Paragraph four** — *"thousands of teams rely on Kestrel in production."* An adoption claim with no source, which the scan script would flag outright. Cut it, or replace with an actual number you can point to — production customer count, download count, a named reference deployment the engineer can confirm.

### Strong (fix unless the author disagrees)

5. **No limitations section anywhere.** The post recommends adopting a faster index builder and never says where the win doesn't apply. Add a "Where this doesn't help" section before the close — needs the engineer's input, but candidates worth asking about: dataset sizes/shapes where the 12x doesn't hold, whether query performance or memory changed as a trade-off, what wasn't re-tested after the rewrite (concurrent builds? incremental updates? non-columnar workloads?).

6. **Closing paragraph** — *"we're thrilled to share this journey with the community."* Marketing register in a post that should end in the engineer's voice. Replace with something concrete and specific: what's still unresolved, what surprised the author most, or the one thing they'd do differently next time.

### Optional

7. Confirm early in the post, in one plain sentence, that Kestrel is the author's own company's product (title says "Kestrel Data engineering blog," so this is presumably obvious in context — but state it explicitly near the top rather than letting the reader infer it, especially since claim #1 above reads like a competitive claim).

### Handoff — needs the author, not an editor

- The actual benchmark run: hardware, both Kestrel versions, dataset, run count and variance.
- The current (2.0+) name and signature for whatever `kestrel.build_index()` became.
- At least three honest limitations — the engineer who ran this knows what didn't improve; an editor can't invent them.
- A technical reviewer to confirm the corrected snippet actually runs against 2.0 before it ships.

Run `scripts/scan-draft.sh` over the actual file once these are patched — it'll catch any remaining superlative, unsourced number, or undated version reference lexically, but every hit still needs a human read since it's a grep pass, not a judge.