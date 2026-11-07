# X platform facts

Numbers and mechanics that change with releases, plan reshuffles and staff posts. Re-check each against X's own pages (help.x.com, docs.x.com, the xai-org/x-algorithm README) before quoting it. Archived copies of help pages can be months stale: one still describes Communities after they closed.

## Contents

- Limits, per surface
- Ranking
- Links
- Paid plans
- Surfaces and their status
- Audience evidence
- Cross-posting mechanics
- Sources

## Limits, per surface

"Enforced" = X's systems or moderators act on it. "Policed" = the post's author, contributors or the people you reply among act on it without a published rule.

| Surface                          | Limit                                                                                                                                                                                                                                             | Kind                | Source                                     |
| -------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------- | ------------------------------------------ |
| Account without a checkmark      | 50 original posts and 200 replies per day, split into semi-hourly limits (text present from mid-May 2026; the same page still shows an older 2,400 updates figure)                                                                                | enforced            | [X] help center "X limits"                 |
| Any account                      | 500 DMs per day; 400 follows per day; follow ratios apply past 5,000 follows; limits count every device and API client and can drop under heavy load                                                                                              | enforced            | [X] help center "X limits"                 |
| Any account                      | Up to 10 accounts for non-duplicative purposes (per language, region, support line); no account may boost another; no identical content across accounts, translations allowed                                                                     | enforced            | [X] Authenticity policy                    |
| Any account                      | Coordinating or paying others to inflate metrics, engagement exchanges, follow churn, aggressive engagement to drive traffic, mass reports: banned. Authentic coordination to express support is allowed                                          | enforced            | [X] Authenticity policy                    |
| Any account                      | Bulk unsolicited replies or mentions, hashtag hijacking, excessive unrelated hashtags, activity that is mostly links without commentary, post-and-delete, swapping a link's destination after posting: banned                                     | enforced            | [X] Authenticity policy                    |
| Links                            | URL redirect chains are scored; posts mentioning non-followers with a low-quality URL get a spam label (high-reputation and gray-check government accounts exempt)                                                                                | enforced            | [X] scarecrow rules in xai-org/x-algorithm |
| Automation                       | Scheduling and API posting of original content allowed; auto-replies only to users who engaged with you, one per interaction, with opt-out; brand auto-response campaigns need X approval; AI reply bots need written approval                    | enforced            | [X] automation rules                       |
| Automation                       | Keyword-triggered replies or mentions, automated likes, bulk or aggressive follow and unfollow, bulk list adds, unsolicited automated DMs, automated trend posts: banned                                                                          | enforced            | [X] automation rules                       |
| Sponsored posts                  | Paid Partnership label through the composer's content disclosure setting, from 2026-03-01; policy text as reported says sponsored posts must carry it, with penalties up to suspension; launch coverage framed it as an option creators can apply | enforced (reported) | [X via press]                              |
| Replies on others' posts         | Author can limit who replies and hide replies                                                                                                                                                                                                     | enforced            | [X]                                        |
| Reply etiquette                  | Product links dropped in a stranger's thread draw "not interested", mutes and blocks                                                                                                                                                              | policed             | [practitioner]                             |
| Community Notes                  | Contributors attach context to misleading posts; benchmark and "fastest" claims attract them                                                                                                                                                      | policed             | [practitioner]                             |
| Parody, fan, commentary accounts | Label, non-identical avatar, the word in the name and the bio                                                                                                                                                                                     | enforced            | [X] Authenticity policy                    |
| API posting                      | Pay per use: $0.015 per post, $0.20 per post containing a URL                                                                                                                                                                                     | enforced (billing)  | [X] docs.x.com pricing                     |
| Third-party schedulers           | Own caps and link handling; one hosted tool strips links from X posts                                                                                                                                                                             | tool limit          | [practitioner]                             |

## Ranking

Score = sum of weight times the model's predicted probability of each action, for one viewer. X's code comments say weights combine how much an action is valued with how rare it is: a report is over 1,000 times rarer than a like, so it is weighted heavily just to register. The 2023 weights were set so each weighted action contributed about equally on average. Never read a weight ratio as value [X].

| Action                                | 2023 heavy ranker         | 2026 Phoenix ranker                                                                                                |
| ------------------------------------- | ------------------------- | ------------------------------------------------------------------------------------------------------------------ |
| Like                                  | 0.5                       | 0.708                                                                                                              |
| Repost                                | 1                         | 1                                                                                                                  |
| Reply                                 | 13.5                      | 5; plus 15 on a post whose author the viewer mutually follows (launched 2026-07-13 at 20, cut to 15 on 2026-07-24) |
| Reply the author engages with         | 75                        | not a separate published weight                                                                                    |
| Quote                                 | -                         | 5                                                                                                                  |
| Share                                 | -                         | 2.118                                                                                                              |
| Share via DM                          | -                         | 5                                                                                                                  |
| Share via copy link                   | -                         | 20                                                                                                                 |
| Follow author                         | -                         | 4                                                                                                                  |
| Profile click then engage             | 12                        | profile click 0                                                                                                    |
| Conversation click then engage        | 11                        | click 0.274                                                                                                        |
| Conversation click and stay 2 minutes | 10                        | -                                                                                                                  |
| Continuous dwell and click dwell      | -                         | small positive weights                                                                                             |
| Video playback, video quality view    | 0.005 (50% watched)       | 0                                                                                                                  |
| Open link                             | -                         | 0.2                                                                                                                |
| Not interested, show less             | -74 (with block and mute) | -53.07                                                                                                             |
| Mute author                           | (in the -74)              | -58.8                                                                                                              |
| Block author                          | (in the -74)              | -31.2                                                                                                              |
| Report                                | -369                      | -234                                                                                                               |

