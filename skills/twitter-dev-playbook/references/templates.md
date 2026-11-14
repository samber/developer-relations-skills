# X templates

Examples for a fictional company, Quillfeather (log search for Postgres), its founder Marta and its advocate Ines. Adapt every line to the person posting and rewrite it in their own voice; never post these as written.

## Contents

- Profiles
- First posts
- Replies
- Launch week
- Launch thread
- A thread about you
- Articles and long posts
- Brand hub posts
- Spaces AMA
- Disclosure lines
- Cross-post rewrites
- Go-or-skip summary

## Profiles

```
Name: Ines Okafor
Bio: Developer advocate at @quillfeather (log search for Postgres). Query plans, bad indexes, slow migrations. Posts are mine.

Name: Quillfeather
Bio: Log search for Postgres. Releases, incidents, docs. Built by @marta_dev and team. Status: status.quillfeather.dev
Pinned: latest release post with a 30-second demo
```

```
// ✗ Bad - brand voice on a person, no employer, growth bait
Bio: 🚀 Building the #1 AI-powered observability platform | DMs open | Follow for daily tips!
```

## First posts

```
I spend most days inside Postgres EXPLAIN output. This week: a 40-minute report query that dropped to 2 seconds once we stopped sorting 9M rows we then threw away. Plan before and after below. What's the worst sort you've found in a plan?
[image: two plans side by side, alt text describing both]
```

```
// ✗ Bad - broadcast, link-first, hashtags, nothing to reply to
Excited to announce Quillfeather is now on X! Check us out: quillfeather.dev #Postgres #DevOps #AI #Startup
```

## Replies

Substantive reply under a framework author's post:

```
Hit the same thing on 16. The planner picks the seq scan because n_distinct is stale after the bulk load; ANALYZE right after the COPY fixed it for us. Happy to share the before/after plan if useful.
```

Reply that mentions the product, only when asked or directly relevant:

```
(I work on Quillfeather, so weigh this.) For a single service pg_stat_statements gets you most of the way. A tool only earns its place once you're correlating slow queries across services.
```

```
// ✗ Bad - link drop in a stranger's thread
Great thread! We built a tool for exactly this 👉 quillfeather.dev
```

## Launch week

Teaser, founder account, two to five days before:

```
Spent three months on one problem: finding which deploy made a query slow. Thursday we ship it. Here's 10 seconds of it catching a regression from a migration.
[video, 10 s, captions on]
```

Launch post, founder account, link with a standalone caption:

```
Quillfeather 2.0 is out: point it at your Postgres logs and it tells you which deploy made which query slow, with the plan diff.

Free for one database. Self-hosted or cloud.

30-second demo below. Docs: quillfeather.dev/docs/2-0
[video, 30 s, captions on]
```

First reply by the same person, if the account's own test favours link-in-reply:

```
Docs and the migration note for 1.x users: quillfeather.dev/docs/2-0
Happy to answer anything here today.
```

Staff quote-posts, each with their own angle:

```
Ines: The part I fought for: plan diffs render in the terminal, no browser needed. Try it on the slowest query you have and tell me what it finds.

Engineer: Under the hood this is a 40-line change to how we fingerprint queries. Writing up why next week.
```

```
// ✗ Bad - same copy from every account, coordinated likes
Brand, Ines, Marta, three engineers all post: "Quillfeather 2.0 is here! 🚀 quillfeather.dev"
Slack message: "Everyone like and repost in the next 10 minutes please!"
```

## Launch thread

Use only when the story needs steps; the first post must stand alone, because each post is scored on its own.

```
1/ Quillfeather 2.0 tells you which deploy made which Postgres query slow, with the plan diff. 30-second demo below. Benchmark method linked in 4/.
[video, 30 s, captions on]

2/ How it works: we fingerprint each query before normalizing it, so two queries that look alike stop sharing a history.

3/ What it costs: about 3% CPU on the log shipper at 50k queries per second on our test box.

4/ Method and raw numbers: quillfeather.dev/bench/2-0 (hardware, dataset, how to rerun it).

5/ Free for one database. Docs: quillfeather.dev/docs/2-0. I'm answering questions here for the next two hours.
```

