That's a minor leftover empty directory; not worth pursuing further. Proceeding to the deliverable.

**Mode: Draft mode. Pattern: Benchmarks and Test Results, classified as Benchmarketing** (own product vs. a named competitor — the highest-scrutiny, least-benefit-of-the-doubt class this skill defines). No other pattern fits: the material is a single comparative measurement, not a debugging story, migration, or architecture writeup.

Before drafting, the setup as described fails the skill's benchmark bar on the two traps it names explicitly:

- *"Tuning your own side while leaving the comparison at defaults is the specific move that gets a post publicly dismantled."* That's exactly this setup — production-tuned vantagedb against out-of-the-box Northlake. "Most people run it on defaults" may be true, but it doesn't make this a fair *performance* comparison — it makes it a tuned-vs-typical comparison, which is a different, narrower claim than the headline states.
- *"A single run reported as if it were stable"* is named as a trap, and *"'3x faster' hides whether the gap is 3ms versus 1ms or three hours versus one"* — one run, no absolute values, is exactly that gap.

So I'm not shipping "3x faster ingest than Northlake" as a flat, unqualified headline — that's the one sentence Northlake's own engineers (who will read this, per the sales-led B2B audience the skill flags) can dismantle in the first comment. What follows is the version that survives that comment: same result, same number, disclosed enough to be defensible.

## Claim ledger

| Claim as given | Class | Action |
|---|---|---|
| "our ingest was ~3x faster in this run" | Measured, but incomplete | Keep — but only alongside methodology and absolute values |
| "Northlake on its install defaults because that's what most people actually run" | Opinion presented as justification | Downgrade to a stated assumption; cite it as an assumption, not evidence, since no source is given for "most people" |
| "3x faster ingest than Northlake" (as an unqualified headline) | Unsupported as stated | Cut the unqualified form. Reframe to name the asymmetry in the same sentence |
| m6i.2xlarge, one run, ours tuned / theirs default | Shown | Disclose in full, prominently, not in a footnote |
| absolute ingest throughput (records/sec or MB/s) for either engine | Missing | **Blocking** — not in the material you gave me; the draft below has it as a placeholder you must fill in before this can publish |
| version numbers, workload shape, dataset | Missing | Blocking — same treatment |

## The draft

---

**Title:** *Ingest throughput: vantagedb vs. Northlake TSDB's default config*

*(Not "3x faster than Northlake" — the config asymmetry has to be load-bearing in the title itself, or the post reads as hiding it.)*

*Posted by [engineer name], Vantage Metrics. Vantage Metrics builds vantagedb, the engine on our side of this comparison — Northlake TSDB is a direct competitor. [name] ran the Northlake side, [name] ran ours.*

We wanted to know how our ingest path compares to Northlake TSDB, the engine we're most often evaluated against. This post covers one run, on one instance type, with our side on the tuning profile we run in production and Northlake left at its install defaults. We're publishing the full setup so you can judge how far this travels to your own workload — and we're treating it as a first data point, not a verdict.

### What we measured

Ingest throughput: [records or points] per second sustained by each engine while replaying [dataset description — real workload shape or synthetic, cardinality, point size].

### How we measured

- **Hardware:** 1 × `m6i.2xlarge` (8 vCPU, 32 GiB), [region/AZ]
- **Versions:** vantagedb [version], Northlake TSDB [version], tested [date]
- **Configuration:**
  - vantagedb: our production tuning profile ([name the settings that differ from default, or link the config])
  - Northlake TSDB: install defaults, unmodified
  - **This is the asymmetry to weigh before trusting the ratio below.** We did not tune Northlake against its own production guidance. This is a tuned-vs-default comparison, not a tuned-vs-tuned one — closer to "how does our production setup compare to someone who just installed Northlake" than to "which engine is faster."
- **Workload:** [client, concurrency, batch size, total volume]
- **Runs:** 1 per engine. We have not re-run for variance — read the number below as indicative, not stable.
- **Reproduce:** [link scripts/config, or say none exist yet]

