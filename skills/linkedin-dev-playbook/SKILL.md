---
name: linkedin-dev-playbook
description: "Runs a devtool vendor's, founder's or developer advocate's LinkedIn presence to reach engineers and technical buyers without sales spam - go-or-skip verdict, company Page versus personal profiles, newsletters, Events, employee advocacy without pods, what the feed rewards, invitation and posting limits. Use whenever someone asks how to grow a devtool company on LinkedIn, why their devtool's Page gets no reach, whether their engineers should post for the company, how to run employee advocacy, link in post or in comments, or how to earn CTO attention organically - even if they only say 'should we bother with LinkedIn'. Not for outbound sales, paid campaigns or a personal job search. Do NOT use for hiring-brand strategy - use samber/developer-relations-skills@tech-employer-branding."
license: MIT
metadata:
  author: Samuel Berthe
  version: "1.0.0"
---

# LinkedIn Developer Playbook

You run a devtool vendor's, founder's or advocate's presence on LinkedIn: a feed that ranks each post for each reader, reads the author's headline and history as part of the post, and now filters bait, pods and automation by name. Reach travels through named people; the company Page is the hub they point to.

Label every LinkedIn claim: **[LinkedIn]** when LinkedIn Help, its Legal pages, its Engineering blog, its Pressroom or a named LinkedIn employee states it; **[law]** for regulators (FTC, ASA); **[LinkedIn, via press]** or **[LinkedIn, via practitioner relay]** when a LinkedIn employee is quoted second-hand; **[practitioner]** for vendor datasets, agency guides, company handbooks and your own reading of a LinkedIn rule. Never present a practitioner number as a LinkedIn rule. Limits and features move several times a year: before quoting a number, re-check it in [references/platform-facts.md](./references/platform-facts.md) and, when you can browse, on LinkedIn Help.

## Refuse these moves

| Tempting move                                                                              | Why it fails                                                                                                                                                                                                                                                                |
| ------------------------------------------------------------------------------------------ | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Make the company Page the main channel and have staff reshare its posts                    | The ranker puts author name, headline and company into every post [LinkedIn]; Page posts reach a fraction of personal posts [practitioner]. Invert it: people post, the Page reshares                                                                                       |
| A Slack channel or rota where staff like and comment on each new post                      | Policy bans agreeing "ahead of time to like or re-share each other's content" [LinkedIn]; LinkedIn is working to make pods ineffective [LinkedIn]. Tell staff a post exists; never assign the reaction                                                                      |
| Forty employees paste the same copy at 9:00                                                | "Gratuitously repetitive" content is spam [LinkedIn] and it reads as a pod [practitioner]. Each person writes from their own role                                                                                                                                           |
| Auto-commenting, auto-connect, scheduled DM sequences, scraping or engagement extensions   | User Agreement 8.2 bans automated commenting, liking, sharing, messaging and scraping; accounts get restricted [LinkedIn]. Native scheduling of your own posts is fine                                                                                                      |
| Connection invitation with a pitch, or a demo ask in the first DM after accept             | Invitations may not carry promotional messages to strangers [LinkedIn]; ignored or reported invites lead to restrictions [practitioner]; LinkedIn advises Follow, not Connect, for people you do not know [LinkedIn]                                                        |
| "Comment YES for the PDF", "Agree?", reaction polls, gated giveaways                       | Named as engagement bait LinkedIn filters [LinkedIn]; reaction polls are listed spam [LinkedIn]                                                                                                                                                                             |
| Quote a link penalty ("links cut reach 60%") or say 360Brew runs the feed                  | LinkedIn says it does not intentionally limit link posts [LinkedIn, via practitioner relay]; practitioner datasets range from -50% to positive and the 60% figure has no source [practitioner]; LinkedIn said 360Brew is not in the feed [LinkedIn, via practitioner relay] |
| Publish posts or comments written for staff (by marketing or AI) that they did not rewrite | Generic AI content gets less distribution and readers can flag it as "AI slop" [LinkedIn]. Drafts you write are starting points the person rewrites in their own voice; LinkedIn recommends saying so when AI did most of the work [LinkedIn]                               |
| Set up the My Company tab, the Employee Advocacy tab or Creator Mode                       | All removed: the advocacy tab and curator role from November 2024 [LinkedIn], the Creator Mode toggle in 2024 [practitioner]                                                                                                                                                |
| Promote the product inside a LinkedIn article or newsletter edition                        | LinkedIn's publishing guidelines call product and service promotions inappropriate in articles [LinkedIn]; a newsletter edition is an article. Teach in the edition; name your employer in the opening and link the product once as context                                 |
| Post sponsored, gifted or affiliate content without the label                              | Anything posted for value, free product included, needs the brand partnership label [LinkedIn]; employee posts about the employer's product need the relationship disclosed [law]                                                                                           |
| Report impressions or followers as the result                                              | Both are estimates of exposure [LinkedIn]; measure buyer-role reach, comments from target roles and attributed signups                                                                                                                                                      |

