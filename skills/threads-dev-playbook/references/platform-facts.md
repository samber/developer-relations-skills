# Threads platform facts

Numbers and mechanics that change with app releases. Re-check each against Meta's own pages (help.instagram.com, developers.facebook.com/docs/threads, creators.instagram.com/threads, about.fb.com) before quoting it.

## Contents

- Limits, per surface
- Feed and ranking
- Discovery surfaces
- Fediverse sharing
- Instagram link and accounts
- Developer presence
- Cross-posting tools

## Limits, per surface

"Enforced" = Meta's systems or moderators act on it. "Policed" = Community members, Mastodon server admins or readers act on it without a Meta rule.

| Surface                | Limit                                                                                                                                                                                                                                                                                                                                                                       | Kind                                | Source                                                                             |
| ---------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------- | ---------------------------------------------------------------------------------- |
| Any post               | 500 characters; the API counts emoji by UTF-8 bytes                                                                                                                                                                                                                                                                                                                         | enforced                            | [Meta] Threads API posts doc                                                       |
| Any post               | One topic tag; first valid tag wins; 1-50 characters, no periods or ampersands; may contain spaces                                                                                                                                                                                                                                                                          | enforced                            | [Meta] API posts doc, newsroom March 2025                                          |
| Any post               | At most 5 unique links; over-limit API posts fail since 2025-12-22                                                                                                                                                                                                                                                                                                          | enforced                            | [Meta] API posts doc and changelog                                                 |
| Link cards             | Only on text-only posts; first link becomes the card when none is set                                                                                                                                                                                                                                                                                                       | enforced                            | [Meta] API posts doc                                                               |
| Text attachment        | Up to 10,000 characters plus one link; text-only posts; no poll; the app cannot edit it after publishing or schedule it; the API can publish it (from 2025-10)                                                                                                                                                                                                              | enforced                            | [Meta] newsroom 2025-09, API doc                                                   |
| Polls                  | Text-only posts; 2-4 options of 1-25 characters                                                                                                                                                                                                                                                                                                                             | enforced                            | [Meta] API polls doc                                                               |
| Carousel               | 2-20 images or videos; video up to 5 minutes                                                                                                                                                                                                                                                                                                                                | enforced                            | [Meta] API posts doc                                                               |
| Reply controls         | Everyone, accounts you follow, mentioned only, followers only, author only; reply approvals hold replies until approved                                                                                                                                                                                                                                                     | enforced                            | [Meta] API reply-management doc                                                    |
| API publishing         | 250 posts, 1,000 replies, 100 deletes, 500 location searches per profile per 24-hour moving window; a carousel counts as one post; `threads_publishing_limit` returns current usage                                                                                                                                                                                         | enforced                            | [Meta] API overview and posts docs                                                 |
| API access             | Without App Review only the app's testers can publish; reply and mention webhooks need advanced access                                                                                                                                                                                                                                                                      | enforced                            | [Meta] API docs                                                                    |
| Any account            | Fake engagement, inauthentic behaviour and spam are banned under Instagram Community Guidelines and Meta Community Standards; no thresholds published                                                                                                                                                                                                                       | enforced                            | [Meta]                                                                             |
| Any account            | Branded content policies name Threads as a surface; whether a paid-partnership label appears on Threads is unconfirmed                                                                                                                                                                                                                                                      | enforced                            | [Meta] Branded Content Policies summary                                            |
| Any account            | Impersonation of people and brands is banned                                                                                                                                                                                                                                                                                                                                | enforced                            | [Meta]                                                                             |
| Fediverse sharing      | Instagram-created profile, 18+, public, outside the European Region                                                                                                                                                                                                                                                                                                         | enforced                            | [Meta] help 760878905943039                                                        |
| Fediverse sharing      | Posts with limited quote or reply audience, or unsupported attachments, do not federate; replies federate only if the parent author shares                                                                                                                                                                                                                                  | enforced                            | [Meta] help 760878905943039                                                        |
| Mastodon servers       | About 80% of 400 servers that publish a Threads policy suspend or limit threads.net, a sample skewed toward blockers; Meta says Threads has interacted with over 75% of fediverse servers; mastodon.social does not block, infosec.exchange limits, mas.to suspends threads.net, threads.com and threads.instagram.com; hachyderm.io and fosstodon.org lists are not public | policed                             | [practitioner] mastodon.help and server domain-block APIs; [Meta] newsroom 2025-06 |
| Communities            | Meta decides which topics become Communities; a progress tracker shows steps for topics near the threshold; Champions choose flair; no published per-Community self-promotion rule                                                                                                                                                                                          | enforced (creation), policed (tone) | [Meta] newsroom 2025-10, 2025-12, 2026-06                                          |
| Insights               | Follower demographics need at least 100 followers; history from April 2024                                                                                                                                                                                                                                                                                                  | enforced                            | [Meta] API insights doc                                                            |
| Third-party schedulers | Their own feature gaps (alt text, polls, tags); never a Meta rule                                                                                                                                                                                                                                                                                                           | tool limit                          | [practitioner] vendor docs                                                         |

## Feed and ranking

