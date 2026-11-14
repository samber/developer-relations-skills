# Mastodon platform facts

Numbers and rules that change with releases and with each server's moderators. Re-check each against Mastodon's docs (docs.joinmastodon.org), the server's `/about` page, or its public API (`/api/v2/instance`, `/api/v1/instance/rules`) before quoting it.

## Contents

- Limits, per server and surface
- Server rules for brand and project accounts
- Hachyderm entity accounts
- Discovery mechanics
- Identity and verification
- Measured account behaviour
- Self-hosted brand servers
- Cross-posting and bridges
- Measurement
- Post format

## Limits, per server and surface

"Enforced" = software or moderators act on it. "Policed" = the community reacts without a written rule.

| Surface            | Limit                                                                                                                                                                                                                                                                 | Kind     | Source                                |
| ------------------ | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------- | ------------------------------------- |
| Any account        | API: 300 calls per 5 minutes per account and per IP; media upload 30 per 30 minutes; status delete or unboost 30 per 30 minutes; account creation 5 per 30 minutes per IP; servers may change defaults                                                                | enforced | [Mastodon] rate-limits doc            |
| Any account        | No posting-frequency limit in Mastodon itself; quotas exist only in server rules                                                                                                                                                                                      | -        | [Mastodon]                            |
| Post length        | 500 characters by default; links count 23; forks differ: hachyderm.io 2263, infosec.exchange 11,000, techhub.social 3072, social.vivaldi.net 1337, mas.to 1000, social.tchncs.de 800                                                                                  | enforced | [Mastodon], each server's API         |
| hachyderm.io       | Every account using automated or scheduled posts, personal ones included: max 5 a day, exceptions on request; bot flag; `#hachybots` (curated `#hachygrator`); bots reply only when tagged; bots need moderator approval                                              | enforced | [server] bot and curated account docs |
| hachyderm.io       | Entity accounts must be visibly approved; unapproved ones are removed                                                                                                                                                                                                 | enforced | [server] rules                        |
| infosec.exchange   | Accounts posting more than 50% by automation must carry the bot label; all automated posts max one an hour, 24 a day public, no limit quiet public                                                                                                                    | enforced | [server] rules                        |
| infosec.exchange   | LLM- and AI-operated accounts removed without notice; terms ban scrapers and data-mining tools; rule breaks may bring an immediate permanent ban                                                                                                                      | enforced | [server] rules, terms                 |
| fosstodon.org      | No commercial promotion, link-only posts with excessive hashtags, or repetitive self-promotion for profit; no automated posting without monitoring; no dog-piling; model training needs consent; moderators take "a pragmatic and individual approach to each report" | enforced | [server] rules, code of conduct       |
| mas.to             | Commercial advertising, promotion of services and SEO count as spam; bots respect `#nobot` and post quiet public unless under one post an hour                                                                                                                        | enforced | [server] rules                        |
| techhub.social     | No direct advertising, no unsolicited product or job DMs, no SEO-only accounts; bots post quiet public                                                                                                                                                                | enforced | [server] rules                        |
| social.tchncs.de   | No advertisements; no exclusive cross-posting from other platforms                                                                                                                                                                                                    | enforced | [server] rules                        |
| floss.social       | Public image, video or audio needs a media description; repeated misses escalate                                                                                                                                                                                      | enforced | [server] rules                        |
| mastodon.social    | AI use must be disclosed; accounts may not solely post AI content; no brand or bot rule                                                                                                                                                                               | enforced | [server] rules                        |
| Hashtag count      | Hachyderm suggests about five tags per two paragraphs as an upper bound; more may be reported as spam                                                                                                                                                                 | policed  | [server] hachyderm.io docs            |
| Unattended mirrors | Read as "zombie accounts"; some servers ban them                                                                                                                                                                                                                      | policed  | [practitioner] fedi.tips              |
| Federation         | A server suspended by another loses all follows from it, not restored if lifted                                                                                                                                                                                       | enforced | [Mastodon] moderation doc             |
| Account move       | Followers move, posts do not; 30-day cooldown                                                                                                                                                                                                                         | enforced | [Mastodon]                            |

## Server rules for brand and project accounts

API snapshot; active-user counts drift monthly.

