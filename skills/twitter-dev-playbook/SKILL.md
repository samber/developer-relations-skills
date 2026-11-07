---
name: twitter-dev-playbook
description: "Runs a devtool vendor's, developer advocate's or open-source maintainer's presence on X (formerly Twitter) - go-or-skip verdict for developer audiences, people versus brand accounts, paid plans, launch posts, a thread about your product, link handling, what the open-sourced ranking rewards and punishes, posting caps, automation rules, cross-posting to Bluesky, Mastodon and Threads. Use whenever someone asks how to grow a developer audience on X or Twitter, launch a devtool or OSS release there, whether links get penalized, whether to pay for verification, why their devtool's tweets get no reach, or whether their devtool or project should stay on X. Not for account recovery, ads or ranking-code study. Do NOT use for Bluesky presence - use samber/developer-relations-skills@bluesky-dev-playbook."
license: MIT
metadata:
  author: Samuel Berthe
  version: "1.0.0"
---

# X (Twitter) Developer Playbook

You run a vendor's, advocate's or maintainer's presence on X: a network where one ranked feed decides reach, paid status changes who sees your replies, and the developer audience has partly moved elsewhere. Every rule below follows from that.

Label every claim: **[X]** when X's help center, rules, docs, open-sourced ranking code or named staff posts state it; **[X via press]** when only press reports an X statement; **[platform docs]** for another network's or bridge's own docs; **[research]** for peer-reviewed studies and preprints; **[survey]** for developer or population surveys; **[press]** for reported incidents; **[practitioner]** for tool-vendor datasets, third-party code readings and your own reading of a rule. Never present a practitioner claim as an X rule. X changes limits, plans and ranking monthly, and staff statements have contradicted measured outcomes: before quoting a number or date, re-check it in [references/platform-facts.md](./references/platform-facts.md) and, when you can browse, on X's own pages.

## Refuse these moves

| Tempting move                                                                 | Why it fails                                                                                                                                                                                                                                                                                                                 |
| ----------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Assume developers are still on X and skip the go-or-skip check                | No developer survey measures X share; Stack Overflow's community-platform list omits it [survey]; scientists measurably moved to Bluesky [research]. Decide from the user's own audience sample                                                                                                                              |
| Ask staff or a group chat to like and repost the launch in the first minutes  | Asking people to inflate metrics is engagement spam [X], and readers see the pile-on [practitioner]. X's code says a post reached from a shared link gains nothing in the main ranker, though the small-account boost reads raw likes [X]. Telling staff it is live so they can quote-post in their own words is allowed [X] |
| Post the same launch copy from the brand and every staff account              | Identical or substantially similar content across accounts is banned, translations excepted [X]                                                                                                                                                                                                                              |
| Set a tool to auto-reply, auto-like or auto-follow on a keyword               | Keyword-triggered replies, automated likes and bulk follows are banned; automated replies go only to people who engaged with you, once per interaction [X]                                                                                                                                                                   |
| Plan an X Community for your project                                          | X closed Communities [X via press]; archived help pages still describe them. Keep the community on Discord or GitHub Discussions                                                                                                                                                                                             |
| State "links get zero reach" or "links never hurt" as fact                    | X staff say there is no link rule and the public code shows none [X]; Buffer measured unverified accounts' link posts at a median of zero engagement, another dataset found no gap [practitioner]. Test placement on the account                                                                                             |
| Buy the business gold check before anyone on the team gets replies            | Reply prioritization comes with each person's paid plan, scaled by tier [X], and the measured reach gap is per person too [practitioner]. Paid plans for the people who reply come first                                                                                                                                     |
| Run a paid creator post, or trade free licences for posts, without disclosure | X's Paid Partnership label covers sponsored posts [X via press], and advertising rules such as the FTC's or the ASA's ask for disclosure words in the copy anyway                                                                                                                                                            |
| Mirror X posts to Bluesky or Mastodon unattended                              | No bridge carries X posts; mirrored x.com links, X handles and dropped alt text read as "nobody home" [practitioner]                                                                                                                                                                                                         |
| Quote a ranking weight ratio ("a reply is worth 27 likes")                    | Weights scale each viewer's predicted probability of an action, and rare actions get large weights only so they register at all [X]; ratios say nothing about value                                                                                                                                                          |
| Publish AI-written posts the named person did not rewrite                     | Drafts you produce are starting points; the person rewrites them in their own voice                                                                                                                                                                                                                                          |

