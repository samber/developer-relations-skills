**Using the `engineering-blog-post` skill (samber/developer-relations-skills).** This is an incident-derived post, so the skill requires a redaction pass before drafting — that's step one below.

**Mode / pattern:** Draft mode. Pattern = **Bug Hunt** (you have a timeline + a genuine surprise: a partial, not total, write failure). No benchmark, no migration numbers, so the other patterns don't fit.

**Assumptions I made instead of running the full interview** (flag if wrong): reader = a developer already using the service, publishing on the company engineering blog, this week's deadline rules out gathering new evidence, blameless framing throughout.

## Redaction pass (per the skill's "Incident-derived posts" rule)

Removed, not softened — these don't belong in any public draft regardless of impact-figure decisions:

| Internal detail | Public treatment |
|---|---|
| `PROJ-8812` | Cut |
| `mreynolds` | Cut — no individual named, blameless framing |
| `change #4471` | Cut — "a routine config change" |
| `Falcon` | You already gave the safe public name in your own message — "our routing tier" |
| `go/falcon-postmortem-aug3` | Cut — internal wiki link |
| "two enterprise accounts" | **Cut, deliberately.** Enterprise accounts are a small, identifiable population; naming that segment (even without naming the accounts) adds re-identification risk with no benefit to a reader trying to understand the failure. This is a call for whoever owns customer confidentiality, not something to decide by default — flagging it, not silently deciding it. |
| 34% of writes, 51 minutes, ~1.2M requests, 118 customers | **Kept**, as aggregate, non-identifying figures. This is the "decide deliberately" fork the skill asks for — my default is publish these, since they carry the severity a reader needs and identify no one. |

## Claim ledger

| Claim as drafted | Class | Evidence / action |
|---|---|---|
| "error rate reached 34% of writes" | Measured | Needs the actual dashboard screenshot or metric name — **placeholder in draft** |
| "lasted 51 minutes" | Measured | Same — pin to incident start/resolution timestamps |
| "roughly 1.2M failed requests" | Measured | Source says "estimated" — kept marked as an estimate, not a precise count |
| "affected about 118 customers" | Measured | Needs the query/dashboard that produced this count |
| "root cause: a config reload path read the wrong key" | Shown | Needs the actual before/after config diff or the offending resolution code — **not in the material you gave me, so I could not draft this as fact-checked; placeholder inserted** |
| Everything under "What we changed" | — | **Not in your source material at all.** I have not invented a fix. See placeholder in the draft and the handoff list. |
| "a deploy that looks clean can still wake up an untested code path" | Opinion | Kept, marked as the generalizable lesson |

Two things didn't survive: the enterprise-account detail (cut) and any specific remediation (never existed in the input, so nothing to cut — it's just missing).

## Outline (Bug Hunt, ~1,000 words — shorter than the 1,500–2,500 baseline because the condensed postmortem you pasted has no investigation narrative to fill 30% of the post)

Symptom 10% → First hypothesis 15% → Investigation 30% (thin, placeholder-heavy — this is exactly the part your full internal postmortem almost certainly has and this condensed version cut) → Root cause 20% → Fix 15% (placeholder) → Lesson 10%.

## The draft

---

**Suggested title:** Why a routine config reload dropped a third of our writes for 51 minutes
**Suggested slug:** `/blog/config-reload-write-failures-incident`
**Suggested meta description:** A routing-tier config reload path resolved the wrong key during a routine deploy, failing 34% of writes for 51 minutes. Here's what we found and what we're changing.

---

# Why a routine config reload dropped a third of our writes for 51 minutes

On August 3, 2026, our routing tier — the layer that sits in front of every write to our platform — failed roughly a third of writes for 51 minutes. Not all writes, and not zero: a partial, sustained failure rate is a stranger thing to debug than an outright outage, and it's the reason this post exists.

## What we saw

At 14:02 UTC *[confirm timezone]* we shipped a routine, low-risk-looking configuration change to the routing tier. Within minutes, our error-rate alerting fired: write failures climbed to and held at roughly 34% for the next 51 minutes before the service recovered.

