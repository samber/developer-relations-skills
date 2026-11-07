# Reddit platform facts

Sitewide mechanics behind the playbook. Re-check numbers against Reddit's help center (support.reddithelp.com) and the Reddit Rules (redditinc.com/policies) before quoting them.

## Contents

- Self-promotion: what Reddit itself says
- Gates and filters
- Ranking
- Enforcement
- Brand tools
- AMA mechanics
- Ads
- Disclosure law

## Self-promotion: what Reddit itself says

| Source         | Text                                                                                                                                                                                                                   | Counts                            | Kind                                          |
| -------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------- | --------------------------------------------- |
| Reddit Rule 2  | "Participate authentically in communities where you have a personal interest, and do not spam"                                                                                                                         | behaviour                         | enforced [Reddit]                             |
| Spam policy    | Business owners whose contributions are "primarily links to a business that you run" should be "thoughtful about the frequency of posting, or consider advertising"; moderators decide what is spam in their community | own links                         | enforced [Reddit]                             |
| Reddiquette    | "only 1 out of every 10 of your submissions should be your own content"; described by Reddit as informal, written by users                                                                                             | submissions                       | policed [Reddit]                              |
| Moderator help | "promotional content is not inherently considered to be spam"; some communities ban it, others use "the 10% rule" over "posting and comment history in the community"                                                  | posts and comments, per community | enforced where a subreddit adopts it [Reddit] |

Read the ratio per subreddit and per community history: a history that is mostly your own domain reads as spam to moderators whatever the arithmetic says [practitioner].

## Gates and filters

All [Reddit] unless marked.

- **Hidden thresholds.** When someone cannot post, the posting screen lists which of account age, karma and verified email they fail. "Specific karma and account age thresholds used by communities aren't disclosed".
- **AutoModerator conditions** moderators can set: account age (minutes to years), combined, post and comment karma, karma earned inside the subreddit, Contributor Quality Score.
- **Contributor Quality Score (CQS):** a classification to identify potential spammers, from past actions on the account, network and location signals, and security steps such as email verification. Five tiers, Highest to Lowest, updated regularly.
- **Reputation filter:** optional, informed by CQS, filters on account behaviour, not content.
- **Crowd Control:** moderate filters negative community karma; high adds new accounts; maximum adds non-members.
- **Approved users** skip the reputation filter and Crowd Control; moderators can grant it.
- **Rate-limit filter:** new low-karma users hit a per-community posting limit; "even earning a small amount of karma by commenting on another post within a community" helps.
- **Potential Spam:** likely spam comments are auto-collapsed.
- **Post Check:** an LLM compares a draft with the community's rules before submit; advisory only.
- **Eligibility screen limits:** it reflects only AutoModerator rules set to remove, refreshed every six hours; filter-action rules, CQS, the reputation filter and Crowd Control are not on it.
- **Gates in developer subreddits** [sub rule unless marked]: r/sysadmin 24-hour account; r/ExperiencedDevs 3+ years of experience and user flair; r/opensource 1-year account (pinned announcement) [press].

## Ranking

- **Current, published:** Hot "prioritizes posts that have recently been getting upvotes, comments"; most communities default to Hot; votes are hidden for the first hours of a post [Reddit].
- **Archived open-source code** (reddit-archive/reddit, `_sorts.pyx`) [Reddit, archived]: `hot = sign(s) * log10(max(|s|, 1)) + (created - 1134028003) / 45000`, with `s` = upvotes minus downvotes. 45,000 seconds is 12.5 hours, so a tenfold net score equals 12.5 hours of recency; the first 10 net votes weigh as much as the next 90; a net-negative post sinks. Production ranking has changed since and is unpublished.
- **Comment "Best" sort** in the archived code: Wilson lower bound with z for 80% confidence [Reddit, archived].
- **AMA posts** are pushed back up the community feed when they start [Reddit].
- **Practitioner claims, unconfirmed:** a first-hour velocity window, comments worth double an upvote, upvote-ratio thresholds, best weekday hours. Use them only as tie-breakers after the lane is right.

## Enforcement

- Vote manipulation includes multiple accounts, voting services, and "coordinated voting with an organized group of people (or bots) to target a specific post"; ladder warning, 3-day, 7-day, permanent [Reddit].
- Several accounts are allowed; never vote with more than one on the same content [Reddit].
- Reddit is testing a pause for posting substantially similar content in too many communities: "You've posted similar content multiple times. Try again in 12 hours." [Reddit, experiment]
- Shadowbans (you see your posts, nobody else does) happen "in more severe cases" of flooding [Reddit, Reddiquette].
- "Being banned across several communities dealing with the same topics" counts as community disruption [Reddit].
- Spam accounts are actioned temporarily, then permanently after a probationary period [Reddit, transparency report July-December 2024].
- AutoModerator made about seven in ten moderator removals in that period [Reddit, transparency report July-December 2024].
- Appeals for flagged accounts go through Reddit's appeal form, logged in [Reddit].

## Brand tools

- **Reddit Pro:** free organic business tools, in beta for eligible businesses in English-speaking countries; profile flair, scheduling posts on the profile and in communities you moderate, profile AMAs, Performance (views, upvote rate, comments, shares, hourly views for 48 hours), Trends. Not an ads product [Reddit].
- **Verified profiles:** grey check; businesses on Reddit Pro apply in settings, business name must match the display name; individuals in limited testing. "Verification confirms identity only. It doesn't affect moderation rules, feed ranking, ad performance" [Reddit].
- **Brand Affiliate tag:** marks a post or comment as incentivized or with commercial intent [Reddit].
- **Reposting (crossposting):** endorsed into relevant communities; random reposting "can be seen as spammy"; some communities disable it [Reddit].

## AMA mechanics

All [Reddit].

- Contact the community's moderators first; each community sets its own AMA rules.
- Schedule up to 21 days ahead; announcing up to five days before works best; up to five co-hosts; Remind Me notifications 24 hours before and at start.
- Moderators add the guest as an approved user and exclude them from filters, confirm assets 48 hours ahead, lock comments after.
- At least an hour, answered by the host personally, varied and hard questions included, no pre-written answers.
- Moderators are told to steer guests away from "a pure marketing strategy".
- Reddit reports AMAs lift community comments by up to 5% and returning users by 8%.

## Ads

"Promote your post" turns your own safe-for-work post in a public community into a cost-per-click traffic ad, community targeting only, feed placement only, daily budget; engagement does not accrue to the original; the ad stops if moderators remove the original. Fuller targeting needs Reddit Ads' advanced campaign builder [Reddit].

## Disclosure law

- US FTC Endorsement Guides (guidance interpreting the FTC Act): an employee promoting the employer's product discloses in the post; a profile naming the employer is not enough [FTC].
- FTC rule on reviews and testimonials: bans insider testimonials without disclosed material connection and buying fake social indicators [FTC].
- Documented astroturfing: a game marketing agency published, then deleted, a case study on promoting a game through fake Reddit accounts [press]; Reddit banned undisclosed AI personas used in a university experiment and sent legal demands [press].