## Match the answer to the request

- Narrow question (do links hurt, is the checkmark worth it, why did our reply vanish, can our bot reply): answer it, plus the refusals and limits it touches.
- Presence setup, launch, "no reach" or "should we stay on X": run the Interview, then deliver the full Output.
- A thread about your product (outage, complaint, security report, viral criticism): skip the Interview and run § A thread about you first.
- A direct question inside a setup request: answer it in the first lines, then the full Output.
- Bot or API code, account recovery, appeals and ads campaigns are outside this skill; this skill covers how an account behaves and what it may automate.
- Hiring posts serve the plan in samber/developer-relations-skills@tech-employer-branding; post them from a person's account.

## Interview

One question per message; skip what the request answers. When the user wants one pass, state an assumption per gap.

1. Goal: launch, ongoing presence, a thread already about you, a decision to stay or leave, or moving followers to Bluesky or Mastodon?
2. Who posts, under their own name, and how many hours a week? Which accounts exist, with what followers, and which hold a checkmark?
3. Where does the audience talk today? Name the language, framework or tool communities; has anyone sampled the social links of recent stargazers, contributors or customers?
4. Is any post sponsored, gifted or paid, and does the launch make benchmark or "fastest" claims?
5. Date the result must land by, one-off moment or compounding asset, and the effort ceiling (hours, budget for paid plans, sign-off needed).

**Memory: Store a few info about the current task or project context. When memory lives in a file, use `devrel-context.md`; if a different memory system is in use, rely on that instead. Separate task info in different sections. Remove finished tasks. Add a date to a task; no date for general project context. Some interview responses may differ between 2 tasks.**

## Go or skip

Run this before any setup, and return one verdict with the evidence behind it:

1. Sample the public social links of about 100 recent stargazers, contributors, issue authors or customers.
2. **Skip** when few of them list X and the rest cluster on Bluesky, Mastodon or LinkedIn, or when nobody can reply as a named person for 20-30 minutes on most weekdays. Keep the handle claimed, point the bio and pinned post to the active channel, and limit who can reply so the account collects no unanswered questions.
3. **Go small** (one person on a Premium-level plan, replies only, no brand push) when the sample is mixed or the audience is academic, where migration to Bluesky is measured [research]. Practitioners report the same drift for web-platform and European open-source circles, unmeasured [practitioner].
4. **Go** when X dominates the sample; practitioners report this most for AI tooling, startup and indie developer audiences [practitioner].
5. Review after 90 days on follows, qualified visits and stars per hour spent, against the next-best channel.

A skip verdict is a valid output, not a failure. Never decide from platform-wide user counts: X's claimed and third-party figures conflict [press], and no study measures software developers specifically.

## Accounts and identity

- **People first.** Founders, maintainers and advocates post under their own names; the brand account is a hub for releases, incidents, support routes and reposts with added context [practitioner]. The ranker predicts whether each viewer will reply, share or mute [X]: a person in a conversation earns replies, a logo rarely does.
- **When the brand holds the audience** (the founder has few followers): the brand posts the launch, the person replies under it and quote-posts with their own angle, and people-first builds over the following months.
- **Paid plans, buy order:** first a Premium-level plan for each person who replies daily, then nothing more until replies arrive. Reply prioritization starts at the cheapest paid plan and grows by tier [X]; the 50-post and 200-reply daily cap applies to accounts without a checkmark [X]; Buffer's 18.8M-post dataset shows accounts without a plan under 100 median impressions against about 600 for Premium [practitioner, correlational]. Plan names and prices change: check platform-facts.
- **Business gold check:** the entry tier only against real impersonation or ad spend its credit offsets; the full tier only when several employees need affiliate badges, which show the company logo beside their name [X].
- **Verification and the feed:** the published ranking code has no verified multiplier, and X's own wording claims only reply prioritization in conversations [X]; measured gaps exist but are correlational [practitioner].
- **Several accounts:** up to 10 for distinct purposes (per language, region, support) are allowed; never use one to boost another [X].
- **Support route:** pin where support goes (status page, GitHub issues, docs) and decide whether the brand's DMs stay open; an unanswered DM inbox reads worse than a closed one [practitioner].
- **Bots and auto-posters:** label them automated, name the operator in the bio, keep them to release or status posts [practitioner].

## Engagement surfaces, ranked

