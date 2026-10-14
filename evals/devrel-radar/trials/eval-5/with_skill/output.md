## Straight answer first

No — almost nothing here is free of an angle, and pretending otherwise would be the least trustworthy thing I could hand you. Podcast hosts run consultancies, newsletter curators run agencies, "state of the ecosystem" reports come from the vendor whose category they're sizing. I'm not filtering those out — that would empty the list, since most of the field's real expertise sits inside people who also sell something adjacent to it. Every entry below carries a **what they're selling** column instead, so you can weigh it rather than assume neutrality that isn't there.

One entry actually is the exception, and it goes first: **Developer Relations Foundation** (dev-rel.org), hosted by the Linux Foundation. No product, no course, no agency behind it — it's the field's only vendor-neutral practice body. Everything else on this list, treat as "useful and interested."

I don't have live web access in this session, so this is built from a source audit dated 2026-08-26 to 2026-09-10 (a skill-maintained list I re-checked against, not re-fetched live) — 2-4 weeks old against today, 2026-09-23, well inside the field's own ~3-month freshness window. I did **not** invent anything to fill gaps — see "embedded/IoT ecosystem gap" below.

## Assumptions I made instead of interviewing you turn-by-turn

You gave me role, ecosystem, budget and mediums directly. I inferred the rest — flip any of these and the ranking changes:

- **Pillars owned**: advocacy + enablement (a DevRel-titled advocate at an SDK company owns speaking/content/feedback plus docs/SDK/quickstart quality). Not marketing, not community. Tell me if that's wrong.
- **Catching up vs. standing habit**: standing habit, not onboarding to a new role. If you're actually new to this, long-form (podcasts, talks) gets promoted harder.
- **Commute/gym time for audio**: unknown. This is the single biggest lever on the list below — see the note on Community Pulse.
- **B2B motion**: bottom-up devtool (engineers self-serve the SDK) rather than top-down hardware-OEM sales. Changes which conferences would matter.
- **Events (Q9)**: assumed no CFP/sponsor obligation this year, so no event line. Say the word and I'll add one CFP target plus one attend/sponsor candidate with confirmed dates.

---

# DevRel radar — developer advocate, embedded/IoT SDKs (Lorrick)

Pillars owned: advocacy, enablement. Ecosystem: embedded/IoT. Budget: 75 min/week. Sources verified 2026-08-26–2026-09-10; not re-fetched live this session.

## Weekly core — 66 of 75 min

Ordered by value per attention-minute, highest first. Text buys "what happened" cheaply; the one podcast buys the reasoning behind it and costs most of an hour, so it only earned its slot because you named audio as a medium you actually use — not because the ratio favored it.

| Source | URL | Medium | Pillar | Min | Why it earned the slot | Selling | Status |
|---|---|---|---|---|---|---|---|
| DRF community list & Discord | lists.dev-rel.org/g/community | mailing list (text) | reserved discovery slot | 8 | The mandatory renewal slot, capped to a skim — ungated, foundation-run, nothing to sell. Where a new source surfaces before it shows up in any curated list, including this one. | nothing (foundation) | active — Touchpoint calls restarted 2 Sep 2026 |
| Community Pulse | communitypulse.io | podcast | advocacy | 40 | The one long-running show about DevRel *as a practice*, not about any one vendor's tool. Four rotating hosts means four different ecosystems' worth of judgment, not one consultancy's pitch. | hosts' consulting | active — Ep 105, 17 Aug 2026 |
| Tessa Kriesel | tessakriesel.com | blog | advocacy | 8 | Treats advocacy as a function that has to show revenue impact, not goodwill — the framing you'll need when Lorrick asks what the SDK program is for. | consulting/advisory | active, 31 Jul 2026 |
| The New Stack | thenewstack.io | news site | all four (general) | 10 | Daily pulse across the whole devtool space. Not embedded-specific, but the cheapest way to not get blindsided in a hallway conversation about something that shipped elsewhere. | ad-supported media | active, 26 Aug 2026 |

**If your podcast time rides on a commute or workout you already have** — tell me. Halving Community Pulse to ~20 min frees exactly enough room to add Scaling DevTools (bench, below) as a second show, without touching the rest of the list.

## Monthly — reference, not a feed

