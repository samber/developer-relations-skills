## Verdict: the outage postmortem (a)

Ship the outage writeup. Not close.

**Why the other two are out, fast:**

- **(c) Competitive benchmark** — dead on arrival. You don't have a harness, and building one, running it fairly against three competitors, and writing it up in 6 hours total is not a writing problem, it's a 2-3 week engineering project wearing a blog post costume. Rushed benchmarks also carry asymmetric risk: one methodology nitpick from a competitor on launch morning turns your keynote into a thread about your flawed test, not your product. Park it — do it properly post-keynote as its own launch.
- **(b) eBPF is overhyped** — you have an opinion, not a piece. A contrarian technical take needs specific claims, numbers, and named counter-examples to survive Hacker News, or it reads as vibes. Six hours is enough to write a hot take, not enough to defend one. Do this one later, when you can spend real research hours making the argument bulletproof.
- **(a) Outage postmortem** — the analysis is already done. Your 6 hours go entirely into shaping and writing, not investigating. It's the only option where "writing hours" is actually the bottleneck you're solving for.

## Scoring (per content-strategy rubric)

| Idea | Customer Impact 40% | Content-Market Fit 30% | Search/Share Potential 20% | Resources 10% | Total |
|---|---|---|---|---|---|
| (a) Outage postmortem | 8 | 9 | 8 | 10 | **8.5** |
| (b) eBPF overhyped | 6 | 6 | 8 | 3 | 6.1 |
| (c) Competitor benchmark | 9 | 9 | 9 | 1 | 6.4 |

(c) scores well on paper and still loses — resource fit is a hard gate, not just a weighted factor, when the deadline is fixed and the harness doesn't exist.

Postmortems are also the strongest format for a keynote-day drop specifically: a cloud-conference audience is disproportionately SREs/platform engineers, and public incident writeups are one of the few B2B formats that reliably earn organic shares and backlinks *because* they're honest, not because they're promotional. Shareable (peer-audience trust) and searchable (people search your product name + "outage" / "status" regardless) at once.

## 6-hour build plan

| Time | Task |
|---|---|
| Hour 1 | Convert internal timeline into a clean chronological skeleton. Strip internal jargon, ticket links, Slack handles. Mark the 3-4 moments that need the most narrative work (detection, misdiagnosis if any, the fix, the "why it won't recur"). |
| Hour 2 | Write Impact + Timeline sections (below). This is mostly transcription + clarity pass, fastest hour. |
| Hour 3 | Write Root Cause + What We Fixed. This is the hour that needs actual thinking — don't rush it. |
| Hour 4 | Write intro/summary + "What we're changing" + close. Draft all 10 title options now while the piece is fresh. |
| Hour 5 | Edit pass 1 (structure/cut) + edit pass 2 (line edit, kill hedging language legal/PR tends to inject). |
| Hour 6 | Final read aloud, pick title, write meta description, prep social pull-quotes, schedule for keynote morning. |

Buffer: none built in on purpose — if you overrun, cut scope from "What We're Changing" (make it 3 bullets, not a roadmap), never from the timeline or root cause. Those are the trust-building core.

## Post structure

1. **One-paragraph summary** — what broke, how long, who was affected, current status. Front-load this; don't make people scroll to find out if it's still happening.
2. **Timeline** — timestamped, plain language, no internal tooling names unless load-bearing to the story.
3. **Root cause** — the actual technical explanation. This is the section engineers screenshot and share — don't soften it into PR-speak.
4. **What we fixed / what we're changing** — concrete, not "we take reliability seriously." Specific guardrails, specific dates if you have them.
5. **Close** — short. Thank the team, link to status page, invite questions.

## Title options

1. What broke, and why, on [date]
2. Postmortem: the [duration] outage on [date]
3. We had an outage. Here's exactly what happened.
4. Anatomy of an outage: [root cause in 4-6 words]
5. [Date] incident report: root cause and fixes
6. How a [root cause, e.g. "single retry loop"] took down [system] for [duration]
7. Our [date] outage, in full
8. What [duration] of downtime taught us about [specific lesson]
9. The [date] incident: timeline, root cause, and what changes now
10. Incident retro: [date]

Pick #3 or #6 if you want shareability (concrete, human); pick #2 or #5 if you want it to read as reference material people bookmark and link to from status pages.

## Morning-of checklist

- Publish before the keynote goes live, not during — you want it indexed/shareable the moment anyone searches your name.
- Have the presenter reference it by name/URL on stage once — turns keynote audience into readers directly.
- Pre-write 2-3 social posts pulling the summary paragraph and the root-cause line verbatim (subheads written to stand alone, per distribution discipline) — post the moment the talk ends, not after.
- Don't bury the incident's actual severity to look better — an audience of engineers can tell, and it's the section most likely to get screenshotted either way.

**One risk to flag to whoever owns comms/legal:** postmortems sometimes get watered down in review until the root cause section says nothing real. Get sign-off on the root-cause paragraph specifically, early, not in hour 6 — that's the one section where a late rewrite blows your buffer.