*[Insert the actual error-rate graph and the exact alert/error text here — a reader searching for this failure later should be able to find this post by pasting in the error string they're seeing.]*

By the time it resolved, we'd failed an estimated 1.2 million requests, affecting around 118 customer accounts.

## First hypothesis: the deploy itself

A step-function change in error rate right after a deploy points at the deploy first, so that's where we looked. We checked the diff being shipped and the health of the newly rolled-out instances.

*[This is where your fuller internal postmortem almost certainly has real detail — what you actually checked, what the dashboards showed, how long this took. I don't have that detail from the condensed version you pasted, so I'm not going to invent it. Drop in the real sequence here.]*

The deploy's own code turned out to be unremarkable. That ruled out the obvious cause and pointed at something the deploy *triggered* rather than something the deploy *was*.

## The investigation

*[This section is the reason readers stay on a Bug Hunt post — the wrong turns, not just the answer. Your condensed postmortem doesn't include this narrative; it's probably sitting in the full internal write-up. Pull it in before publishing: what you tried that didn't pan out, what log line or metric finally pointed at the config reload path rather than the deploy code, roughly how long each step took.]*

What we can say from the material we do have: the deploy triggered a downstream configuration reload — a separate code path from the deploy itself, and one that hadn't changed in this release. That reload path is where the actual bug lived.

## Root cause

The reload path resolved a different configuration key than the one it was supposed to use. Under the old key's value, some fraction of write traffic hit routing state that was stale or absent, which produced failures for that fraction of requests rather than all of them — the partial failure rate we saw is a direct consequence of which requests happened to route through the affected state and which didn't.

*[I don't have the actual key names, the resolution code, or a before/after diff. Insert that here — a reader trying to avoid the same class of bug needs to see the shape of the mistake, not just a one-line description of it.]*

## What we're changing

*[Not in the material you gave me — I'm not going to fabricate a fix. This is the one section that needs a real answer before this goes out: did you add schema validation on reload? Gate the reload behind the same canary/rollout process as a deploy? Add a test that exercises the reload path with a deploy in the loop? Whatever it actually was, it goes here, with enough detail that a reader could tell if their own system has the same gap.]*

## What generalizes

The bug here wasn't in the code we reviewed and deployed — it was in a code path the deploy *woke up* without going through the same scrutiny. Anything that reads configuration at runtime (a reload, a feature-flag refresh, a cache warm) deserves the same review and test coverage as the deploy that can trigger it, not less, because "the deploy was fine" is not the same claim as "everything the deploy touched was fine."

## Where we're still exposed

*[Placeholder for one honest, specific limitation — the credibility gate requires this and I don't have enough from your input to state one truthfully. Something like: other reload paths we haven't audited for the same class of bug, or: this fix covers this key's resolution but not the general reload mechanism. State it plainly; it's what makes the rest of the post believable.]*

---

## Credibility gate report

| Check | Status |
|---|---|
| 1. Claims classified 100% | **Not yet** — Investigation, Fix, and Limitations sections are placeholders pending real detail from you |
| 2. No unsourced numbers | Pass for the four figures kept; each needs its source dashboard linked before publishing |
| 3. Benchmark disclosure | N/A — no comparative numbers in this post |
| 4. One honest limitation stated | **Fails until you fill the placeholder** — I won't invent one |
| 5. Zero unsupported superlatives | Pass |
| 6. Snippets pinned and dated | N/A — no code shown yet; if you add the config diff, pin the version and date it |

This draft is not ready to ship — three sections are honest placeholders, not filler, because the material you gave me doesn't contain what belongs there.

## Handoff — what you need to add before this publishes

1. **Investigation narrative** — the actual wrong turns and how long each took. Pull from the full postmortem behind the wiki link.
2. **The fix** — what you actually changed. This wasn't in your condensed summary at all.
3. **Exact error text and a graph** of the error-rate spike, so this post is findable by someone searching the error.
4. **The config-key mechanism**, specific enough that a reader can check their own system for the same class of bug, without exposing your actual config schema if that's sensitive.
5. **One real limitation** for the "where we're still exposed" section.
6. **A decision, from whoever owns customer confidentiality**, on whether the 118-customer figure ships as-is — I defaulted to yes since it's non-identifying, but that's a call for a human, not me.
7. **Technical review** from an engineer who worked the incident, and a read from someone outside the team for jargon.
8. Confirm the 14:02 timestamp's timezone before it goes out.