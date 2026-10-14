Short answer: no, don't drop any of them — and none of the three should carry a weekly minute budget at all. Here's the reasoning per source, plus what I couldn't verify live.

## Why "no dates anywhere" isn't the abandonment signal it feels like

All three are **evergreen reference works, not feeds** — a docs framework, a maintainer-guides collection, and a practice-hub's guide library don't get rewritten on a cadence, so they have nothing to be "stale" against. Forcing a staleness verdict on them is a known failure mode: judge them on *who's maintaining them*, not *when they last posted*.

⚠️ One honest caveat before the verdicts: I don't have live web access in this session (WebFetch/WebSearch and even `curl` were blocked), so I can't re-fetch these three pages right now. What follows is built from a source audit already on file for your radar, whose entries were last confirmed mostly in Aug–Sep 2026 — recent, but not fetched live for you today. I've flagged exactly where that matters.

## The three, individually

**Diátaxis (diataxis.fr)** — ✅ keep, docs/enablement pillar (direct fit for what you own)
- The prior audit couldn't confirm who's behind it from the page itself ("not stated on the fetched page"). That's the weakest ownership signal of the three, worth closing.
- 90-second manual check to do yourself: open `github.com/diataxis/diataxis-documentation-framework` (or search "diataxis github"), check the last commit/merged-PR date. A framework repo with commits in the last few months = alive; if it's been silent 2+ years, downgrade your confidence, but still don't delete — the framework itself doesn't rot just because nobody's editing it.
- What it's selling: nothing visible. Nobody funds it through you reading it, which is a mild point in its favor.

**Open Source Guides (opensource.guide)** — ✅ keep, community pillar (direct fit)
- Backed by GitHub directly (platform-official) — the least likely of the three to be quietly abandoned, since it's an institutional property, not a side project.
- Same gap: the audit recorded "none rendered" for dates and never independently checked repo activity. Manual check: `github.com/github/opensource.guide`, look at last push / last merged PR.
- What it's selling: GitHub's own view of "how to run an open-source project" — mildly GitHub-centric by nature, still broadly useful for a workflow-engine maintainer.

**developerrelations.com guides section** — ✅ keep, but with a real caveat, spans all four pillars
- This one has the best corroborating evidence of the three: a copyright range running through 2026, and — at last check — an upcoming DevRelCon New York listing, independently confirmed via MLH's own announcement (MLH took over the conference after Hoopy, the founding agency, wound down in 2026).
- Catch: that DevRelCon date was 22–23 Jul 2026. Today is 2026-09-23 — that proof-of-life signal is now two months stale itself. It doesn't mean the guides died, it means the specific evidence I have for "someone's still tending this" has aged past its own shelf life.
- Real open question the prior audit never resolved: now that Hoopy (the agency that ran the site) has wound down, who is actually maintaining developerrelations.com day to day? The guides being unchanged isn't suspicious on its own (evergreen), but "founding org closed, no named successor for the site itself" is a different risk than "framework has no dates." Worth a 2-minute look next time you're there: is there a newer event listed than July, or any 2026 H2 activity anywhere on the site?

## The budget: zero minutes/week, for all three

This is the actual answer to your second question, and it's not a rounding-down of a small number — it's a category difference. Evergreen references sit outside the weekly budget entirely; giving them a per-week ratio would be false precision, since you don't re-read a framework every week the way you skim a newsletter.

What to budget instead:
- **One-time read**, order of magnitude: tens of minutes each, once, if you haven't actually read them cover to cover.
- **Recheck cadence**: fold a maintenance-signal glance into whatever refresh rhythm you already run on your radar (quarterly is the field-standard default) — 2 minutes each, not a re-read. Confirm a repo still has recent commits, or the guides page didn't move / didn't vanish behind a paywall. That's it.

Don't put any of the three on your weekly or monthly rotation line. If you're tracking total weekly minutes against a budget, these three should show as a separate "reference, no ongoing cost" bucket, not eat into the number you're trying to keep under your actual time ceiling.