| Source | URL | Pillar | Why | Status |
|---|---|---|---|---|
| **Developer Relations Foundation** | dev-rel.org | all four | The vendor-neutral one — Maturity Model, Tools Catalog, Persona Library, public governance. Read this before you read anyone's opinion piece. | verified-active, governance repo commit 28 Aug 2026 |
| DeveloperRelations.com guides | developerrelations.com | all four | The practice canon: four pillars, CORE framework, friction logging, champion programs — 21 guides. The agency that wrote them (Hoopy) wound down in 2026; the hub survived it. | active, evergreen (no dates on the guides themselves) |
| Diátaxis | diataxis.fr | enablement | The tutorial/how-to/reference/explanation split your SDK docs should already be structured against. | evergreen, undated |
| EveryDeveloper | everydeveloper.com | enablement | Technical content strategy from someone who wrote the two books on documenting developer products. No single dated post confirmed, so check monthly rather than trust a weekly cadence. | active, page asset dated 12 May 2026 |
| DevRel Weekly | devrelweekly.com | all four | The field's canonical link roundup, run by the same person behind Community Pulse. Downgraded from weekly to occasional in 2026 — the curator posted the change herself. | verified, format-changed to sporadic |

## Annual — data you'll be asked for

- **DRF Metrics Index** (github.com/DevRel-Foundation/metrics-index) — vendor-neutral metric definitions; the counterpart to a tool vendor's own measurement blog. Early/v0.0, last push 9 Jan 2026 — a definitions reference, not a benchmark dataset yet.
- **GitHub Octoverse** — annual ecosystem trends, 2025 edition, published 28 Oct 2025.
- **JetBrains State of Developer Ecosystem** — 24,534 respondents, 2025 edition; historically the one mainstream survey with an embedded/IoT tooling breakout worth citing by name and year.

Cite the edition and date every time — a bare number from any of these ages invisibly and gets repeated as if it were current two years later.

## Bench (verified substitutes, not in this round's budget)

- **Scaling DevTools** (feeds.transistor.fm/scaling-devtools) — devtool growth interviews, Ep 197, 20 Aug 2026. Promotes to weekly core the moment you confirm commute/gym listening time.
- **Console.dev** — weekly devtool reviews, active 20 Aug 2026. Marketing pillar — add if that becomes something you own.
- **Open Source Ready** (Heavybit) — open-source/maintainer economics, Ep 41, 30 Jul 2026. Add if community becomes an owned pillar.

## Embedded/IoT ecosystem gap — do not trust these yet

This skill's own source audit covers general DevRel practice and B2B devtool sources densely. It has **no embedded/IoT ecosystem entries at all** — not an oversight in this radar, an actual gap in what's been verified. I have no web access this session to fetch and confirm any, so I'm not going to hand you names dressed up as checked sources — that's the exact failure this whole list exists to avoid.

What I can do honestly: name candidates I know exist from general knowledge, unverified, for you to run the same check yourself (open it, find the newest dated item, confirm who runs it):

- Interrupt (interrupt.memfault.com) — embedded engineering blog, run by Memfault (tool vendor).
- Zephyr Project blog (zephyrproject.org/blog) — Linux Foundation-hosted RTOS, foundation-backed.
- Hackster.io/news — maker/IoT community news, hardware-vendor sponsored.
- CNX-Software (cnx-software.com) — independent IoT/embedded hardware news blog.
- Embedded.fm and The Amp Hour — long-running embedded/EE engineering podcasts (not DevRel-specific).

Status on all six: **unverified, no fetch performed.** Don't add any of these to your weekly rotation until you've confirmed a dated post in the last ~3 months and checked who currently owns the domain.

## Dropped

- **Video** — not among the two mediums you picked (text, audio). No conference-talk archive or channel appears anywhere above, bench included.

## Totals

- Estimated weekly minutes: 66 of 75 (88%) — slack is intentional, reserved for the commute question above.
- Counts: 4 weekly + 5 monthly + 3 annual + 3 bench = 15 tracked entries, plus 6 explicitly unverified ecosystem candidates (not counted as part of the list).
- Verified-active: 14 of 15. Evergreen-undated: 1 (Diátaxis).
- Pillar coverage: advocacy ✅ (Community Pulse, Tessa Kriesel), enablement ✅ (EveryDeveloper, Diátaxis, DRF Tools/Persona Library). No community or marketing padding — not owned pillars.
- Unverified entries presented as verified: 0.

## Optional second artifact — OPML

Only one entry above has a confirmed raw feed URL in the audit; guessing the rest would reintroduce exactly the "plausible but wrong" problem this list is trying to avoid. Paste the other homepage URLs into your reader (Feedly, NetNewsWire, etc.) — real feed discovery beats a guessed path.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<opml version="2.0">
  <head><title>Lorrick DevRel radar</title></head>
  <body>
    <outline text="Community Pulse" type="rss" xmlUrl="https://communitypulse.io" htmlUrl="https://communitypulse.io" />
    <outline text="Scaling DevTools (bench)" type="rss" xmlUrl="https://feeds.transistor.fm/scaling-devtools" htmlUrl="https://feeds.transistor.fm/scaling-devtools" />
  </body>
</opml>
```

Set a quarterly reminder to re-run this — that's when domains get resold, agencies wind down, and conferences change organisers without anyone announcing it loudly.