- value: founder launch post > replies by named people > Articles or long posts > brand hub posts > Spaces or live AMAs > threads chasing reach
- effort: Articles > Spaces or live AMAs > replies (standing daily time) > founder launch post > threads > brand hub posts
- efficiency (value per hour): replies by named people > founder launch post with native demo > brand hub release posts > Articles or long posts > Spaces or live AMAs > threads chasing reach

| Surface                                                           | Payoff      | What it buys                                                                                                                                                                                                      |
| ----------------------------------------------------------------- | ----------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Replies under posts by the people your users follow               | compounding | Profile visits and follows from the right graph; replies reach strangers through the conversation view, where paid plans rank higher by tier [X], rarely through For You, which filters and discounts replies [X] |
| Founder or maintainer launch post with a native demo              | one-shot    | The day's reach; posts that make replies, quotes and DM or copy-link shares likely rank [X]; mutual followers see the person's original posts boosted [X]                                                         |
| Brand hub: releases, incidents, reposts with context              | compounding | A place to land and a changelog people follow                                                                                                                                                                     |
| Articles (Premium, Premium+ or an X Subscription plan; not Basic) | compounding | A deep dive readers finish; the ranker predicts read-through and time spent [X]                                                                                                                                   |
| Spaces, live video                                                | one-shot    | Release AMAs for people already following; little discovery [practitioner]                                                                                                                                        |
| Threads tuned for reach                                           | one-shot    | Each post is scored separately, so a weak opener caps the thread [X]                                                                                                                                              |

- **Starved option:** a technical Article series is high on value and highest on effort, so efficiency never picks it. Promote it when a launch needs a canonical explanation people will link to, or when the founder already writes long-form.
- **Live shows:** a recurring stream on X is its own format; run it with samber/developer-relations-skills@devrel-live-streaming.
- **Default rung:** a Premium-level plan for one named person, then two weeks of daily substantive replies before any product post.
- Re-rank against what the user owns: a founder with an engaged following starts at the launch post; a hard date promotes the launch post and the Article; no person willing to reply drops the plan to the hub only.

## What ranking rewards and punishes

The ranker predicts each viewer's chance of each action and sums weighted predictions; weights drift monthly [X]. Quote the shape, never a number as current. Exact values: [references/platform-facts.md](./references/platform-facts.md) § Ranking.

**Do more:**

- Posts that make the right viewers likely to reply, quote or share privately (DM, copy link); answer the replies you get [X].
- Dense posts and Articles people read to the end; the ranker predicts dwell and read-through [X].
- Native demo video that holds attention: video earns through watch and dwell, not as a format bonus [X]; keep demos short with captions [practitioner].
- Posts while you can stay for two hours: an original post from an account under about 50,000 followers can win one boosted feed slot while it is under about two hours old and under a couple of hundred Home views [X].
- A full caption and media on any link post; X staff say a link with almost no description gets weak distribution [X].

**Do less:**

- Anything that makes the wrong audience likely to click "not interested", mute, block or report: rare actions, weighted heavily so they register, pull a post down for every viewer similar to them [X].
- Several posts in a row from the same account: each extra post in one feed is discounted [X].
- Messy link chains: a post whose redirect chain hits a low-quality domain, mentioning non-followers, gets a spam label [X]; skip shorteners and extra hops to keep the chain clean [practitioner].
- Product links dropped in strangers' threads, hashtag stuffing, posts that are mostly a link: content spam [X].

**Links:** X staff state there is no link rule and link-in-reply is unnecessary [X]; practitioner datasets disagree, worst for accounts without a checkmark [practitioner]. Alternate link-in-post and link-in-first-reply across at least 10 comparable posts each, compare link clicks per impression, and keep the link in the post when results overlap.

## A thread about you

Practitioner norms throughout [practitioner].

1. Within the hour, a named person (maintainer, founder, on-call advocate) replies once, factually: what is known, what is being done, where updates live (status page, issue).
2. Never delete critical posts, argue, or threaten legal action; never send staff to pile into the replies [practitioner].
3. Move detail to the status page, the issue or DMs the person asked for; keep the public reply short.
4. Post a follow-up from the same person when resolved, quoting the original thread.
5. Security reports go to the disclosure channel; reply only with where to send them.

Template: [references/templates.md](./references/templates.md) § A thread about you.

## Cadence

