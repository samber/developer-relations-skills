---
name: threads-dev-playbook
description: "Runs a devtool vendor's, developer advocate's or open-source maintainer's presence on Threads (Meta) - go-or-skip verdict for developer audiences, the Instagram link, brand versus people accounts, topic tags and Communities, what the For You feed rewards, fediverse sharing and its real reach into Mastodon, cross-posting with X, Bluesky and Mastodon. Use whenever a devtool company, developer advocate or OSS project asks whether to be on Threads, why their Threads posts get no reach, which topic tag to use, whether to turn on fediverse sharing, or how to mirror X posts to Threads - even if they only say 'should we be on Threads'. Not for Threads API code, Instagram marketing or investigating Threads accounts. Do NOT use for Bluesky itself - use samber/developer-relations-skills@bluesky-dev-playbook."
license: MIT
metadata:
  author: Samuel Berthe
  version: "1.0.0"
---

# Threads Developer Playbook

You run a vendor's, advocate's or maintainer's presence on Threads: a very large general network, ranked by one personalized For You feed, where developer voices are few and most devtool brand accounts sit parked. Every rule below follows from that gap between audience size and developer density.

Label every Threads claim: **[Meta]** when Meta's help center, developer docs, Transparency Center, creator site, newsroom or an official Meta executive post states it, **[research]** for peer-reviewed studies, **[press]** for reported facts, **[practitioner]** for tool vendors, marketers, third-party trackers and your own reading of a Meta rule. Never present a practitioner claim as a Meta rule. Meta ships Threads features monthly: before quoting a number, re-check it in [references/platform-facts.md](./references/platform-facts.md) and, when you can browse, on Meta's own pages.

## Refuse these moves

| Tempting move                                                                    | Why it fails                                                                                                                                                                                                                                                                                         |
| -------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Treat the monthly-user count as proof developers are there                       | Few well-known engineers post regularly, several devtool brands hold parked accounts, and no developer survey lists Threads [practitioner]; the verdict comes from named people in the user's stack                                                                                                  |
| Port the X playbook: 1/n hook chains, several hashtags, best posting hour        | One topic tag per post [Meta]; long text goes in a text attachment up to 10,000 characters [Meta]; replies account for almost half of the views [Meta]; "first hour decides reach" is unconfirmed [practitioner]                                                                                     |
| Hide the link in the first reply "because Threads penalizes links"               | Meta's Instagram head said links are not deliberately downranked and worked better from mid-2025 [Meta]; link posts draw fewer likes and replies, which is the real cost [Meta]; test both with click Insights                                                                                       |
| Ask staff to like and repost the launch post in its first minutes, or join a pod | Meta's rules ban fake engagement and inauthentic behaviour across its apps, Threads included [Meta]; readers see the pile-on [practitioner]                                                                                                                                                          |
| Ask followers to post "Dear Algo, show me more <our brand>"                      | Dear Algo is a reader's feed control [Meta]; a coordinated brand request reads as manufacturing a ranking signal [practitioner]. A topic request ("more Rust") from the person's own feed is fine                                                                                                    |
| Promise that fediverse sharing puts the brand in front of Mastodon developers    | Only Instagram-created, public, 18+ profiles outside the European Region can share [Meta]; about four in five Mastodon servers that publish a Threads policy suspend or limit threads.net, a sample skewed toward blockers [practitioner]; you can like fediverse replies but not answer them [Meta] |
| Run an unattended mirror of the X account through a scheduler                    | Mentions and links break, some schedulers drop alt text on Threads, and nobody reads the replies [practitioner]; Meta favours original posts [press]                                                                                                                                                 |
| Report fediverse followers inside the Threads follower total                     | Fediverse followers sit in a separate list and are excluded from the Threads count [Meta]                                                                                                                                                                                                            |
| Post sponsored or affiliate content without saying so                            | Meta's branded content policies cover Threads [Meta]; disclose in the post text even where no label appears in the composer [practitioner]                                                                                                                                                           |
| Promise follower growth from the brand's Instagram following                     | Instagram follows do not carry over and the Instagram graph left Threads onboarding suggestions in late 2024 [press]; every Threads follower starts from zero                                                                                                                                        |

## Match the answer to the request

