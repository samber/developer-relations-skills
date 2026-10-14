# We moved billing reconciliation from Ruby to Go. Here's what actually happened.

At Halcyon Systems, our billing reconciliation service is the thing nobody notices until it's wrong. It pulls transaction records from payment providers, matches them against our internal ledger, flags discrepancies, and produces the numbers finance uses to close the books. It runs on a schedule, it runs on demand, and increasingly, it ran too slow and cost too much. Over about four months this year, we rewrote it from Ruby to Go. p99 latency dropped from 840ms to 95ms. Our EC2 bill for the service fell roughly 60%. And — the part people usually don't believe — the migration itself was uneventful.

This isn't a "Ruby is bad" post. Ruby served us well for years, and plenty of the problems below were as much about our specific implementation as the language. But if you're a backend engineer weighing whether a Go rewrite is worth the disruption, here's what we actually ran into, decision by decision.

## Why reconciliation specifically hit a wall

Reconciliation is a batch-and-fan-out workload: pull N days of transactions across M providers, match them against the ledger, reconcile discrepancies, write results. It's embarrassingly parallel in shape, which is exactly the kind of workload that exposes the weak points of a threading model built around a global interpreter lock.

Our Ruby implementation used a Sidekiq worker fleet to get concurrency, since threads inside a single Ruby process don't give you real parallelism for CPU-bound matching logic. That meant our concurrency unit was a process, not a thread — so scaling out meant scaling out *processes*, each with its own memory footprint, each idling between reconciliation runs, each contributing to a GC that paused harder as batch sizes grew. We were provisioning for peak batch size and idling most of the day. That's most of where the EC2 bill came from.

The other recurring pain was correctness at the boundary between "money as a number" and "money as a Ruby object." Nothing catastrophic ever happened in production, but decimal precision bugs and unit mismatches (cents vs. major units) were the most common category of bug we caught in code review, and the one every senior engineer on the team had a personal war story about.

## Why Go, specifically

Three things mattered more than the general "Go is fast" pitch:

**Goroutines make the actual workload shape cheap.** Reconciliation is fan-out-fan-in: spin up a goroutine per provider batch, collect results over a channel, done. No process-per-worker overhead, no separate job queue infrastructure just to get parallelism. We still use worker pools to bound concurrency against provider rate limits, but the unit of concurrency is now a few KB of stack, not a Sidekiq process.

**Static typing catches money bugs before they ship.** Every amount in the new service is a distinct type wrapping a fixed-point integer (minor units, never a float, never an untyped number). Mixing a `USDCents` with a `EURCents` or passing a raw `int64` where an `Amount` is expected is a compile error, not a code review comment or a production incident. This is the single change the team cites most when asked what they'd never go back on.

**Deployment got boring, in a good way.** A single static binary replaced a Ruby app image with its interpreter, gem dependencies, and native extension build chain. Startup time dropped from tens of seconds to under one. That mattered more than expected once we started running reconciliation as short-lived, autoscaled jobs instead of long-running workers — fast, predictable startup is what makes scale-to-zero actually pay off.

## How we did it, without anything breaking

Four months is a long time for something this central to finance operations, and we spent a good chunk of it on process, not code.

We used a strangler approach: new reconciliation logic went into the Go service provider-by-provider, not all at once. Each provider's matching logic ran in *shadow mode* first — the Go service processed the same batches as the Ruby service, and we diffed outputs line by line before the Go output was trusted for anything finance-facing. Discrepancies in shadow mode were almost always in the Go implementation early on (an edge case in provider-specific rounding, a timezone boundary in batch windows), which is exactly what you want shadow mode to catch before it catches a real dollar amount.

Cutover per provider happened only after shadow mode ran clean for a full billing cycle. That's a conservative bar, and it's why the migration took four months instead of a few sprints. We didn't hit a moment where we had to roll back a cutover, largely because nothing got promoted out of shadow mode until it had already proven itself against real production traffic.

The team already had Go experience from other services, which mattered more than any of the technical decisions above. If your team is learning goroutines, channels, and context cancellation for the first time on a financial system, budget more room for review than we needed to. We didn't hit goroutine leaks or context-propagation bugs in this migration, but that's a track record earned on earlier, lower-stakes Go services, not a property of the language.

## The numbers

| | Ruby | Go | Change |
|---|---|---|---|
| p99 latency | 840ms | 95ms | ~8.8x faster |
| EC2 spend (this service) | baseline | ~40% of baseline | ~60% reduction |
| Migration duration | — | — | ~4 months |
| Production incidents caused by migration | — | — | 0 |

The latency drop came from three compounding factors, not one: real parallelism during fan-out, no GC pauses on the scale Ruby's mark-and-sweep hit under batch load, and removing the per-request overhead of the interpreter itself. The cost drop came almost entirely from the concurrency model change — fewer, denser processes doing the same work, plus the ability to scale reconciliation jobs down to near-zero between runs instead of keeping a worker fleet warm.

## What we'd tell a team evaluating this

Go's advantage here wasn't raw speed for its own sake — it was that goroutines matched the actual shape of the workload, and the type system matched the actual risk profile of the domain. If your bottleneck is CPU-bound fan-out work currently paying a per-process tax, or your bug class is "the wrong number got multiplied by the wrong thing," those are the two signals worth weighing heavily. If neither applies to your service, the migration cost may not be worth it — ours was, because reconciliation is both.

The unglamorous truth is that the smooth migration was a process outcome, not a language outcome: shadow mode against real traffic, provider-by-provider cutover, and a team that had already made its Go mistakes on lower-stakes systems first. Go didn't make the migration safe. Running new code next to old code and diffing the output before trusting it did.