- 2026 after scoring: each extra post from the same author in one feed response multiplied by 0.5 per post, floor 0.25; out-of-network posts multiplied by 0.75, also replies and reposts from followed accounts; out-of-network replies dropped [X].
- Small-account boost: an original post (not a reply or repost) from an account with at most about 50,000 followers, under about two hours old and under about 200 Home views, already near the top of the ranked list, can be lifted into one slot per feed request; eligibility depends on experiment arms [X].
- Posts older than 48 hours leave the feed [X].
- README updates: ranking predicts time spent better (2026-10-08); retrieval trained to surface posts people read to the end (2026-10-13) [X].
- X's code comments say navigating to a post from a group chat link has no impact on the main ranker, and reports count only on posts served in Home; penalties apply per viewer, not as a global demotion [X]. The small-account boost below reads raw like counts, capped by the post's Home views [X].
- 2023 extras: For You about half in-network on average; feedback fatigue discount over 140 days; verified authors multiplied by 4 in-network and 2 out-of-network in code defaults, absent from the 2025 and 2026 public code [X]. Code defaults sync from production but many values come from a configuration system, so code absence is not proof [X].
- Musk said the feed has been "purely AI" since November 2025 and the Following tab is ranked by Grok, with chronological still available [X via press].

## Links

- 2023-08-07: Musk said on-platform content gets boosted over links because time spent on a link is short [X]. In August 2023 the press measured multi-second t.co delays on links to Threads, Bluesky, Substack and news sites; most were lifted within hours [press].
- 2024-11-24: Musk advised putting the link in the reply "to stop lazy linking" [X].
- 2025-10-17: Musk: a link with almost no description gets weak distribution, one with an interesting description or image distributes [X].
- 2025-10-19: Nikita Bier: links get lower reach because the in-app browser covers the post and people forget to like or reply; an iOS change collapsed the page so people can react (shipped about 2025-11-05; link prefetch inflated publisher traffic counts) [X; press].
- 2026-07-28/29: Bier: "you do not need to put the links in replies anymore"; Musk: "We haven't for over a year" [X via press].
- 2026 public code: no explicit link penalty; opening a link is a small positive signal [X].
- Measurements disagree: Buffer found link posts from accounts without a checkmark at a median of zero engagement since March 2025, and checkmark link posts at about a third of text-post engagement; OpenTweet found no impression gap on 28,124 posts in 2026 [practitioner].
- Testing on one account: at under 100 impressions a post, a few posts per arm cannot separate placement from topic; alternate placements across at least 10 comparable posts each and compare link clicks per impression [practitioner].

## Paid plans

X folded Premium into "The X Subscription" (plans named Plus, Premium, Super, Ultra), one plan covering X, Grok and Cursor; existing subscribers keep their old plan, price and features until they switch [X]. Check current prices on X's pricing page before quoting any. "Premium-level" in the skill means any plan that includes the checkmark and Articles; X publishes no mapping from legacy tiers to the new plan names, so check the plan page.

| Plan                      | Price                                                      | Check                  | Reply prioritization                                | Relevant features                                                           |
| ------------------------- | ---------------------------------------------------------- | ---------------------- | --------------------------------------------------- | --------------------------------------------------------------------------- |
| No plan                   | -                                                          | none                   | none                                                | 280 characters; per-post metrics in the app; 50 posts and 200 replies a day |
| Legacy Basic              | about $3/month                                             | none                   | small                                               | Edit, longer posts; still no checkmark                                      |
| Legacy Premium            | about $8/month                                             | blue, after review     | larger                                              | Articles, analytics dashboard, livestreaming                                |
| Legacy Premium+           | about $40/month                                            | blue                   | largest                                             | No ads, occasional promoted content                                         |
| Business gold, entry tier | $200/month or $2,000/year                                  | gold                   | org level                                           | No affiliate badges; time-limited ad credit                                 |
| Business gold, full tier  | $1,000/month or $10,000/year, plus $50/month per affiliate | gold, affiliate badges | org level, Premium+ benefits for org and affiliates | Affiliates, impersonation monitoring, priority support                      |