| Server             | Active per month | Registration                   | Brand stance                                                                            |
| ------------------ | ---------------- | ------------------------------ | --------------------------------------------------------------------------------------- |
| mastodon.social    | ~270,000         | open                           | no brand rule                                                                           |
| infosec.exchange   | ~10,800          | approval with reason           | "legitimate brand accounts" permitted                                                   |
| mas.to             | ~7,900           | invite-only                    | promotion is spam                                                                       |
| hachyderm.io       | ~7,400           | approval with reason, 18+      | entity accounts approved; corporate paid                                                |
| fosstodon.org      | ~7,300           | invite-only                    | no commercial promotion; "toot your own horn" for personal work                         |
| social.vivaldi.net | ~6,700           | open                           | company-run; no promotion of AI, crypto or NFTs                                         |
| techhub.social     | ~3,700           | approval with reason           | no direct advertising                                                                   |
| social.tchncs.de   | ~3,600           | approval with reason           | no advertisements                                                                       |
| ruby.social        | ~800             | approval, must state Ruby link | no advertising or excessive promotion; rejects sign-ups that exist to promote a company |
| floss.social       | ~600             | waitlist                       | alt text required                                                                       |

## Hachyderm entity accounts

- Personal accounts may post about their employer; a username that reads as a company makes the account that type [server].
- Corporate: invite-only; apply by email to the admins (the verification page lists creating the handle before the approval issue, so post nothing as the brand until approved); verified with the company domain; paid since March 2023: fewer than 25 employees $650 a year, 25-49 $1,000, 50-99 $1,500, 100-149 $2,000, 150-999 $2,500, 1,000+ employees or more than 50,000 followers custom [server].
- Project accounts are free only for community-governed projects. A product of a parent company, a single company sponsor, or a free and paid tier makes it corporate; named examples: CNCF projects, Go, Terraform [server].
- No duplicate project accounts and no project account that only relays PR or build events [server].
- Approval runs through a GitHub issue on the server's community repo, then a profile field `APPROVED` linking the approval page; add it after approval, or re-save the profile once approval is live [server, Mastodon].
- Bots need moderator approval and Hachyderm verification; a project may not run a bot dedicated to PR or build events; bots cannot respond to hashtags or keywords without being tagged; bots that handle data, even public data, need opt-in consent; automated posts may skip "Say It Once" only within the daily cap [server].
- Username parking is prohibited; parked accounts are suspended [server]. Small startups for whom the fee is a burden are invited to reach out [server].
- Specialized Account Expectations: "fines doubled", "Say It Once", "We don't want to host an ad server", moderators may ask the account to leave [server].
- Accounts with more than 10,000 followers elsewhere email the admins before announcing their arrival [server].

## Discovery mechanics

- Home: chronological from followed accounts and followed hashtags; a public post with a followed hashtag appears in Home even as a reply [Mastodon].
- A reply reaches a follower's Home only if they follow both the replier and the person replied to; a post that starts with an @mention is not a reply [Mastodon].
- Live feeds show only public posts the server already knows; "There is no global shared state between all servers" [Mastodon].
- Boost reshares into the booster's followers' Home feeds; quote sits in the Boost menu since 4.5 [Mastodon].
- Quote posts: author picks Anyone, Followers only or Just me, per post or as default; followers-only posts cannot be quoted; the quoted author is notified and can revoke the quote irreversibly; posts from 4.4 and earlier are not quotable until their policy is changed [Mastodon].
- Trends are per server; hashtags need admin approval to trend unless auto-approval is on; links and posts are moderated too; no threshold is published [Mastodon].
- Search: accounts and hashtags for everyone; full-text search is optional per server and covers public posts only from accounts that opted in, plus your own interactions [Mastodon]. Quiet public posts never appear in search, live feeds or explore [Mastodon].
- Collections (4.6+): shareable lists of up to 25 accounts; only accounts with "Feature profile and posts in discovery algorithms" on can be added; members are notified and can remove themselves; curators are not included automatically [Mastodon blog].
- Readers can filter public timelines by detected language; set the language per post [Mastodon].
- Profile directory and explore are opt-in; featured hashtags show on the profile with last use and count; up to 5 pinned posts; every account and tag page has RSS [Mastodon].
- Weekly volume seen from mastodon.social, 7-day sums of uses and posting accounts [practitioner measurement]: #OpenSource ~3,100 / ~1,500; #Python ~590 / ~380; #FOSS ~510 / ~400; #WebDev ~460 / ~310; #DevOps ~440 / ~260; #Rust ~330 / ~220; #Introduction ~140 / ~135; #Kubernetes ~110 / ~90; #Golang ~90 / ~70. Check your own stack's tags at `/api/v1/tags/<tag>` on a large server.

## Identity and verification