## Match the answer to the request

- Narrow question (link in post or comment, why the Page is flat, can we schedule, is our rota a pod): answer it, plus the refusals and limits it touches.
- Presence setup, launch, "no reach", or advocacy program design: run the Interview, then deliver the full Output.
- A direct question inside a setup request: answer it in the first lines, then the full Output.
- Outbound sales sequences, Sales Navigator prospecting, paid campaigns and ad targeting are outside this skill; say so. Thought Leader Ads appear here only as amplification of posts that already worked.
- A recurring LinkedIn Live show belongs to samber/developer-relations-skills@devrel-live-streaming; hiring posts serve the plan in samber/developer-relations-skills@tech-employer-branding and go out from a person's account like any other post.

## Interview

One question per message; skip what the request answers. When the user wants one pass, state an assumption per gap.

**Memory: Store a few info about the current task or project context. When memory lives in a file, use `devrel-context.md`; if a different memory system is in use, rely on that instead. Separate task info in different sections. Remove finished tasks. Add a date to a task; no date for general project context. Some interview responses may differ between 2 tasks.**

1. Goal: launch, standing awareness with buyers, a newsletter audience, or rescuing a Page nobody sees?
2. Who signs and who uses: name the buyer roles (CTO, platform lead, engineering manager) and the user roles, and whether a team purchase or procurement exists at all.
3. Who will post under their own name, with what headline today, and how many hours a week each?
4. What already exists: Page followers, founder followers, a newsletter, a backlog of technical material (benchmarks, postmortems, design docs).
5. Constraints on staff posting: comms or legal review, disclosure rules, works councils, anyone who must not be named.
6. Date the result must land by, one-off moment or compounding asset, and the effort ceiling.

## Go or skip

Return one verdict before any setup:

- **Skip**, and say so, when either holds:
  - nobody can post as a named person for at least an hour a week;
  - users are individual developers who buy alone, no team purchase or hiring goal exists, and the user's own stack communities live elsewhere. Point to where they already are: samber/developer-relations-skills@discord-community-playbook, samber/developer-relations-skills@hackernews-playbook or samber/developer-relations-skills@bluesky-dev-playbook.
- **Go small** (one person commenting, Page kept complete, no posting program) when nothing shippable exists yet or legal will not clear employee posts this quarter.
- **Go** otherwise, and first when a team, platform group or CTO signs the purchase.

A skip verdict is a valid output, not a failure. Revisit it when a hiring goal, a corporate sponsor or a team buyer appears.

## Accounts and identity

- **People first.** The founder, one or two advocates and the engineers who built the thing post under their own names. The ranker reads each poster's headline and history as part of the post [LinkedIn], so one topic per person beats everyone posting about everything.
- **Three to six posters**, whatever the headcount [practitioner]; everyone else comments only when they have something to add.
- **Headline names the topic and the employer:** "Staff engineer at Tidewire - schema evolution for Kafka", not "Passionate about innovation". The headline is context, never the disclosure: regulators want the relationship inside the post itself, before "see more" [law].
- **The Page is a hub:** complete Overview, industry, size and website; release posts and the changelog; reshares of employee posts; one or two original posts a week (LinkedIn reports Pages posting weekly see 2x higher engagement, comparator unstated [LinkedIn]); daily Page posting takes hours that buy more as people's posts; the Page newsletter once it passes 150 followers and LinkedIn's evaluation [LinkedIn]. A personal profile run as a brand gets restricted [LinkedIn].
- **Showcase Pages** only for a distinct long-term audience, never per campaign or region; employees cannot attach to them [LinkedIn]. Product Pages exist for B2B software [LinkedIn]; a Product Page sits on the parent Page's Product tab, so followers stay on one Page [practitioner].
- **Follow button:** posters who want strangers to follow turn on "make follow primary" in visibility settings [LinkedIn]; pin the best technical post in the Featured section.
- **Comment as people.** The Page comments only for official replies on its own posts, partner posts or support questions [practitioner]; Pages can comment and react [LinkedIn].
- **Verify** the Page and each poster's workplace: LinkedIn ties posting and comment capacity and visibility partly to verification [LinkedIn].

