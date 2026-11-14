# Reddit templates

Examples for a fictional project, Ferrule (an MIT-licensed Go CLI that lints Postgres schema migrations, with a paid hosted dashboard), and its maintainer Dana. Each is structure for the named person to rewrite in their own words; in subreddits that ban AI-written text, the final wording must be theirs.

## Contents

- Profiles and disclosure
- Answers in existing threads
- Showcase-thread comment
- Project posts by lane
- Technical write-up for a ban-regime subreddit
- Modmail
- AMA
- Launch-day replies
- What gets removed

## Profiles and disclosure

```
u/dana_okoye (personal, years of history)
Bio: Postgres and Go. I maintain Ferrule, a migration linter. Opinions mine.

u/ferrule_hq (brand, Reddit Pro, verified business profile)
Bio: Official account of Ferrule. Release notes and support. Maintainer: u/dana_okoye.
```

Disclosure lines, first line of the post or first sentence of the comment:

```
I maintain Ferrule, so I'm biased.
Disclosure: I work at Ferrule (we sell the hosted dashboard; the CLI is MIT).
```

## Answers in existing threads

No product needed:

```
The lock comes from ALTER TABLE ... ADD COLUMN with a volatile default: Postgres rewrites the table. Add the column nullable, backfill in batches of ~10k rows, then set the default and NOT NULL in a separate migration. On 11+ a constant default is metadata-only, so check which case you're in with \d+ before and after.
```

Product answers the question, disclosed, alternatives named:

```
I maintain Ferrule, so weigh this accordingly. This is the exact check it does (flags table-rewriting ALTERs in CI). If you'd rather not add a tool, squawk and strong_migrations cover the same case; even a grep for "ADD COLUMN .* DEFAULT" in CI catches most of it.
```

## Showcase-thread comment

```
Ferrule - lints Postgres migrations for locking and rewrite hazards before they hit prod. Go, MIT, single binary.
What's new: catches ADD COLUMN with volatile defaults and missing CONCURRENTLY on index builds.
Looking for: false positives on your real migration folders.
Repo: [link]
(I'm the maintainer.)
```

Drop the "Looking for" line in r/dataengineering, which bans "looking for feedback" posts.

## Project posts by lane

r/golang (open-source Go tool, read the linked project-post rules first):

```
Title: Ferrule: a migration linter that parses SQL with the Postgres parser via cgo-free bindings

I maintain this. Problem: our migrations kept taking ACCESS EXCLUSIVE locks in prod. Ferrule parses each migration with a Go port of the Postgres grammar and flags 14 lock and rewrite hazards.

Design choices I'd like feedback on:
- no cgo, so the parser is a generated Go port; ~3x slower than libpg_query, fine for CI
- rules are plain Go funcs over the AST, no DSL

Repo, MIT: [link]. A paid hosted dashboard exists; nothing here needs it.
```

r/Python showcase shape (text post, showcase flair, all three sections):

```
What My Project Does: ...
Target Audience: production teams / toy project / ...
Comparison: how it differs from the two closest tools, honestly
Source: [repo link]
```

r/selfhosted, project under three months old: a comment in the current weekly New Project Megathread only, with screenshots, a docker-compose snippet and a docs link.

## Technical write-up for a ban-regime subreddit

r/programming accepts link posts about how something was built, never "I made this":

```
Title: Why ADD COLUMN DEFAULT now() rewrites your whole table, and how we detect it statically
Link: the blog post (the post must teach without the product; the tool appears once, at the end)
```

## Modmail

Never send these where a pinned announcement bans exception requests (r/opensource).

Rule clarification, before posting:

```
Hi mods, I maintain Ferrule (MIT Go CLI, paid hosted add-on). Is a project post OK here if it focuses on the parser design, or should it go in the weekly thread? Happy to do either.
```

AMA proposal:

```
Hi mods, would r/PostgreSQL be open to an AMA with Dana Okoye (Ferrule maintainer) and Lee Park (Postgres contributor, locking internals)? Topic: zero-downtime schema changes. Proposed: Tuesday 17:00 UTC, two hours. We'd answer everything, product questions included, and can verify identity however you prefer.
```

After a removal:

```
Hi mods, my post "[title]" was removed. Which rule did it break? I'd like to fix it rather than repost. Thanks.
```

## AMA

```
Title: We maintain Ferrule and work on Postgres locking. Ask us anything about zero-downtime migrations.
Body: who each host is, one proof line or photo, what you'll answer, when you'll stop, co-hosts listed.
```

## Launch-day replies

Criticism with a point:

```
Fair. The default rule set is noisy on partitioned tables; that's #212. Until it lands, `--skip partition-rewrite` silences it. What would you expect it to flag instead?
```

Competitor thread:

```
(I work on Ferrule, a competitor.) For your setup the other tool is the better fit because it hooks into Rails migrations directly; we only read raw SQL.
```

## What gets removed

```
// ✗ Bad - product-name title, link only, posted on a Tuesday to r/webdev
Ferrule.dev 🚀

// ✗ Bad - undisclosed, posted from a two-day-old account
"Has anyone tried Ferrule? Heard it's amazing for migrations!"

// ✗ Bad - same text to r/golang, r/PostgreSQL, r/devops, r/programming within the hour

// ✗ Bad - team chat
"Launch post is up, everyone go upvote and comment!"
```