- No blue check: "Document-based verification and blue ticks are not possible without a central authority" [Mastodon].
- rel=me: the profile field holds an HTTPS link; the page must contain `<a rel="me">` or `<link rel="me">` whose href equals the profile URL, in raw HTML without JavaScript, outside any iframe, response under 1 MB; the hostname must not change after IDN normalization; a fallback accepts a first link that redirects to the profile. Verification runs on profile save; re-save after fixing [Mastodon].
- Own server: "there is no better way to verify your identity than to host Mastodon on your own domain" [Mastodon].
- Author attribution (4.3+): `<meta name="fediverse:creator" content="@user@server">` in the article head, plus the domain in the author's "Websites allowed to credit you" setting; previews then show the byline [Mastodon]. Duplicate tags from two plugins break it [practitioner].
- Profile: link shorteners "actively discouraged" [Mastodon]. Field count and lengths are per server: display name 40 characters and bio 500 on hachyderm.io, fosstodon.org and mastodon.social, display name 80 and 6 fields on infosec.exchange; read `configuration.accounts` at `/api/v2/instance`.

## Measured account behaviour

Last 40 posts per account from the public API [practitioner measurement]; small samples.

| Account                    | Followers | Original / Reply / Boost | Images with alt text |
| -------------------------- | --------- | ------------------------ | -------------------- |
| Thunderbird                | ~40k      | 7 / 33 / 0               | 17 / 17              |
| Nextcloud                  | ~38k      | 22 / 17 / 1              | 11 / 21              |
| KDE                        | ~37k      | 8 / 11 / 21              | 6 / 6                |
| Codeberg                   | ~29k      | 5 / 31 / 4               | 5 / 5                |
| Fedora                     | ~27k      | 21 / 15 / 4              | 0 / 1                |
| GNOME                      | ~26k      | 9 / 4 / 27               | 4 / 4                |
| Python Software Foundation | ~15k      | 30 / 2 / 8               | 1 / 1                |
| Tailscale                  | ~9k       | 35 / 5 / 0               | 1 / 24               |
| Fly.io                     | ~0.5k     | 39 / 1 / 0               | 0 / 7                |

- None used content warnings; none carried the bot flag.
- Unofficial squats exist, for example an "Unofficial GitLab account" on mastodon.social [practitioner].

## Self-hosted brand servers

- BBC: social.bbc, 2023, closed to outside sign-ups, run as an experiment [press].
- Linux Foundation: social.lfx.dev on managed hosting, with project accounts [practitioner].
- EU Voice (EDPS): opened April 2022, closed 18 May 2024, unable to secure new ownership [press].
- mozilla.social: opened 2023, closed 17 December 2024 with about 270 active users [press]; the domain now answers HTTP 410.
- Servers that want listing on joinmastodon.org commit to active moderation, daily backups, a second person with emergency access, and 3 months' notice before shutting down [Mastodon covenant].
- Mastodon 4.5 lets admins show the local feed to visitors, "useful for small organization servers" [Mastodon].

## Cross-posting and bridges

- Bridgy Fed: opt-in both ways; requires an account at least a week old, a profile picture and a display name different from the handle; older posts are not backfilled; bridged accounts cannot yet be quote-posted on Mastodon except by themselves; a Mastodon account follows `@bsky.brid.gy@bsky.brid.gy` and accepts its follow; quiet public, followers-only and DMs are not bridged; Bluesky truncates past 300 characters; replies from unbridged users arrive as an hourly DM digest; some servers block the bridge [practitioner: Bridgy Fed docs].
- Threads: opt-in fediverse sharing for adult public profiles; restricted-reply posts, replies to non-federated posts and polls do not federate; a fediverse feed in Threads since June 2025 [press: Meta engineering and newsroom]; reported unavailable in the EU region [practitioner]; hundreds of server admins signed the FediPact pledge to block Meta [practitioner].
- Moa is archived and the Twitter-era crossposters shut down when the X API closed [practitioner].

## Measurement

- Exposed per post: replies, boosts, favourites, quotes; per account: followers. No views or impressions [Mastodon].
- By default Mastodon sends no referrer on outbound links; since 4.4 admins can opt in to send only the server's domain [Mastodon changelog]. Add UTM tags yourself [practitioner].
- Exclude preview fetchers (user agent containing `Mastodon/`) from log-based analytics [practitioner].
- Link previews: every server receiving the post fetches the URL within about a minute; one measurement logged 578 requests in 20 seconds for a 1,500-follower account [Mastodon repo issue].

## Post format

- Images: up to 4, 16 MB each, downscaled to 8.3 megapixels; or one video up to 99 MB; or one audio file; files or a poll, not both [Mastodon].
- Hashtags: letters, digits and underscores, not digits only [Mastodon].
- Content warning collapses the post behind a summary; adding one marks attached media sensitive [Mastodon].
- Visibility: Public, Quiet public (formerly Unlisted), Followers, Private mention; past public posts cannot be made private [Mastodon].
