## Pick: Bug Hunt, from the outage timeline (a)

**Ranking and strikes, per the skill's decision rules:**

- **(c) Benchmarks — struck outright.** SKILL.md is explicit: *"An effort ceiling that excludes producing new evidence deletes Benchmarks and Rewrote It in X outright — struck from the menu, not parked at the bottom."* A harness that's never been built and never run is new evidence, and it's a quarter of work per the pattern table, not six hours. This isn't a close call — it's a gate, not a preference.
- **(b) Thoughts on Trends (eBPF opinion) — survives fillability, loses the ranking.** A hard date "promotes Bug Hunt and Thoughts on Trends, whose material is already written down or already in your head" — so both clear the deadline test. But the efficiency order (`bug hunt > explainer > how we built it > rewrote it in X > lessons learned > thoughts on trends > ...`) puts Bug Hunt ahead of it by five positions, and the reason tracks your actual material: an outage timeline is a finished narrative spine (symptom → investigation → root cause → fix), while "eBPF is overhyped" still needs its evidence section built from scratch and, per the pattern's own trap list, a steelman good enough that an eBPF advocate would recognize their own position — that's real drafting time against a 6-hour budget, not assembly time.
- **(a) Bug Hunt — wins.** Effort per the table: *"an hour, on top of an incident review already written."* You have the incident review. What's left is the public-facing shell: redaction, a reason for an unaffected reader to care, and the lesson. That fits inside six hours with room for a technical review pass, and it's first on the efficiency list for a reason — *"the widest travel per hour on the list — engineers forward a good mystery."* That's exactly the profile you want live the morning a keynote puts eyeballs on the blog.

## The 6-hour plan (Bug Hunt section budget, scaled to ~1,800 words)

| Hour | Section | Do |
|---|---|---|
| 0.5h | Redaction pass first | Strip names/handles, internal service codenames, ticket IDs, wiki links. Decide deliberately whether impact numbers ship — don't default-copy them from the internal doc. |
| 0.5h | Symptom (10%) | What broke, as observed — error text verbatim (readers search for it), the alert or graph that fired first. |
| 0.75h | First hypothesis (15%) | What you assumed and why it was reasonable. Resist cleaning this up in hindsight — an already-correct first guess reads as reconstructed. |
| 1.5h | Investigation (30%) | The wrong turns, with commands/logs/diffs. This is the section that carries the post's credibility — don't compress it to make room elsewhere. |
| 1h | Root cause + Fix (20% + 15%) | Plain explanation, then the actual change/config. Keep it blameless — conditions that allowed the failure, not who typed the commit. |
| 0.5h | Lesson (10%) + "why should an outsider care" | An internal postmortem never has to earn this — a public one does. One sentence a reader repeats to a colleague. |
| 0.75h | Claim ledger + credibility gate | Classify every number (Measured/Shown/Sourced/Opinion), run `scripts/scan-draft.sh`, confirm at least one honest limitation, zero unsourced superlatives. |
| ~0.5h buffer | Reviewer brief | One engineer who lived the incident (accuracy), plus security/customer-confidentiality sign-off since this touches an outage — start that today, run it in parallel with drafting, not after. |

That's the six hours. The 9 days matters most for the parts that aren't writing time: getting the security/confidentiality clearance and the outside-the-team clarity read back before morning-of.

## What I don't have yet, and need before drafting

Per the skill's interview, these still change the draft even though they don't change the pattern pick:

- **Reader** — developer evaluating your stack, existing user, or peer engineer? Changes what needs explaining.
- **Blog context** — company blog (triggers the disclosure/attribution obligations) vs. personal.
- **What stays unpublished** — the impact figures decision above needs an owner, not just me.
- **Technical reviewer** — who signs off, and are they actually available in the next 9 days.

## On (b) and (c) — not discarded, resequenced

- **eBPF opinion**: cheap fast-follow. Draft it after the keynote post ships, once six hours aren't gated by a hard morning deadline — the steelman needs the time it needs.
- **Benchmark**: this is the one line in SKILL.md worth flagging back to you directly — *"an author who only ever computes efficiency never publishes the one post that would be cited for years."* If the competitive benchmark is a real compounding asset for you, it deserves a quarter and a harness, not a squeeze into this week. Worth scoping as a separate project once the keynote is behind you, not compressed to fit it.