## Engagement surfaces, ranked by value per hour

- Efficiency: substantive comments on posts your buyers already read > the builder's own technical posts > Page reshares of those posts > a newsletter > original Page posts > Events and Live > Page follow invites > Groups.
- Value: newsletter > builder's own posts > comments > Events and Live > Page reshares > original Page posts > Page follow invites > Groups.
- Compliance cost: comments == own posts == Page posts (no review beyond the poster) < an employee program (disclosure training, voluntary participation) < paid creator posts (brand partnership label, endorsement law). Comments and own posts tie because each person speaks for themselves under their own name.

| Surface                       | Payoff       | What it buys                                                                                                                              |
| ----------------------------- | ------------ | ----------------------------------------------------------------------------------------------------------------------------------------- |
| Comments on others' posts     | compounding  | Profile visits and follows from people already in the conversation; LinkedIn now judges the comment itself, not likes on it [LinkedIn]    |
| Builder's own technical posts | compounding  | Out-of-network reach for knowledge content [LinkedIn]; text plus a diagram or a document post on profiles [practitioner]                  |
| Page reshare of a staff post  | one-shot     | Followers see the person, the person gets the reach; reshare as the Page [LinkedIn]                                                       |
| Newsletter (person or Page)   | standing job | Each edition sends push, in-app and email to subscribers [LinkedIn]; new followers get a subscribe invite [LinkedIn]                      |
| Original Page posts           | compounding  | An archive buyers check; documents and multi-image do best on Pages [practitioner]                                                        |
| Events and LinkedIn Live      | one-shot     | Reminders at 7 days, 3 days and 15 minutes [LinkedIn]; Live must be a scheduled Event [LinkedIn, via press]                               |
| Page follow invites           | one-shot     | Followers from your connections; monthly credits, now scarce [LinkedIn, practitioner]                                                     |
| Groups                        | rare         | Posts stay in the group feed; owners may recommend one post a week, which emails members [LinkedIn]; no evidence groups still drive reach |

- **Starved option:** the newsletter scores highest on value but costs a standing job, so efficiency never picks it. Promote it when one person owns a monthly edition and three editions of material already exist.
- **Default rung:** three weeks of daily substantive comments by the named posters, then one technical post a week each.
- Re-rank against the user: a founder with a large following starts at own posts; a hard launch date promotes an Event plus the builder's post; a comms team that already reviews posts lowers the cost of an employee program.

## What the feed rewards and punishes

**Do more:**

- Posts that hold attention: the ranker predicts long dwell and skips [LinkedIn]. Lead with the finding, then the numbers, the code or the trade-off.
- Comments that add a fact, a counter-case or a question the author can answer; replies to every substantive comment on your own post [LinkedIn: comments are a ranked action; practitioner: threads lift reach].
- Posts that stand alone without the click; put the link last, or in the first comment, and measure which works for that person [LinkedIn denies a link penalty; practitioner datasets disagree].
- A visual that carries the same message as the text; LinkedIn shows less video unrelated to its post [LinkedIn].

**Do less:**

- Bait, recycled thought leadership and click-driven repetition [LinkedIn].
- Posting several times a day; LinkedIn may cap posts and comments and lower visibility for excess [LinkedIn]. Two to five posts a week per person is the practitioner range [practitioner].
- Hashtag stacks: no measured reach effect, six or more hurt [practitioner].

## Employee advocacy without a pod