- Narrow question (which tag, should we federate, why no reach on link posts): answer it, plus the refusals and limits it touches.
- Presence setup, launch, or "no reach": run the Interview, then deliver the full Output.
- A direct question inside a setup request ("is Threads worth it?"): answer it in the first lines, then the full Output.
- Building a publishing integration, webhook or analytics pipeline on the Threads API is integration engineering: say it is outside this skill and point at Meta's Threads API docs; this skill covers only how the accounts behave.
- Paid: Threads ads run through Meta's ad tools [Meta]; this skill covers organic presence. Before buying reach, check that the go/skip verdict is not skip: ads cannot fix an audience with no developers in it [practitioner].
- Hiring posts: the employer-brand plan belongs to samber/developer-relations-skills@tech-employer-branding; post them from a person's account like any other post.

## Interview

One question per message; skip what the request answers. When the user wants one pass, state an assumption per gap.

**Memory: Store a few info about the current task or project context. When memory lives in a file, use `devrel-context.md`; if a different memory system is in use, rely on that instead. Separate task info in different sections. Remove finished tasks. Add a date to a task; no date for general project context. Some interview responses may differ between 2 tasks.**

1. Goal: launch, ongoing presence, a conversation already about you, or reaching Mastodon or Bluesky users through Threads?
2. Who will post, under their own name, and how many hours a week? A brand account with no named person is the weakest setup.
3. Name three to five people or projects in your stack: do they post on Threads now, and in which topic tags or Communities?
4. Where are the company and the posting people based (inside the European Region or not), and does each one have an Instagram account?
5. Does the brand already run an Instagram account, and who follows it: developers or a general audience?
6. Date the result must land by, one-off moment or compounding asset, and the effort ceiling.

## Go or skip

Run this before any setup, and return one verdict:

- **Skip**, and say so, when either holds:
  - none of the named people or projects in the stack posted on Threads in the last month, and no topic tag or Community for the stack shows recent posts;
  - nobody can post as a person for at least an hour a week.
- **Go small** (one named person, replies first, brand account parked) when the stack is present but thin, or when the real goal is Mastodon or Bluesky readers: send that goal to its own playbook first.
- **Go** otherwise.
- **Check unknown** (no answer to Interview question 3, and no way to search): return Go small with a dated week-0 check, never Go. The check: in the app, search the stack's topic tag and each named person, and count posts from the last 30 days.

Every verdict, skip included, ends with: claim the brand and project handles with a bio pointing to the channels you do run. Squatted developer handles exist on Threads [practitioner], and claiming costs minutes. A skip verdict is a valid output, not a failure.

## Accounts and identity

- **People first.** Advocates, maintainers and engineers post under their own names; the brand account is a hub that reposts their teaching posts, announces releases and answers support [practitioner].
- **Location first, then Instagram.** A person or company inside the European Region cannot share to the fediverse, whatever the account setup [Meta]; Meta defines the European Region in its terms, and the Fediverse sharing toggle in Settings is the final check.
- **Instagram link** outside the European Region: profiles created from an Instagram account can turn on fediverse sharing; profiles created with email or phone cannot [Meta]. Create the brand profile from the brand's Instagram when one exists (recognition, no audience), and each person's from their own Instagram when they want to federate.
- **No brand Instagram:** sign up with email; never create an empty Instagram account only to link it, since it brings no audience and federation may be unavailable anyway.
- **Followers start at zero** on every account; plan the first month around earning them, not importing them [press].
- **Usernames may differ** between Instagram and Threads; the in-profile link, not a matching handle, proves the tie [Meta]. Never take handles of competitors or projects you do not run: impersonation is banned [Meta].
- **Squatted handle:** an impersonation report applies only when the account poses as you, and a trademark report needs a registered mark [Meta]. An empty account holding the name is usually not reclaimable: take a clear variant (`<name>dev`, `<name>hq`) and link it from the website and README so readers can tell the real account apart.
- **Verification:** do not promise a badge. What Meta Verified for business adds on Threads is unconfirmed; re-check Meta's page before quoting it.
- **Disclosure line** in every staff bio ("I work at X; posts are mine") and in replies about your own product.

## Engagement surfaces, ranked by value per hour

Efficiency: replies in your stack's existing conversations > original posts with one accurate topic tag > posting in an existing Community for your stack > text attachments for long technical posts > brand hub release posts > fediverse sharing on a person's account > trending topics.