### Results

| Engine | Ingest rate | Config |
|---|---|---|
| vantagedb | **[absolute value — required before publish]** | production tuning profile |
| Northlake TSDB | **[absolute value — required before publish]** | install defaults |
| Ratio | ~3x | — |

*(Chart goes here — this pattern is read visually, not as a bare table.)*

We're not publishing the ratio alone: "3x" hides whether this is 3,000 points/sec against 1,000, or 3M against 1M, and a reader can't judge whether that's relevant to their own scale without the absolute numbers. Those numbers are the one hard blocker on this draft — see handoff below.

### What this does and doesn't tell you

It tells you our production configuration outperforms Northlake's out-of-the-box ingest path, once, on this workload shape. It does not tell you:

- How vantagedb compares to a Northlake instance tuned against Northlake's own production guidance — we haven't run that.
- Whether the gap holds at other instance sizes, or over a longer sustained run.
- Whether it holds on a different workload (higher cardinality, smaller batches, mixed read/write) — we only tested ingest in isolation.
- Anything about query performance, compaction, or the rest of the engine.

If Northlake's docs recommend non-default settings for production ingest — most engines' docs do — a reader running Northlake in production isn't really the audience this number describes. They're closer to a comparison we haven't published yet.

### What we'd want before calling this conclusive

- Re-run with Northlake configured per its own documented production guidance — ideally reviewed by a Northlake maintainer or checked against their docs, the way the worked disclosure block in this skill treats an invited vendor review.
- At least 3 runs per side, to report a spread instead of a point.
- The absolute throughput numbers, published next to the ratio.
- A second instance size, to see if the gap is stable or an artifact of this one.

Until then, the honest version of the headline claim is: *our tuned config beat their defaults, once* — a real result, and a narrower one than "faster than Northlake."

---

## Credibility gate

| Check | Status |
|---|---|
| Claims classified, 100% | Pass — ledger above |
| No unsourced numbers | **Fails until you supply**: absolute ingest rates, versions, workload detail (bracketed placeholders) |
| Benchmark disclosure complete | Pass in this draft — hardware, versions (pending), workload (pending), tuning asymmetry, run count all named near the numbers |
| At least one honest limitation | Pass — "What this does and doesn't tell you" |
| Zero unsupported superlatives | Pass — no "fastest"/"best"/"revolutionary" language; scanned against `scripts/scan-draft.sh`'s pattern set by hand (couldn't execute the script itself — file write to a scratch path was blocked by this session's permissions) |
| Snippets pinned and dated | N/A — no code snippets in this post |

Two checks are not closeable by me: the missing absolute numbers, and the tuning asymmetry, which requires an actual decision from you, not a rewrite from me.

## Handoff — what still needs a human

1. **Blocking:** absolute ingest throughput for both engines (this is the number a reader actually needs; the ratio alone doesn't clear the gate).
2. **Blocking:** exact versions of vantagedb and Northlake TSDB, and the test date.
3. **Strong:** re-run at least twice more per side before calling any number stable — right now it's one run each.
4. **Strong:** decide, explicitly, whether you're willing to also tune Northlake per its own docs. If yes, that's the post that survives being cited for years (this skill's "compounding asset" case). If no, ship the narrower, disclosed claim above and say so in the title, not just the fine print.
5. **Needs your input:** dataset/workload description, region/AZ, and whether the "most people run Northlake on defaults" framing is something you can actually source (adoption survey, support tickets) or should be dropped as an assumption.
6. **Review chain:** someone who ran the Northlake side should independently confirm the "install defaults, unmodified" claim before publish — that's the line most likely to be checked by a Northlake engineer in the comments.
7. Not yet run: the humanizer pass (do that after you fill in the placeholders — running it on bracketed TBDs wastes the pass) and a legal/competitor-naming read, since this names a competitor directly.