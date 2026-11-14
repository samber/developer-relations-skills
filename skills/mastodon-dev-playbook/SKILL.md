---
name: mastodon-dev-playbook
description: "Runs a devtool vendor's, developer advocate's or open-source maintainer's presence on Mastodon and the fediverse - go-or-skip verdict, which server to join (fosstodon, hachyderm, infosec.exchange or a self-hosted one), people versus brand accounts, rel=me verification, hashtag discovery with no central algorithm, boosts, content warnings, alt text, quote posts, release bots, cross-posting from X. Use whenever someone asks how to grow a developer audience on Mastodon, set up a company or OSS project account on the fediverse, run a changelog bot, or why their toots get no reach - even if they only say 'should we be on fosstodon'. Not for operating a server, coding API clients, or moderating your own instance. Do NOT use for Bluesky - use samber/developer-relations-skills@bluesky-dev-playbook."
license: MIT
metadata:
  author: Samuel Berthe
  version: "1.0.0"
---

# Mastodon Developer Playbook

You run a vendor's, advocate's or maintainer's presence on Mastodon: thousands of independent servers with their own rules, no ranking algorithm, and a culture that treats advertising as spam. Reach travels through people, followed hashtags and boosts that carry a post from one server to the next. Every rule below follows from that.

Label every claim: **[Mastodon]** when Mastodon's docs, blog or software state it, **[server]** when one server's own rules or docs state it (name the server), **[press]** for reported incidents, **[practitioner]** for guides, tool docs, measurements and your own reading of a rule. Never present a practitioner claim as a rule. Server rules, prices and sizes drift: before quoting a number, re-check it in [references/platform-facts.md](./references/platform-facts.md) and, when you can browse, on the server's `/about` page or its public `/api/v1/instance/rules`.

## Refuse these moves

| Tempting move                                                                                 | Why it fails                                                                                                                                                                                                                                                                                                                                                    |
| --------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Engagement-bait hooks, or the same promo repeated in a daily slot                             | No ranking exists to game, but Home is chronological [Mastodon], so post when your stack's tag followers are awake. Hachyderm's "Say It Once" bars specialized accounts from repeating a message; scheduled repeats are tolerated only within 5 posts a day and until the community calls them spam [server]; elsewhere repetition reads as spam [practitioner] |
| Open the brand account on Fosstodon, or any company account on Hachyderm without asking first | Fosstodon is invite-only and bans commercial promotion [server: fosstodon.org]; Hachyderm removes unapproved entity accounts and makes corporate accounts invite-only and paid [server: hachyderm.io]                                                                                                                                                           |
| Run the company's OSS project account as a free "project" account on Hachyderm                | A project with one company sponsor or a free/paid tier counts as corporate there; named examples are Go and Terraform [server: hachyderm.io]                                                                                                                                                                                                                    |
| Mirror the X account unattended, or use Moa                                                   | Fosstodon bans automated posting without monitoring; social.tchncs.de bans exclusive cross-posting [server]; Moa is archived [practitioner]                                                                                                                                                                                                                     |
| Stuff 8-10 hashtags, or post link-only promotions                                             | Fosstodon and Hachyderm treat excessive tags and link-only promotion as spam [server]                                                                                                                                                                                                                                                                           |
| Quote-dunk a critic or a competitor                                                           | Quotes follow the author's per-post policy (default Anyone), notify the author, and can be revoked irreversibly [Mastodon]; a dunk reads as dog-piling, which Fosstodon, mastodon.social and techhub.social ban [server]                                                                                                                                        |
| DM people about the product, or reply to keyword hits from a bot                              | Unsolicited product messages are banned on techhub.social [server]; Hachyderm bots may reply only when tagged [server]                                                                                                                                                                                                                                          |
| Scrape timelines for "social listening" or train a model on posts                             | infosec.exchange terms ban scrapers and data-mining tools [server]; Hachyderm bots may handle even public data only with opt-in consent [server]; Fosstodon requires consent for model training [server]                                                                                                                                                        |
| Post images without alt text, or hide hashtags in the content warning                         | floss.social sanctions public media without a description [server]; tags in a CW are not indexed [practitioner]                                                                                                                                                                                                                                                 |
| Promise follower growth or "virality"                                                         | Large dev tags see a few hundred to about 1,500 posting accounts a week on a big server [practitioner]; reach is small and slow by design                                                                                                                                                                                                                       |

## Match the answer to the request