| Surface                                     | Effort                                                            | Payoff            | What it buys                                                                                                         |
| ------------------------------------------- | ----------------------------------------------------------------- | ----------------- | -------------------------------------------------------------------------------------------------------------------- |
| Replies in other people's conversations     | minutes a day, every day                                          | compounding       | Replies are almost half of Threads views [Meta]; follows from people already reading your stack                      |
| Original posts with one topic tag           | minutes per post                                                  | compounding       | Tagged posts generally get more views [Meta]; the tag reaches people who tap it or search it [Meta]                  |
| An existing Community for the stack         | a week of reading, then minutes per post                          | compounding       | A public feed built from a tag [Meta]; Champion badges go to regular posters who keep replies going [Meta]           |
| Text attachment (up to 10,000 characters)   | an hour per write-up                                              | one-shot per post | A full write-up without a 1/n chain; text-only posts, one link [Meta]                                                |
| Brand hub posts, pinned post, profile links | an hour a week                                                    | compounding       | A place to land; releases, docs, status                                                                              |
| Fediverse sharing on a person's account     | minutes; copies on other servers may persist after turning it off | compounding       | Followers on Mastodon servers that federate with threads.net [Meta]; many FOSS-leaning servers do not [practitioner] |
| Trending topics                             | near-zero                                                         | rare              | Mostly current events and pop culture [Meta]                                                                         |

Dear Algo is not a reach surface: it changes only the poster's feed, or the feed of whoever reposts the request [Meta]. Use it, where available, to tune the person's own feed toward the stack conversations they reply in.

- **Starved option:** growing a stack tag into a Community. Meta shows a progress tracker with steps for topics near the threshold, and decides which topics qualify [Meta]; the work is a standing job of regular posts and replies. Efficiency never picks it. Promote it only when the tracker already shows on the stack's tag and a named person commits to it for a quarter [practitioner].
- **Default rung:** two weeks of daily replies by a named person, then at least two to five original posts a week with one topic tag.
- Re-rank against what the user owns: an EU-based person drops the fediverse rung; an existing developer-heavy Instagram following promotes the brand hub; a hard launch date promotes tagged original posts over Community building.

## What ranking rewards and punishes

**Do more:**

- Posts that start a conversation: Meta says they are more likely to be recommended [Meta].
- Replies in other people's threads, as often as original posts [Meta].
- At least two to five posts a week per person: Meta links higher frequency to more views per post, so more is fine while each post stands alone [Meta].
- One accurate topic tag per post; text on media posts, which did better than media alone [Meta].
- Links in the post, with a conversation hook in the text [Meta: no deliberate penalty]. Run a four-week link-in-post versus link-in-reply test on routine posts before any launch, and use the winner on launch day; compare medians, not totals, and keep the link in the post unless link-in-reply wins clearly. With no test data, keep the link in the post and make the text worth replying to.

**Do less:**

- Link-only posts with no question or claim: few likes and replies [Meta].
- Copies of X posts: Meta favours original content [press]; reach loss is unmeasured.
- Bait and pile-ons: banned as fake engagement [Meta]; folklore weights ("replies count 3x likes", "first 30 minutes") are unconfirmed [practitioner].
- Off-topic or misleading tags: the tag reaches people who tap or search it [Meta], and a mismatch earns hides from them [practitioner].

**Posting times:** Meta publishes no best hour for Threads, and practitioner tables do not transfer between audiences. When asked, give a test instead of a table: two time slots matched to the audience's working hours, alternated for four weeks, compared on views and replies per post.

## Cadence

1. **Week 0:** profiles from the right Instagram accounts, bios with names and disclosure, handles claimed, pinned post. Each person lists the stack's topic tags and Communities to read daily. Where the audience already follows the person elsewhere, announce the new handle once there.
2. **Weeks 1-2:** each person replies daily in those tags and in conversations by the people named in the Interview; no product links unless asked.
3. **Week 3:** original posts start, at least two to five a week per person, one topic tag each; long write-ups as text attachments.
4. **Week 4 onwards:** the brand hub reposts staff teaching posts and posts releases. Turn on fediverse sharing for eligible people only after deciding the one-way door below.
5. **Launch day:** the person who built it posts with the stack tag, the hub reposts, staff answer questions with the disclosure line and never mass-like. Use reply approvals if replies turn hostile [Meta].
6. **Monthly:** read Insights per person (views, replies, link clicks), drop tags and surfaces that returned nothing, re-check the stack's Communities.

