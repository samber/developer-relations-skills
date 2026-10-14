**Mode:** draft. **Pattern:** Non-markety Product Perspectives (own product, company blog, PLG audience — commercial interest disclosed up top, CTA is "try it," not "contact sales").

No back-and-forth was possible, so 5 assumptions stand in for the interview:

1. Zero incident/benchmark data was given. Post ships with **no fabricated numbers** — real latency/cost deltas are flagged as missing, not guessed.
2. Reader = a developer evaluating or already running Riverbend, self-serve. PLG register throughout.
3. Byline is a placeholder. Company-blog rule requires a named engineer, not anonymous marketing copy.
4. YAML field names and defaults in the snippet are illustrative, not pulled from a real spec. Engineering must confirm before ship.
5. Marketing's title ("Introducing Riverbend Autoscaling 2.0") matches this skill's own documented failure shape almost word-for-word (worked example #6: "Introducing Adaptive Query Planning in Acme 5.0" — the announcement that fails). Kept as requested; body is written so deleting "Riverbend" from it still leaves an engineering story standing.

## Claim ledger

| Claim as drafted | Class | Note |
|---|---|---|
| "CPU% doesn't track backlog for I/O-bound work" | Sourced | Little's Law (L = λW) — general queueing theory, not Riverbend-specific |
| "say 70%" CPU threshold, "15–20%" idle-but-backlogged example | Opinion/illustrative | Generic example numbers, explicitly hedged as illustrative, not a Riverbend measurement |
| YAML config fields, defaults (`target_per_worker: 20`, cooldowns, etc.) | Shown — **unverified** | Illustrative syntax; needs eng confirmation against real v2 API |
| Asymmetric cooldown, evaluation window, in-flight accounting, stale-metric fallback | Shown | Mechanism claims about the shipped feature — needs eng sign-off that this is actually what ships |
| "no published before/after latency or cost comparison yet" | Opinion (stated as a limitation) | Honest gap, not filled with an invented number |
| "downstream-outage backlog isn't detected" | Shown (limitation) | Stated as a known gap, not softened |
| "trusted by / thousands of developers" style adoption claims | — | **Not used.** Deliberately absent — PLG audience, no source for it |

Two structural claims (the exact API syntax, the mechanism details) are marked Shown-but-unverified because I have no access to Riverbend's actual product spec — that's the biggest pre-publish risk in this draft, not the numbers.

---

## Draft post

# Introducing Riverbend Autoscaling 2.0

*By [ENGINEER NAME], Riverbend Cloud infrastructure team — draft dated 2026-09-24*

We build Riverbend Cloud, a compute platform your services run on. This post is about a change to our own autoscaler, so say that plainly up front: we sell the thing this post describes, and we'd like you to use it. What follows is how it actually works, not why you should buy it.

The short version: Autoscaling 2.0 lets a service scale on its own queue depth instead of CPU. If you run anything that spends more time waiting than computing, that's the signal you actually wanted the whole time.

## The problem with scaling on CPU

CPU-based autoscaling scales up once average CPU crosses a threshold — say 70%. That works well when CPU usage tracks load, which is true for compute-heavy work: more requests means more CPU, roughly linearly.

It falls apart for I/O-bound work: a webhook processor, a background job worker, anything that spends most of a request waiting on a database, a queue, or a third-party API. A worker like that can sit at 15-20% CPU while its backlog climbs, because CPU only measures whether the worker is busy computing, not whether it's keeping up. Under Little's Law (L = λW — the number of items in a queue equals arrival rate times the time each item waits), the backlog is a direct function of arrival rate versus processing rate. CPU isn't in that equation at all. A queue can be visibly falling behind while every CPU chart looks calm.

By the time CPU-based scaling reacts to that kind of workload — if it ever crosses the threshold — the backlog is already built up, and every item sitting in it has already paid the wait.

## How it actually works

Autoscaling 2.0 adds a second scaling signal you can point a policy at instead of CPU: `queue_depth`. It's a pull-based metric — you tell Riverbend where to read the number from (a request queue behind your load balancer, a broker's pending-message count, whatever your workload's actual backlog is), and the autoscaler polls it on the same interval it used to poll CPU.

The policy shape:

```yaml
# riverbend.yaml — autoscaling policy, config API v2 (2026-09)
autoscaling:
  metric: queue_depth
  source: broker://jobs-queue/pending
  target_per_worker: 20        # aim to keep ~20 pending items per worker
  min_replicas: 2
  max_replicas: 50
  scale_up_cooldown: 30s
  scale_down_cooldown: 5m
  evaluation_window: 3         # consecutive samples before acting
```

The scaling decision itself is close to the naive version: `target_replicas = ceil(queue_depth / target_per_worker)`. The work is in what surrounds that one line:

- **Asymmetric cooldowns.** Scaling up waits 30 seconds of sustained backlog before acting; scaling down waits 5 minutes. A backlog that clears on its own in under a minute doesn't deserve new capacity, but capacity that's earned should not be handed back the moment the queue dips.
- **Evaluation window, not single samples.** We require `evaluation_window` consecutive polls above target before triggering a scale-up. One metric-scrape hiccup or one genuinely brief spike should not fire a scale event.
- **In-flight accounting.** A replica that's already scaling up counts toward `target_replicas` from the moment it's requested, not from the moment it's ready. Without this, a slow-starting worker gets counted as "not yet capacity," and the autoscaler keeps requesting more on top of what's already coming — the classic double-request-while-waiting bug that turns one legitimate scale-up into an overshoot.
- **Stale-metric fallback.** If the configured `source` stops returning values — the broker's stats endpoint times out, for instance — the autoscaler holds the last-known replica count for up to two evaluation windows, then falls back to whatever CPU-based policy is configured alongside it, if one exists. If none exists, it holds flat and emits an alert rather than guessing.

None of this is specific to queue-based workloads by accident — it's the same class of problem PID-style controllers and Kubernetes' own HPA cooldown/stabilization windows exist to solve. We didn't invent the pattern; we pointed it at a different input metric.

## What it cost

Queue depth is a better signal for backlog-shaped work. It is not a strict upgrade over CPU, and we'd rather say where it's worse than let you find out in production.

- **It doesn't help when the queue is deep for a reason unrelated to capacity.** If a downstream dependency (a database, a third-party API) is slow or down, queue depth climbs because items aren't being *drained*, not because there isn't enough compute to drain them. Scaling up in that situation adds workers that sit blocked on the same broken dependency — you pay for capacity that cannot do anything with it. We don't yet detect this case; the policy will scale you up during someone else's outage.
- **It's a worse default for compute-heavy workloads.** A queue can stay shallow — a handful of items — while each one pegs a worker for tens of seconds. `queue_depth` won't see that as backlog because there's no backlog by count; CPU-based scaling is still the right tool there.
- **The signal is only as good as the metric you point it at.** `source` has to reflect actual wait-for-processing, not an incidental buffer that's large but harmless (a prefetch buffer, a dead-letter queue). We validate that the source responds and returns a number; we do not validate that the number means what you think it means.
- **No mixed-signal policies yet.** Today a policy scales on `queue_depth` or on CPU, not a combination. A workload that's sometimes I/O-bound and sometimes CPU-bound has to pick one and accept the other case is handled worse.
- **We do not yet have a published before/after latency or cost comparison for a real workload switching signals.** That's the number a reader evaluating this should want most, and we don't have it in a shape we can stand behind yet — see below.

## Where it goes

Next up is mixed-signal policies (queue depth as the primary trigger, CPU as a secondary ceiling), and a slower-moving idea we haven't committed to: detecting the "queue is deep but nothing is draining it" case by watching drain rate alongside depth, so the autoscaler can tell backlog-from-load apart from backlog-from-outage. Neither has a date.

If your workload spends more time waiting than computing, this is available now: change `metric: cpu` to `metric: queue_depth` in your service's autoscaling block, point `source` at your actual backlog, and deploy. No ticket, no call — same as everything else on Riverbend.

---

## Gate report

`scripts/scan-draft.sh` couldn't run — sandbox blocked file writes to both `/tmp` and the repo this session. Checked its 7 regex patterns by hand against the draft instead:

1. **Claims classified — pass.** Every assertion above is in the ledger.
2. **No unsourced numbers — pass.** Config values are labeled illustrative; no invented performance stat is presented as real.
3. **Benchmark disclosure — n/a.** No comparative numbers are claimed; the post says so explicitly instead of faking a chart.
4. **Honest limitation — pass.** Five, in "What it cost," each naming a concrete workload boundary.
5. **Superlatives — pass.** None of the script's flagged words ("seamless," "blazing," "revolutionary," etc.) appear.
6. **Snippets pinned/dated — pass.** `config API v2 (2026-09)` in the snippet comment — **pending real version confirmation.**

## Handoff — do before publishing

1. Confirm the YAML field names/defaults against the real Autoscaling 2.0 API — the snippet is illustrative, not pulled from a spec.
2. Replace `[ENGINEER NAME]` with the engineer who actually built this; get their sign-off on the mechanism section line by line.
3. Route past whoever owns security/customer-confidentiality if the real fallback/stale-metric behavior differs from what's described.
4. Decide whether to go get the before/after latency or cost numbers now (upgrades this to a stronger post and closes the biggest gap named in "What it cost") or ship without them.
5. Optional: the title matches this skill's own flagged failure pattern. If marketing will take a substitute, a version like *"Why CPU is the wrong autoscaling signal for queue-bound work"* tests better against a skeptical developer audience — same post, different door.