1. Make it voluntary, and say so in writing.
2. Brief, never script: send the release, the hard problem, the data, and who built it ([references/templates.md](./references/templates.md) § Employee brief). Each person writes from their own role or skips.
3. Tell staff a post exists in a channel that never asks for reactions, and name the channel for reading, not boosting. Allowed: the Page resharing a staff post, and "Notify employees", capped at once a day [LinkedIn] and best used at most weekly as LinkedIn's Pages guide advises [LinkedIn]; it reaches a relevance-picked subset [LinkedIn]. Not allowed: any assigned like, comment or reshare by a person, whatever the timing.
4. Disclosure: employees who promote the product name the relationship in the post text, before "see more"; a headline or bio alone does not count; an employer that directs posts trains and monitors them [law].
5. Anything given for posting (swag, credits, contest points) needs the brand partnership label and disclosure [LinkedIn, law].
6. Amplify the best organic staff post with a Thought Leader Ad only after it performed; LinkedIn labels it promoted [LinkedIn].
7. Tools: since the native tab is gone, LinkedIn points to Marketing Partners [LinkedIn]. Use tools that suggest content and track links; refuse any that post, comment or react for people [LinkedIn].

## Cadence

1. **Week 0:** headlines and About sections name topic and employer; Page complete; disclosure line agreed; each poster lists 20 people whose posts their buyers read.
2. **Weeks 1-3:** each poster leaves substantive comments on weekdays, no product links unless asked; no own posts yet except a short introduction.
3. **Week 4 onwards:** one technical post a week per poster; the Page reshares each within the first hours with one line of context [practitioner: early engagement matters; LinkedIn: embeddings refresh within minutes].
4. **Month 2:** first newsletter edition if the starved-option condition holds; Page follow invites spent only on technical connections.
5. **Launch day:** the builder posts the story with the numbers; the Page reshares and notifies employees once; staff comment with questions or details, never "congrats team"; the link goes last or in a comment; pin the most useful reply [LinkedIn: comment pinning]. Answer hostile technical comments once, with facts, from the builder's account; never from the Page, never with staff piling on.
6. **Monthly:** review metrics, check "Seems like AI slop" tips in post analytics [LinkedIn], drop surfaces that returned nothing.

## Limits

Full per-surface table with kind and source: [references/platform-facts.md](./references/platform-facts.md) § Limits. The ones that bind most often:

- Post 3,000 characters; document post 300 pages and 100 MB; scheduling up to 3 months ahead [LinkedIn].
- Connection invitations: no published weekly number; about 100 a week observed, and restrictions follow ignored or reported invites [practitioner]; send only to people you know [LinkedIn].
- Page follow-invite credits: monthly, shared by all admins; LinkedIn publishes no number; practitioners report a cut from 250 to 50 in 2026 [practitioner].
- Employees who are not Page admins may invite 30 connections a month to follow the employer's Page [LinkedIn].
- Page newsletter: more than 150 followers makes the Page eligible for evaluation, not automatic; up to 5 newsletters per author [LinkedIn].
- Groups: each group's owners set what is allowed; most ban promotion outright and approve posts first [LinkedIn: owner rules; practitioner: prevalence].

## Output

For a full plan, deliver in order: the direct answer when the user asked one; go/skip verdict with the reason; refusals triggered; posters with their topic and headline; surfaces ranked for this user with what moved the order; the advocacy rules if staff post; dated cadence; limits that bind, each with kind and source; matching templates from [references/templates.md](./references/templates.md) adapted to the user; metrics. Mark every drafted post or comment as a draft for the named person to rewrite, and run it through your preferred humanizer skill.

## Metrics

- Share of post viewers in buyer roles, from post analytics demographics (job title, seniority, company size) [LinkedIn]; this is the reach number that matters. Demographics appear only once a post has enough viewers [LinkedIn], so read it monthly across posts, never as zero on a small one.
- Comments from target roles per post, not reactions.
- Newsletter net subscribers per edition.
- Link visits [LinkedIn] plus UTM-tagged sessions and signups, and a free-text "how did you hear about us" field, since click data under-counts feed influence [practitioner].
- Page engagement rate counts clicks, many vendor benchmarks do not [LinkedIn, practitioner]: never compare the two.
- Stop signal: "Seems like AI slop" tips, invitation restrictions, or comments calling a post an ad.

## References

- samber/developer-relations-skills@devrel-content-calendar - fitting LinkedIn slots and posters into the quarter.
- samber/developer-relations-skills@build-in-public - the founder's weekly narrative these posts often carry.
- samber/developer-relations-skills@devrel-newsletter-program - whether a newsletter is worth running and who owns it.
- samber/developer-relations-skills@oss-launch - the cross-channel launch plan the launch-day post belongs to.
- samber/developer-relations-skills@devto-playbook - where the long-form post behind a LinkedIn summary lives.
- samber/developer-relations-skills@devrel-analytics - attributing LinkedIn referrals to signups.
