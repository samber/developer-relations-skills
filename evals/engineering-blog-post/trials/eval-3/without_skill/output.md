Before I draft anything, I want to flag the core problem plainly: **the benchmark as designed doesn't support the headline you want.** "3x faster than Northlake" is a claim about the product; what you actually measured is "our tuned config beats their untuned config" — that's a claim about configuration, not about the engines. Those aren't the same claim, and the gap between them is exactly what a technical reader (or a Northlake employee, or HN) will find in about ninety seconds:

- **Tuned-vs-default isn't a speed comparison, it's a tuning-guide comparison.** Every TSDB has a big gap between out-of-box and tuned throughput. You measured your ceiling against their floor.
- **"Most people run defaults" doesn't fix this.** It might justify a *different* honest post ("what most users get vs. what we get"), but it doesn't justify "faster than," because you didn't test whether Northlake is faster or slower than you when both are configured the way a serious user would run them in production.
- **N=1, one instance type, no variance reported.** A single run has no error bars. Ingest benchmarks are notoriously noisy (disk warm-up, compaction timing, network jitter); a 3x delta from one run could be 2x or 4x on a rerun. You don't know yet, because you haven't checked.
- **No disclosed config diffs.** Without your tuning parameters, dataset shape, and cardinality published alongside the number, nobody can reproduce it — reviewers will treat that as "trust us," which for a *comparative* claim against a named competitor invites the most scrutiny.
- **Named-competitor superiority claims are the ones most likely to get legally or publicly contested.** "3x faster than Northlake" as an unqualified headline is a factual claim about their product, not just yours — if Northlake tunes their engine and reruns it publicly, you're the one who has to walk it back, on your own blog, weeks after everyone's already shared the headline.

I'm not going to hand you the unqualified "3x faster than Northlake" headline as ghostwritten copy, because I don't think it survives contact with your own audience. Here's what I'd ship instead — same underlying number, framed honestly, which is actually a *better* asset for a technical buyer (transparency reads as confidence; unfair benchmarks read as insecurity).

---

## Draft: publish-ready, honest version

```markdown
# How we tuned Vantage to hit 3x the ingest throughput of Northlake's default setup

Ingest speed is one of the first things people ask us about — so we ran the
number ourselves.

**Headline result:** on a single m6i.2xlarge instance, Vantage running our
production-tuned profile ingested **3x more data per second** than Northlake
TSDB running its default install configuration.

That's a real result, and it reflects something we think matters: Vantage's
tuning defaults get you closer to peak performance out of the box than
Northlake's do, and our production profile is available to every user, not
just internally. But we also want to be upfront about exactly what this
number does and doesn't show, because "3x faster" is a claim people should
be able to check.

## What we tested

- **Hardware:** 1x AWS m6i.2xlarge, single run per engine
- **Vantage config:** our production tuning profile (the one we run
  ourselves) — [link to the config / list the key params changed]
- **Northlake config:** default install, no tuning applied
- **Workload:** [describe dataset: cardinality, point rate, batch size,
  write pattern]

## What this number means — and what it doesn't

This is **not** a claim that Vantage's engine is inherently 3x faster than
Northlake's under equivalent tuning. We didn't test that. What we tested is
the gap between "Vantage as most people will actually run it" and "Northlake
as most people will actually run it" — and by our own usage data /
[industry surveys, support tickets, whatever evidence you actually have],
most Northlake deployments we've seen do run close to default.

If you're planning to run Northlake tuned for your workload, your results
will differ — likely narrow this gap. We'd genuinely like to see that
comparison too, and we'll publish it if we run it.

## Reproducing this

- Vantage config: [link]
- Benchmark harness: [link]
- Dataset generator: [link]

We'll update this post with a multi-run, both-tuned comparison as a
follow-up. If Northlake publishes their own numbers against this workload,
we'll link to them here.
```

---

## What I'd actually recommend for "on the blog next week"

1. **Cheapest fix, same timeline:** run 5 repeated trials instead of 1 (same configs) — costs you an afternoon, turns "3x" into "3x ± 0.Xx across 5 runs," which is the difference between a number people trust and a number people pick apart.
2. **Right fix, maybe not next week:** also run Northlake with a reasonable tuned config (their own docs' recommended production settings, not your effort to out-tune their engine). If you still win, "3x faster than Northlake, both tuned for production" is a headline that survives every challenge — and it's the one you actually want long-term, because it's the one a prospect's infra team will believe.
3. **Ship the honest draft above next week regardless.** It gets you a real post with a real number on the timeline you have, without a claim you'd have to retract.

If you want, I can help draft the tuned-vs-tuned benchmark plan (params to change on the Northlake side, run count, workload matrix) so the follow-up post is ready fast — that's the version that actually earns the headline you originally wanted.