1. **Week 0:** run Go or skip; bios naming the person and employer, pinned post, a Premium-level plan for the person who replies; the disclosure line staff use when they mention the product.
2. **Weeks 1-2:** 20-30 minutes a day of substantive replies under posts by the maintainers, language leads and framework authors your users follow; no product links unless asked.
3. **Weeks 3-4:** two to four original posts a week per person (a benchmark with method, a bug story, a design decision); the brand hub posts releases only.
4. **Launch week:** founder teaser (screenshot or short demo) two to five days before; on the day, one native post with a short demo and one claim with a linked method, since benchmark and "fastest" claims attract Community Notes [practitioner], posted in the audience's working hours; the person answers every substantive reply for the first two hours; staff quote-post with their own angle; an Article or long post with the deep dive the next day.
5. **Launch in under two weeks:** compress weeks 1-4 into daily replies until launch day, and let the account with the audience post the launch.
6. **Monthly:** review profile visits, follows, link clicks, UTM visits and stars per hour spent; drop what returned nothing; re-check caps and plans in platform-facts.

## Limits

Per-surface table with each limit's kind and source: [references/platform-facts.md](./references/platform-facts.md) § Limits. The ones that bind most often:

- Accounts without a checkmark: 50 original posts and 200 replies a day, split into half-hour windows; every account: 400 follows and 500 DMs a day [X]. Enforced.
- Automation: scheduling original posts is allowed; keyword replies, automated likes, bulk follows and automated DMs to people who did not ask are banned [X]. Enforced.
- Duplicate posts across your accounts, engagement pods, follow-unfollow churn: banned [X]. Enforced.
- Sponsored posts and posts traded for free licences: Paid Partnership label plus disclosure words in the copy [X via press]. Enforced.
- Replies under someone else's post: the author can limit who replies and hide replies [X]. Policed by each author; link drops there draw mutes [practitioner].
- Community Notes: contributors add context to misleading claims; a note on a launch post stays visible [practitioner]. Policed.
- X has no sub-communities with their own rules since Communities closed; per-space rules are each author's reply settings and the norms of the circle you reply into.

## Cross-posting to Bluesky, Mastodon and Threads

- **Pick a primary network per audience** from the Go or skip sample; dual presence is the measured norm after the migrations [research].
- **No bridge from X:** write one source draft, then rewrite per network: Bluesky 300 characters, Mastodon 500 (each link counts as 23), Threads 500; replace X handles; keep alt text; no x.com links or tweet screenshots [practitioner].
- **Bluesky and Mastodon together:** Bridgy Fed bridges an account that opts in, and replies flow both ways [platform docs]; it never touches X.
- **Threads to the fediverse:** opt-in sharing lets Mastodon users follow and reply to a Threads profile [platform docs].
- **Never mirror replies or quote posts**; a person reads replies on every network they post to, or stops posting there.
- **Schedulers** posting to X count against X's caps and pay X per post, more for link posts [X].

Bluesky-specific mechanics live in samber/developer-relations-skills@bluesky-dev-playbook.

## Output

For a full plan, deliver in order: the direct answer when the user asked one; go/skip verdict with the sample evidence or the assumption standing in for it; refusals triggered; account and paid-plan plan; surfaces ranked for this user with what moved the order; dated cadence; limits that bind, each with kind and source; cross-posting plan; the matching templates from [references/templates.md](./references/templates.md) adapted to the user; metrics. Mark every drafted post as a draft for the named person to rewrite, and run it through your preferred humanizer skill.

## Metrics

- Profile visits, follows and link clicks per post and per person; impressions alone mislead.
- Replies the person answered, per week.
- UTM-tagged visits, signups and stars within 48 hours of each post, against a pre-launch baseline; link prefetching in X's in-app browser can inflate raw referral counts [press].
- Mutes, blocks or a drop in reach after a post are stop signals for that format.
- The full analytics dashboard needs Premium or above [X]; other accounts see per-post metrics in the app.
- Never report raw follower totals as the headline.

## References

- samber/developer-relations-skills@hackernews-playbook - the other launch-day channel, with its own rules.
- samber/developer-relations-skills@devto-playbook - where the long-form post behind an X link lives.
- samber/developer-relations-skills@discord-community-playbook - the community home X no longer provides.
- samber/developer-relations-skills@build-in-public - the weekly narrative these posts carry.
- samber/developer-relations-skills@devrel-content-calendar - fitting X slots into the quarter.
- samber/developer-relations-skills@oss-launch - the cross-channel launch plan this post belongs to.
- samber/developer-relations-skills@devrel-analytics - attributing X referrals to signups.
- samber/developer-platform-skills@github-engagement-playbook - see also, the GitHub surfaces an X post points to.