- Articles: Premium, Premium+, any X Subscription plan, businesses and organizations; not Basic [X].
- Reply prioritization: every paid plan, Basic included, scaled by tier ("larger" on Premium, "largest" on Premium+); the 2023 FAQ called it "a slight preference" for verified replies; no feed or search multiplier stated [X].
- Buffer, 18.8M posts from 71,000 accounts (August 2024 to August 2025): median impressions under 100 without a plan, about 600 Premium, over 1,550 Premium+; gap widened from January 2025; correlational, since payers also post more [practitioner].
- The business tiers were called Verified Organizations until a rename to Premium Business around October 2025 [press].

## Surfaces and their status

- **Communities:** closed 2026-05-30 (first announced for May 6) after X said fewer than 0.4% of users used them and they produced 80% of spam reports; admins were pointed to XChat group chats [X via press].
- **Articles:** long-form with headings, media, embedded posts; editing unpublishes until republished [X].
- **Edit:** paid plans, one hour, up to five edits; threads, replies and posts from third-party tools cannot be edited [X].
- **Spaces:** up to 13 speakers; scheduling; recordings replayable until the host deletes them; 30-second clips; started from mobile; missing from the rebuilt Android app in July 2026 with a return promised [X; press].
- **Live:** Live Studio announced 2026-07-01; streaming for paid plans per press [X via press].
- **Analytics:** full dashboard for Premium and above since 2024-06-13; Post Activity export capped at 30 days; API non-public metrics only for the author's last 30 days [X].
- **Staff:** Nikita Bier stepped down as head of product around 2026-08-05; later posts are an advisor's [press].

## Audience evidence

- Stack Overflow 2025 (49,009 responses): community platforms led by Stack Overflow 84%, GitHub 67%, YouTube 61%, Reddit 54%; X absent from the top list [survey].
- State of JS 2025 (13,002 responses): X appears as a minor news source and a channel respondents came from, next to Bluesky [survey].
- Pew, US adults who use X: 23% (2021), 22% (2023), 21% (2025); 10% daily; LinkedIn rising [survey].
- Shiffman and Wester, Integrative and Comparative Biology 65(3), 2025: of 813 scientists, nearly 40% deleted X and about half now mainly use Bluesky; self-selected Bluesky sample [research].
- Quelle et al., arXiv 2505.24801: 276,431 scholars linked from X to Bluesky; quick reconnection with prior contacts predicts staying [research].
- "The Failed Migration of Academic Twitter", arXiv 2406.04005: most academics who moved to Mastodon did not stay active [research].
- No peer-reviewed study measures software developers specifically; claims about developer sub-segments (AI tooling and indie on X, web-platform and European open source on Bluesky) are practitioner observations.
- Platform counts conflict: X claimed 600M monthly users in 2024, third-party estimates nearer 450M; Threads passed X on daily mobile users in January 2026; Bluesky had 41.4M registered accounts at the end of 2025 with daily activity well below its late-2024 peak [press].

## Cross-posting mechanics

- No bridge carries X posts anywhere [practitioner].
- Bridgy Fed: Bluesky and the fediverse; only the bridged account opts in, and people on the other network follow it without bridging themselves; public posts only; replies, likes and follows flow both ways; long fediverse posts truncate at 300 characters; some Mastodon servers block it [platform docs].
- Threads fediverse sharing: opt-in, public profiles, 18+; Mastodon users follow, like, reply and repost federated posts [platform docs].
- Follower moves: a browser extension re-finds X follows on Bluesky by handle, name or X handle in bio and misses many; Bounce moves graphs between Mastodon and Bluesky [practitioner; press].
- Lengths: X 280 without a plan, Bluesky 300, Mastodon 500 with each link counted as 23, Threads 500.

## Sources

- Ranking code: github.com/xai-org/x-algorithm (README, `home-mixer/params/param.rs`, `vm-ranker/params.rs`, `docs/BIDIRECTIONAL_BOOST_CHANGE.md`, `botmaker-rules/scarecrow/`); github.com/twitter/the-algorithm-ml `projects/home/recap/README.md`; blog.x.com engineering post "Twitter's Recommendation Algorithm" (2023).
- Rules: help.x.com pages "X limits", "Authenticity", "X automation rules", "About X Premium", "About Articles", "Spaces", "Edit post", "Premium Business".
- API pricing: docs.x.com/x-api/getting-started/pricing.
- Staff posts on links: x.com/elonmusk/status/1688663775744786432, x.com/elonmusk/status/1860812342210240995, x.com/elonmusk/status/1979182892904308917, x.com/nikitabier/status/1979994223224209709.
- Press: socialmediatoday.com coverage of X ranking and link changes (2023-08-15, 2025-10-02 Buffer Premium study, 2025-10-19, 2025-11-30, 2026-01-20); techtimes.com 2026-04-23 on Communities; techcrunch.com 2026-03-02 on the Paid Partnership label; washingtonpost.com and themarkup.org 2023 on t.co delays.
- Audience: survey.stackoverflow.co/2025; 2025.stateofjs.com; pewresearch.org social media fact sheet; arxiv.org/abs/2505.24801; arxiv.org/abs/2406.04005; doi.org/10.1093/icb/icaf127.
- Cross-posting: fed.brid.gy/docs; engineering.fb.com 2024-03-21 "Threads has entered the fediverse"; docs.joinmastodon.org/user/posting.