Launch under four weeks away: run the reply weeks and the first original posts in parallel from week 1, and never drop the replies, since they carry almost half the views [Meta].

## Limits

Per-surface table with kind and source: [references/platform-facts.md](./references/platform-facts.md) § Limits. The ones that bind most often:

- 500 characters per post; one topic tag; up to 5 links per post [Meta].
- Text attachment: up to 10,000 characters, text-only posts; one link, either the post's link card or the attachment's, not both; the app cannot schedule it, the API can publish it [Meta].
- Fediverse sharing: Instagram-created, public, 18+, outside the European Region [Meta]; posts with limited replies or unsupported attachments do not federate [Meta].
- Communities: Meta publishes no per-Community self-promotion rule; Champions set flair and the regulars set the norm [Meta for Champions, practitioner for norms]. Read a week of the Community before posting, and keep your share of product links no higher than what its regulars post [practitioner].
- API publishing: 250 posts and 1,000 replies per profile per 24-hour moving window [Meta]; a quota is not permission, and bulk replies are spam far below it.

## Cross-posting with X, Bluesky and Mastodon

- **Mastodon:** fediverse sharing is a one-way door: turning it off or deleting asks other servers to delete, with no guarantee [Meta]. Use it on eligible personal accounts as a bonus. Check the audience's own servers: the About page's moderated-servers list, or `/api/v1/instance/domain_blocks` where it is public, matching threads.net, threads.com and threads.instagram.com. An empty or failed answer means hidden, not unblocked: ask the admin or test-follow the handle from that server. Reach FOSS-leaning Mastodon readers through a native Mastodon account run by a person (samber/developer-relations-skills@mastodon-dev-playbook).
- **Bluesky:** a federated Threads profile opts in to Bridgy Fed by following `@bsky.brid.gy@bsky.brid.gy`; it needs a display name different from the handle and a profile picture, bridges new posts only, truncates past 300 characters, and Bluesky replies come back only from Bluesky users who bridged themselves [practitioner: Bridgy Fed docs and developer]. For a real presence, post natively (samber/developer-relations-skills@bluesky-dev-playbook).
- **X:** rewrite, never mirror: one post instead of a 1/n chain, one topic tag instead of hashtags, no x.com links, handles checked per network. Never cross-post replies. Pick a scheduler that keeps alt text on Threads; check its docs before trusting it [practitioner]. A person reads Threads replies daily, or the cross-posting stops.

## Output

For a full plan, deliver in order: the direct answer when the user asked one; go/skip verdict with the reason; refusals triggered; account plan with the Instagram link per account; surfaces ranked for this user with what moved the order; dated cadence; limits that bind, each with kind and source; the fediverse decision with its one-way-door warning; the matching templates from [references/templates.md](./references/templates.md) adapted to the user; metrics. Mark every drafted post as a draft for the named person to rewrite, and run it through your preferred humanizer skill.

## Metrics

Threads Insights shows views, interactions, link clicks and, from 100 followers, follower demographics [Meta]; measure those, not likes alone.

- Replies and link clicks per post and per person, against a pre-launch baseline.
- Views on tagged versus untagged posts, and link-in-post versus link-in-reply over four weeks.
- Threads followers and fediverse followers as two separate numbers [Meta].
- Referral sessions and signups from UTM-tagged links, since Threads web traffic is small and app referrers get stripped [practitioner].
- Never report raw follower totals as the headline.

## References

- samber/developer-relations-skills@bluesky-dev-playbook - the opt-in-feed network, when Bluesky is the target.
- samber/developer-relations-skills@mastodon-dev-playbook - reaching Mastodon readers natively, when the audience's servers block or limit threads.net.
- samber/developer-relations-skills@devrel-content-calendar - fitting Threads slots into the quarter.
- samber/developer-relations-skills@build-in-public - the weekly narrative a person's posts carry.
- samber/developer-relations-skills@hackernews-playbook - the launch-day channel with its own rules.
- samber/developer-relations-skills@devto-playbook - where the long-form post behind a Threads link lives.
- samber/developer-relations-skills@developer-community-moderation - when a pile-on starts in your replies.
- samber/developer-relations-skills@devrel-analytics - attributing Threads referrals to signups.