- For You blends followed and recommended accounts and ranks by predicted interest; signals include interactions on Threads and Instagram, likes and hides [Meta: creator site, Transparency Center card].
- "Content that provokes a conversation is more likely to get recommended"; replies are "almost half of the views on Threads"; replying "is just as important as posting" [Meta: creators.instagram.com/threads].
- "Higher post frequency is linked to higher views per post", with a recommendation of at least two to five posts a week; humorous posts and media posts with text did better; the figures come from a Q1 2024 creator analysis [Meta: same page].
- Tagged posts "generally receive more views than those without one" [Meta: newsroom, March 2025].
- Links: not deliberately downranked, but link clicks carry little ranking value and people rarely like or comment on links (Mosseri, November 2024); "Links have been working much better for more than a month now" (Mosseri, June 2025) [Meta]. Outbound referrals roughly doubled year on year to mid-2025 [press: Similarweb].
- Ranking shifted toward followed accounts from late 2024 [press citing Mosseri].
- Political content is recommended by default again since January 2025, with a less, standard or more setting [Meta].
- Dear Algo (February 2026): a public post starting "Dear Algo" adjusts the poster's feed for three days; reposting someone's request applies it to your own feed [Meta]. Your Algo (June 2026): private topic dials for 1, 3 or 7 days [Meta]. Both launched in a handful of English-speaking countries.
- Unconfirmed folklore: first-hour cutoffs, "replies count 3x likes", reposts as the strongest signal, a 30-50% link penalty [practitioner].

## Discovery surfaces

- Topic tags: tap a tag or search a keyword to reach posts on it [Meta]. Profiles can list up to 10 topics (test) [Meta].
- Communities: tested across 100+ popular topics from 22 October 2025, 200+ by December 2025, "not every interest has a dedicated community yet" [Meta: newsroom]; out of beta in June 2026 with a hub, a progress tracker and Champions [Meta]. Which software topics are full Communities changes; search the app for your stack.
- Custom feeds merged into the Communities experience in 2025 [press].
- Trending: suggested in the composer; mostly current events and pop culture [Meta].

## Fediverse sharing

- Turn on: Settings > Account > Fediverse sharing [Meta].
- Others can search, follow, view, interact and reshare; in Threads you see fediverse followers and likes, see and like their replies, and search and follow fediverse users; you cannot reply back to fediverse replies, only like them [Meta: help 760878905943039]. The June 2025 newsroom post's "interact with" fediverse posts covers following, viewing and liking; replying to fediverse replies is not possible [Meta].
- Fediverse followers sit in a separate list and are not counted in the Threads follower count [Meta].
- Posts federate after the 15-minute edit window [press citing Meta]. API posts federate too [Meta].
- Turning sharing off, going private, deleting a post or the profile: Threads asks other servers to delete, "We cannot guarantee" they do [Meta].
- Handle on Mastodon: `@username@threads.net` [practitioner: webfinger check].
- Mastodon users engage Threads users more than the reverse, and interaction concentrates in a few accounts [research: Jeong et al., ASONAM 2025].
- Bridgy Fed treats a federated Threads profile like any fediverse account: follow `@bsky.brid.gy@bsky.brid.gy` from it to appear on Bluesky [practitioner: Bridgy Fed's developer, EFF guide]. It requires a display name different from the handle and a profile picture, bridges new posts only, and Bluesky replies reach back only from users who bridged themselves [practitioner: fed.brid.gy/docs]. Its docs list no Threads-specific support.

## Instagram link and accounts

- Sign up with an Instagram profile or with email or phone; Instagram data unused for the latter [Meta].
- Instagram follows do not carry over; the Instagram graph left onboarding suggestions in late 2024 but still feeds post recommendations [press citing Mosseri].
- Deleting a Threads profile no longer deletes Instagram [Meta].
- Private profiles show posts to followers only and cannot be embedded; photo, name, username and bio stay public [Meta].
- API access for profiles without a linked Instagram account from September 2025; their follower count and demographics from January 2026 [Meta: API changelog].
- Threads web moved to threads.com; the fediverse handle stays `@user@threads.net` [practitioner: webfinger check].
- DMs exist for support hand-offs [Meta: newsroom 2025-07]. Live Chats started with select creators and Community Champions as hosts, co-hosting added from June 2026; not a surface for most devtool accounts yet [Meta: newsroom 2026-04, 2026-06].

## Developer presence

- 500 million monthly users in June 2026 [Meta]; mobile daily users passed X worldwide in 2025, X still led in the US in April 2026, and Threads web visits were under 5% of X's in mid-2025 [press: Similarweb].
- No developer survey breaks out Threads; Stack Overflow's 2025 community-platform question lists no Threads row [practitioner].
- Spot checks (November 2026): one prominent engineer clearly active, several dormant since 2024; GitHub, Microsoft, Cloudflare and JetBrains hold profiles, some nearly empty; none of 15 devtool brands tested had fediverse sharing on [practitioner].

## Cross-posting tools

- Buffer: Threads multi-post threads up to 25 on web, no alt text on Threads, one topic, first link becomes the card [practitioner: vendor docs].
- Publer: Threads threads up to 25, alt text supported [practitioner: vendor docs].
- Hootsuite: posting supported; mentions and polls missing on Meta's side; alt text reported missing [practitioner].
- Self-hosted schedulers need their own Meta app and App Review before posting beyond testers [Meta].
- Length per network: Threads 500, X 280 on free accounts, Bluesky 300, Mastodon 500 by default [practitioner].