```
// ✗ Bad - hook with nothing to check, claim invites a Community Note
1/ 🧵 We just built the FASTEST log search ever made. 10x faster than everything. Here's why everyone is switching 👇
```

## A thread about you

First reply, from a named person, within the hour:

```
I'm on the Quillfeather team. You're right that ingestion stalled for EU cloud users from 14:02 UTC. We've found the cause (a stuck queue consumer) and are draining the backlog now. Live updates: status.quillfeather.dev. I'll post here when it's fully resolved.
```

Follow-up, quoting the original thread:

```
Resolved at 14:40 UTC. No data lost; delayed logs were backfilled. Root cause and what we changed: status.quillfeather.dev/incidents/2026-11-03. Thanks for flagging it publicly, it got us moving faster.
```

Security report posted publicly:

```
Thanks for this. Please send the details to security@quillfeather.dev (PGP key on that page) so we can fix it before it's exploitable; we credit reporters in the advisory.
```

```
// ✗ Bad - defensive, deleted later, staff pile-on
"This is false and we'll be contacting our lawyers." (post deleted an hour later; five employees reply "works fine for me!")
```

## Articles and long posts

```
Title: Why we fingerprint queries before we normalize them
1. The bug: two different queries got the same fingerprint (with the plans)
2. What we tried first, and the benchmark that killed it
3. The fix, in 40 lines
4. What it costs at 50k queries per second
5. What we still get wrong
Link the launch post at the end; end with a question for practitioners.
```

## Brand hub posts

```
Release 2.0.3: fixes plan diffs for partitioned tables (thanks @someone for the report). Changelog: quillfeather.dev/changelog
```

```
Incident: ingestion delayed up to 12 minutes for EU cloud users from 14:02 to 14:40 UTC. Resolved. Write-up: status.quillfeather.dev
```

## Spaces AMA

```
Thursday 17:00 UTC: Marta and Ines take questions on 2.0 for 45 minutes. Bring your slowest query. Recording stays up after.
```

## Disclosure lines

Sponsored creator post (Paid Partnership label on, plus words in the copy):

```
Paid partnership with @quillfeather. I ran it on my own side-project database for a week; here's what it found and what it missed.
```

Gifted licence:

```
Quillfeather gave me a free licence to test. No other payment. Notes below.
```

## Cross-post rewrites

Source (X):

```
Quillfeather 2.0 is out: point it at your Postgres logs and it tells you which deploy made which query slow, with the plan diff. Free for one database. 30-second demo below.
```

Bluesky (300 characters, alt text, no X handles, posted natively):

```
Quillfeather 2.0 is out. Point it at your Postgres logs and it shows which deploy made which query slow, with the plan diff. Free for one database. Demo video has captions. quillfeather.dev/docs/2-0
```

Mastodon (500 characters, hashtags for discovery, content in the post):

```
Quillfeather 2.0: point it at your Postgres logs and it shows which deploy made which query slow, with the plan diff side by side.

Free for one database, self-hosted or cloud. I'm around today for questions.

https://quillfeather.dev/docs/2-0

#Postgres #PostgreSQL #DevOps
```

```
// ✗ Bad - mirror
RT @marta_dev: Quillfeather 2.0 is here 🚀 x.com/marta_dev/status/...
```

## Go-or-skip summary

```
Verdict: Go small.
Sample: 100 recent stargazers; 41 list a social link: 19 X, 14 Bluesky, 6 Mastodon, 2 LinkedIn.
Who posts: Ines, 30 minutes on weekdays, Premium.
Plan: replies only on X for 6 weeks; native posts on Bluesky; brand account posts releases.
Review: 2027-02-15, on follows, UTM visits and stars per hour, X against Bluesky.
```