- Narrow question (which server, can our bot post hourly, why is the check not green): answer it, plus the refusals and limits it touches.
- Presence setup, launch, or "no reach": run the Interview, then deliver the full Output.
- A direct question inside a setup request ("fosstodon or hachyderm?"): answer it in the first lines, then the full Output.
- Running your own server's ops (scaling, Sidekiq, moderation tooling) is hosting engineering: say it is outside this skill; this skill covers only whether to run one and how its accounts behave.
- Hiring posts: the employer-brand plan belongs to samber/developer-relations-skills@tech-employer-branding; post them from a person's account.

## Interview

One question per message; skip what the request answers. When the user wants one pass, state an assumption per gap.

1. Goal: launch, ongoing presence, a thread already about you, or moving followers from X or Bluesky?
2. Who will post, under their own name, and how many hours a week?
3. What is the product: commercial devtool, company-owned OSS with a paid tier, or community-governed OSS? This decides the server.
4. Is the product security-focused, AI-forward, or reliant on collecting user data?
5. Domain the company controls, and whether staff already have fediverse accounts (which servers).
6. Date the result must land by, one-off moment or compounding asset, and the effort ceiling.

## Go or skip

Return one verdict before any setup:

- **Skip** when either holds:
  - the stack's main hashtags, summed, show fewer than about 20 posting accounts a week on mastodon.social's `/api/v1/tags/<tag>` (a practitioner heuristic), and no known maintainer of the stack posts here;
  - nobody can post as a person for at least an hour a week.
- **Go small** (people accounts only, no brand account) when any holds:
  - the product is AI-forward or relies on collecting user data: AI-operated accounts are banned and scraping is refused on core dev servers, so a brand account draws suspicion before it posts [practitioner];
  - no brand-admitting server fits and nobody will own a server, or the company will not pay Hachyderm's corporate tier;
  - the effort ceiling is under about two hours a week: replies by one person beat a brand account nobody tends.
- **Go** otherwise.

A skip verdict is a valid output, not a failure.

## Choose the server

Pick per account, not per company. People accounts follow their community; the brand account follows the rules that admit it.

| Account                               | Default home                                                                                                     | Why                                                                                                                                                                                                                                                                                                                            |
| ------------------------------------- | ---------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| Advocate, maintainer, engineer        | The server their stack's community uses (Fosstodon by invite, Hachyderm, infosec.exchange, a language server)    | Local timeline and server culture match; personal accounts may post about their employer [server: hachyderm.io]                                                                                                                                                                                                                |
| Brand account, security product       | infosec.exchange                                                                                                 | Its rules allow "legitimate brand accounts" [server]                                                                                                                                                                                                                                                                           |
| Brand account, other devtools         | Hachyderm corporate tier, or mastodon.social                                                                     | Hachyderm: approved, priced, a visible trust badge; apply by email to the admins and post nothing as the brand until approved [server]; small startups can ask about the fee [server]. mastodon.social: open, no brand rule [server]. The home server buys rules and trust, not audience: followers on any server see the post |
| Several project accounts plus a brand | Own server on the company domain                                                                                 | Strongest trust signal [Mastodon]; a standing job, see the starved option below                                                                                                                                                                                                                                                |
| Community-governed OSS project        | Fosstodon (invite from a member), floss.social (waitlist), Hachyderm project type; open fallback mastodon.social | Free project accounts where the project is not a company product [server]                                                                                                                                                                                                                                                      |

- A personal account on Fosstodon may share the person's own work, but product posts on repeat count as "self-promotion for profit" there [server]: keep product links to answers and releases, and disclose the employer.
- Foundation-hosted projects (CNCF, Cloud Foundry and their projects) count as corporate on Hachyderm [server].
- Never put the brand on Fosstodon or mas.to: their rules call commercial promotion spam [server]. Ask the admins first on techhub.social: it bans direct advertising and aggressive promotion but allows responsible promotion with a description and hashtags [server]. Keep brand accounts off ruby.social: it rejects sign-ups that exist to promote a company and requires a stated Ruby connection [server].
- For people accounts, mastodon.social admits anyone but gives no topic local timeline; use it only when no topic server fits.
- Before signup, read the server's rules endpoint; infosec.exchange may ban without warning; Hachyderm warns specialized accounts once, then asks them to leave [server].

## Accounts and identity

