# Threads templates

Examples for a fictional company, Quillfeather (log search for Postgres), and its advocate Ines. Adapt every line to the person posting and rewrite it in their own voice; never post these as written.

## Contents

- Profiles
- First posts
- Replies
- Original posts with a topic tag
- Text attachment
- Community and Dear Algo
- Disclosure lines
- Cross-post rewrite
- Launch day
- Skip verdict

## Profiles

```
Ines (created from her own Instagram, fediverse sharing on, outside the EU)
Name: Ines Okafor
Bio: Developer advocate at Quillfeather (log search for Postgres). Query plans, bad indexes, slow dashboards. Posts are mine.
Link: quillfeather.dev/blog

Brand (created from the Quillfeather Instagram)
Name: Quillfeather
Bio: Log search for Postgres. Releases and docs. The people who build it post as themselves: @ines.okafor, @dev.marek
Link: docs.quillfeather.dev
```

```
// ✗ Bad - brand only, link-farm bio, no person
Bio: 🚀 #1 AI log platform | #DevOps #Postgres #AI #SaaS | Link in bio 👇
```

## First posts

```
New here from the Postgres side of things. Most of my week is EXPLAIN output and arguing that the slow query is a missing index. Who here runs Postgres in production and posts about it?
Topic: PostgreSQL
```

```
// ✗ Bad - broadcast, link-first, hashtags copied from X
Quillfeather is now on Threads! 🎉 Check out our AI log platform: quillfeather.dev #Postgres #DevOps #AI
```

## Replies

```
Seen that plan shape. The seq scan holds until the table passes a few million rows; a partial index on (status) WHERE status = 'pending' fixed ours. Post the plan if you want a second pair of eyes.
```

```
(I work on Quillfeather, which does log search.) For this one pg_stat_statements gets you most of the way; you only need a tool once you correlate across services.
```

## Original posts with a topic tag

```
Hot take: most "Postgres is slow" tickets I see are one missing index and one ORM default. What's the worst ORM default you've had to undo?
Topic: PostgreSQL
```

```
Why our planner picked a nested loop over a hash join on a 40M-row table, with the full EXPLAIN. Short version: stale stats after a bulk load. Write-up here: quillfeather.dev/blog/nested-loop
Topic: PostgreSQL
```

## Text attachment

```
Post (500 characters max): We cut p95 dashboard latency from 4s to 300ms without new hardware. Three changes, in the order they mattered. Full write-up attached; which one would you have tried first?
Attachment (up to 10,000 characters): the three changes, each with the before/after EXPLAIN and the one-line fix, and one link to the repo.
Topic: PostgreSQL
```

## Community and Dear Algo

```
Ines, in the stack's Community, weekly: one question, one finding, one link at most.
"Weekly Postgres question: what do you check first when autovacuum falls behind?"
```

```
Ines, on her own feed (a reader's choice, not a campaign):
Dear Algo, more posts about Postgres internals and query planning, fewer AI launch threads.
```

```
// ✗ Bad - coordinated brand request
"Help us out! Post 'Dear Algo, show me more Quillfeather' and tag 3 friends 🙏"
```

## Disclosure lines

```
I work at Quillfeather.
Sponsored by Quillfeather; they paid for this post.
Affiliate link: I earn a commission if you sign up.
```

## Cross-post rewrite

```
X original: 6-post thread, "1/6" hook, 3 hashtags, x.com quote, image without alt text.
Threads version: one post under 500 characters with the key finding first, the details in a text attachment, one topic tag, alt text on the chart, no x.com link, no "1/6".
Bluesky version: under 300 characters, link card, alt text, written separately.
Mastodon version: posted from the person's own Mastodon account with the stack's hashtags.
```

## Launch day

```
Person who built it, own account:
I've spent a year on this: Quillfeather now searches 30 days of Postgres logs in under a second on a laptop. Free tier, no card. What I want feedback on is the query syntax: quillfeather.dev
Topic: PostgreSQL

Brand hub: repost Ines, then one release post with the changelog link.
Staff: answer questions in replies with the disclosure line; never like-bomb the post in its first minutes.
```

## Skip verdict

```
Verdict: skip Threads for now.
Why: none of the five Postgres people you named posted there in the last month, and the PostgreSQL tag shows mostly job posts.
Do anyway (10 minutes): claim @quillfeather and project handles, bio "We post on Bluesky and Mastodon: links below."
Re-check: in a quarter, search the tag and the five names again.
```
