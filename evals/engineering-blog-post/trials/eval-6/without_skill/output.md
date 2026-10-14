# Introducing Riverbend Autoscaling 2.0

**Your workers scale on the metric that actually predicts backlog: queue depth, not CPU.**

If you run background jobs, webhooks, or any kind of async processing on Riverbend, you've probably hit this before: CPU on your worker fleet looks fine — 40%, 50%, nothing alarming — while your queue quietly backs up and your p95 job latency creeps from seconds to minutes. By the time CPU finally climbs enough to trigger a scale-up, the damage is done and users are staring at a stuck "processing" spinner.

Today we're rolling out Riverbend Autoscaling 2.0, which replaces CPU-based scaling with a policy that reacts directly to queue depth. It's available now for every account — no sales call, no plan upgrade, just a toggle in your service settings.

## Why CPU was the wrong signal

CPU utilization tells you how busy a worker is *right now*. It says nothing about how much work is waiting. That gap matters most for exactly the workloads people run on Riverbend:

- **I/O-bound jobs** (calling a third-party API, writing to S3, waiting on a slow downstream service) spend most of their time blocked, not computing. A worker can be fully saturated with backlog and still report 15% CPU.
- **Bursty traffic** — a webhook storm, a batch import, a retry stampede — piles items into the queue faster than CPU-based scaling can notice and react. Autoscalers built on CPU are structurally a lagging indicator for this pattern.
- **Variable job cost.** A worker processing ten cheap jobs and a worker processing one expensive job can show identical CPU while one queue is draining fine and the other is falling behind.

In every one of these cases, the number you actually care about — "how long until this job gets picked up" — correlates much more tightly with queue depth than with CPU. So we built the autoscaler to look at the thing you actually care about.

## How it works

Autoscaling 2.0 targets a **jobs-per-worker ratio** instead of a CPU percentage. You tell it how many queued jobs one worker should be responsible for, and it adjusts worker count to hold that ratio steady as load changes.

```yaml
# riverbend.yaml
autoscaling:
  strategy: queue_depth       # was: cpu
  queue: default
  target_jobs_per_worker: 25
  min_workers: 2
  max_workers: 50
  scale_up_cooldown: 30s
  scale_down_cooldown: 5m
```

A few details worth knowing:

- **Scale-up is fast, scale-down is cautious.** We check queue depth every 10 seconds and can add workers within 30 seconds of a backlog forming, but we hold off shrinking the fleet for 5 minutes by default so you don't thrash on a momentary lull. Both cooldowns are configurable.
- **`min_workers` protects your cold start.** If your jobs depend on a warm cache or connection pool, keeping a floor of always-on workers avoids latency spikes from scaling to zero and back.
- **CPU-based scaling isn't gone.** If your workload genuinely is CPU-bound (image processing, video transcoding, anything compute-heavy with a short queue), you can keep `strategy: cpu`, or combine both — the autoscaler scales up if *either* threshold is breached, and only scales down when *both* agree it's safe to.
- **Per-queue policies.** If you run multiple queues on one service (say, `default` and `high-priority`), each can have its own target ratio and worker bounds.

## What changes for you

Nothing, until you opt in. Existing services keep their current CPU-based policy — this is not a breaking change, and there's no forced migration date.

To switch a service over:

1. Open the service in the Riverbend dashboard, go to **Scaling**, and change the strategy to **Queue depth**.
2. Set `target_jobs_per_worker` — start with your current average job duration in seconds as a rough baseline, then tune from there.
3. Or skip the dashboard entirely and set it in `riverbend.yaml`, commit, and deploy as usual.

Same self-serve flow as everything else on Riverbend: no approval step, no minimum plan tier, no need to talk to anyone. If you want to compare behavior before committing, the dashboard's **Scaling → History** view now shows queue depth and worker count on the same chart, so you can see exactly how the new policy would have reacted to your last week of traffic before switching.

## What's next

We're already working on **predictive scaling**, which uses your service's historical queue patterns to pre-scale ahead of known daily/weekly traffic shapes instead of reacting after the fact. If you want early access when that lands, watch the changelog or ping us in the community Slack.

Try it on one service today — flip the strategy, watch the Scaling History chart, and see your queue latency flatten out. If you hit something confusing or a config that doesn't fit your workload, tell us; that feedback is what shaped 2.0 in the first place.