- **People first.** Named staff talk; the brand account announces releases, security advisories and outages, and boosts staff teaching posts. Most of the largest OSS accounts sampled reply or boost more than they broadcast [practitioner].
- **Verify with rel=me**, not a blue check, which does not exist [Mastodon]. Put the HTTPS site link in a profile field; the page must carry `rel="me"` back to the profile in raw HTML, not inside an iframe, under 1 MB; save the profile after the page is live [Mastodon]. Site builders that wrap content blocks in iframes break it.
- **Author attribution:** add `fediverse:creator` with each author's own handle to engineering posts and allow the blog domain in that author's settings [Mastodon], so shares credit and grow people accounts.
- **Hachyderm approval:** after admins approve, add the `APPROVED` profile field they specify; added early it stays unverified until you re-save the profile after approval [server, Mastodon].
- **Disclosure:** every staff bio names the employer; affiliate links on Hachyderm carry `#AffiliateLinks` [server].
- **Bot accounts** set the Automated (bot) flag, name the human operator, and stay separate from the brand account.
- **Don't park handles** across servers to block squatters: Hachyderm suspends parked accounts [server], and with thousands of servers a reserved empty account buys nothing [practitioner]. Report impersonators to the hosting server's moderators; infosec.exchange and mas.to have explicit impersonation rules [server].
- **Moving servers** moves followers, not posts, with a 30-day cooldown [Mastodon]: choose carefully the first time.

## Engagement surfaces, ranked by value per hour

**Default rung:** each person posts and pins an #introduction in week 0, then spends two weeks replying under the stack's hashtags.

Efficiency: replies in your stack's hashtags > people's original posts with the right tags > the pinned #introduction > a collection of your people and community voices > boosts from community and project accounts > fediverse:creator on blog posts > brand release posts > featured hashtags and profile directory > own server > trends.

| Surface                                         | Payoff                                    | What it buys                                                                                                                                                                                     |
| ----------------------------------------------- | ----------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| Replies under your stack's hashtags             | compounding                               | Follows from people already talking; replies reach followers who follow both parties only [Mastodon], so the conversation itself is the audience                                                 |
| People's posts carrying 1-3 stack hashtags      | one-shot per post, compounding per person | The only route to non-followers: tag followers see public tagged posts in Home [Mastodon]                                                                                                        |
| Pinned #introduction per person                 | compounding                               | Discovery by the many who follow #introduction [practitioner]; up to 5 pinned posts [Mastodon]                                                                                                   |
| Collection of your people plus community voices | compounding                               | Shareable list of up to 25 accounts, Mastodon 4.6+; only accounts with discovery on can be added, members are notified and can remove themselves, and the curator must add themselves [Mastodon] |
| Boosts from community and project accounts      | one-shot                                  | Each boost on a new server delivers the post there [practitioner reading of Mastodon docs]                                                                                                       |
| fediverse:creator on engineering posts          | compounding                               | Author byline in every link preview [Mastodon]                                                                                                                                                   |
| Brand release and status posts                  | compounding as a record                   | A place to land; RSS of the account page [Mastodon]                                                                                                                                              |
| Featured hashtags, directory, search opt-in     | compounding, small                        | Opt-in discovery features [Mastodon]; turn them on for every official account                                                                                                                    |
| Own branded server                              | standing job                              | Trust from the domain; handles like `name@social.company.com`                                                                                                                                    |
| Trending tags and links                         | rare                                      | Per server, admin-approved, no published threshold [Mastodon]                                                                                                                                    |

- **Starved option:** your own server scores high on trust but costs a standing job (hosting, moderation, and 3 months' shutdown notice if you want listing on joinmastodon.org [Mastodon covenant]); BBC ran one as an experiment, EU Voice and mozilla.social closed [press]. Promote it when you run three or more project accounts and a named owner has budget for at least two years.
- Re-rank against what the user owns: a maintainer already followed in the stack skips to original posts; a hard launch date promotes brand release posts plus staff boosts on launch day.

## What discovery rewards and punishes

No ranking exists [Mastodon]; these are the levers that move a post across servers.

**Do more:**

- One to three relevant hashtags per post, CamelCase (`#RustLang`, `#DevOps`) for screen readers [server: hachyderm.io], in the body.
- Alt text on every image, including code and terminal screenshots [server: floss.social, fosstodon.org].
- The right post language: readers can filter public timelines by language [Mastodon].
- Posts that answer something: support-style accounts with mostly replies (Thunderbird, Codeberg) are among the largest in a sample of OSS accounts [practitioner]; a correlation, not proof of cause.
- A content warning as a subject line for long post-mortems, politics or rants [Mastodon, server].
- Cache the landing page: every receiving server fetches the link preview at once [Mastodon repo].

**Do less:**

- Repeating a human announcement: Hachyderm's "Say It Once" targets the "cron job" effect; automated posts are exempt only within its daily cap [server].
- Replies that only add your product link: they reach few people and read as spam.
- Public posting from bots: use quiet public, which stays out of live feeds, search and explore [Mastodon].
- AI-written posts nobody rewrote: mastodon.social requires AI disclosure and Fosstodon asks it on stand-alone AI works [server]; drafts you produce are starting points the person rewrites.

## Cadence

1. **Week 0:** pick servers per account, request Hachyderm approval if needed, rel=me on every official profile, bios with employer, search and directory opt-in, and "Feature profile and posts in discovery algorithms" on (required to be added to a collection) [Mastodon]; each person posts and pins an #introduction.
2. **Weeks 1-2:** each person follows the stack's hashtags and replies daily where they can help; no product links unless asked.
3. **Week 3:** build a collection of your people plus community voices you now talk with, each asked first.
4. **Week 4 onwards:** a few original posts a week per person with stack tags; the brand posts each release once, boosts staff, answers support.
5. **Launch day:** the builder posts with stack tags and a cached link; the brand boosts once; staff answer replies. Hostile replies go to samber/developer-relations-skills@developer-community-moderation.
6. **Monthly:** check each server's rules for changes, review boosts by server and UTM sessions, drop tags that returned nothing.

- **Launch under four weeks away:** compress weeks 1-3 into the time left, keep each #introduction at least a week before launch, and skip the brand account if Hachyderm approval has not arrived; launch from people accounts.
- **Under two hours a week:** one person, replies plus one original post a week; no brand account, no bot.

## Limits

Kind is **enforced** (software or moderators act) or **policed** (the community reacts without a written rule). Per-server table with kind and source: [references/platform-facts.md](./references/platform-facts.md) § Limits. The ones that bind most often:

- Post length is per server: 500 by default, 2263 on Hachyderm, 11,000 on infosec.exchange; links count 23 [Mastodon, server].
- Automation: on Hachyderm every account, staff personal accounts included, may make 5 automated or scheduled posts a day, with exceptions on request, and bots need moderator approval [server]; infosec.exchange caps automated posts at one an hour or 24 a day public, unlimited quiet public, and requires the bot label above 50% automation [server].
- API: 300 calls per 5 minutes per account [Mastodon]; a rate cap, never permission for bulk follows or replies.
- Hachyderm corporate tier is paid and invite-only; pricing in the reference [server].
- A server suspended by others loses those follows permanently, even if lifted [Mastodon].

## Cross-posting

- **Bluesky:** opt in with Bridgy Fed, not a copy bot. It requires an account at least a week old, a profile picture and a display name that differs from the handle, and it does not backfill older posts [practitioner: Bridgy Fed docs], so opt in at least a week before launch; details in samber/developer-relations-skills@bluesky-dev-playbook.
- **Threads:** opt-in fediverse sharing reaches only servers that do not block it, and many signed a block pledge [practitioner]; plan Threads on its own terms with samber/developer-relations-skills@threads-dev-playbook.
- **X:** post natively on Mastodon; if you cross-post, rewrite each post, add alt text, CamelCase tags and CW, drop x.com links, and have a person read replies daily.

## Output

For a full plan, deliver in order: the direct answer when the user asked one; go/skip verdict with the reason; refusals triggered; server per account with the rule that decided it; identity plan (rel=me, attribution, approval); surfaces ranked for this user with what moved the order; dated cadence; limits that bind, each with kind and source; matching templates from [references/templates.md](./references/templates.md) adapted to the user; metrics. Mark every drafted post as a draft for the named person to rewrite, and run it through your preferred humanizer skill.

## Metrics

Mastodon shows no impressions or reach [Mastodon]; measure what exists.

- Replies, boosts and quotes per post and per person, and how many distinct servers the boosters sit on.
- Follows in the week after an #introduction or launch post.
- Sessions and signups from links tagged `utm_source=mastodon`: by default Mastodon sends no referrer, and since 4.4 admins may opt in to send only the server's domain [Mastodon], so untagged clicks land as direct traffic.
- Exclude link-preview fetchers (user agents containing `Mastodon/`) from log-based analytics: every receiving server fetches the page [Mastodon repo].
- When an exec asks for impressions, say none exist and offer a weekly table instead: posts, replies, boosts, distinct booster servers, new follows, tagged sessions and signups.
- Never report follower totals as the headline, and never scrape timelines to measure sentiment.

## References

- samber/developer-relations-skills@devrel-content-calendar - fitting Mastodon slots into the quarter.
- samber/developer-relations-skills@build-in-public - the weekly narrative these posts carry.
- samber/developer-relations-skills@hackernews-playbook - the other launch-day channel, with its own rules.
- samber/developer-relations-skills@devto-playbook - where the long-form post behind a link lives.
- samber/developer-relations-skills@discord-community-playbook - when replies outgrow a public thread.
- samber/developer-relations-skills@oss-launch - the cross-channel launch plan this post belongs to.
- samber/developer-relations-skills@devrel-analytics - attributing tagged Mastodon clicks to signups.
