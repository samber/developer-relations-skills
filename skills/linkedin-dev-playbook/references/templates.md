# LinkedIn templates

Examples for a fictional company, Tidewire (schema registry for Kafka), its founder Lena, its advocate Ana and its engineer Tomasz. Adapt every line to the person posting and have them rewrite it in their own voice; never post these as written.

## Contents

- Headlines and About
- Comments
- Replies on your own post
- Technical post
- Document post outline
- Launch-day post
- Page reshare and release post
- Newsletter edition
- Connection invitation note
- Employee brief
- Disclosure lines
- Event invitation
- Group post

## Headlines and About

```
Tomasz Wrobel
Headline: Staff engineer at Tidewire - schema evolution and compatibility for Kafka
About (first lines): I build the compatibility checker at Tidewire. I write about breaking changes in event schemas and how teams catch them before production. Posts are my own.

Ana Ribeiro
Headline: Developer advocate at Tidewire | Kafka, Avro, Protobuf in production
```

```
// ✗ Bad - no topic, no employer, nothing the ranker or a buyer can match
Headline: Passionate technologist | Innovation | Growth mindset | Let's connect!
```

## Comments

On a platform lead's post about a failed migration:

```
We hit the same wall moving from Avro to Protobuf: field 7 got renumbered and every consumer on the old version read garbage for 40 minutes. What saved the second attempt was running the compatibility check in CI against the last three registered versions, not only the latest. Did you keep both formats live during the cutover?
```

With disclosure, when the product is relevant:

```
(I work at Tidewire, which builds a schema registry.) For your size, the open-source registry plus a CI check covers most of this; a hosted one starts paying off once several teams own topics.
```

```
// ✗ Bad - adds nothing, reads as a pod or a pitch
Great insights! 🔥 Totally agree. We solve exactly this at Tidewire, DM me for a demo.
```

## Replies on your own post

```
Good question. We measured it on 1,200 topics: 31 had at least one incompatible change in six months, and 26 of those were caught only because consumers ran on older versions. Happy to share the query.
```

## Technical post

```
We found 31 breaking schema changes in 1,200 Kafka topics over six months. (I work at Tidewire; we build schema tooling, and this is from our own registry.)

Twenty-six of them passed every check the producer team ran, because they only tested against the latest schema version. Consumers were three versions behind.

What changed our numbers:
- check compatibility against every version still read in production, not the latest
- block field renumbering in Protobuf at review time
- keep a dashboard of the oldest schema version each consumer group reads

The full method and the query are in the write-up (link in the first comment).

How far behind are your slowest consumers?
```

The disclosure sits in the first lines, before "see more"; the headline alone does not count. The closing question asks for information the author can use; it does not ask for a reaction.

```
// ✗ Bad - bait hook, no substance, link-first, hashtag stack
Unpopular opinion: your schemas are broken. 👀
Agree? Comment SCHEMA and I'll DM you our free checklist!
tidewire.io/checklist
#kafka #data #engineering #devops #ai #startup #tech #innovation
```

## Document post outline

```
Title slide: "Breaking schema changes: 31 found in 1,200 topics"
2: What counts as breaking (one table)
3: Why latest-only checks miss them (one diagram)
4-6: The three changes, one per slide, each with the before and after
7: The query (code, readable at phone size)
8: Who wrote it, and where the full write-up lives
Caption: two sentences with the finding, not "swipe to learn more"
```

## Launch-day post

```
From Lena's account:
For two years, our compatibility checker only ran inside Tidewire. As of today it is open source and runs in any CI, with or without our registry.

It checks a new schema against every version your consumers still read, which is where 26 of the 31 breaks we measured came from.

What I'd like feedback on: the rule format. Link in the first comment.
```

## Page reshare and release post

```
Repost as the Page, with thoughts:
Tomasz wrote up how the checker decides what "still read in production" means. Worth it if you run more than one consumer version.
```

```
Release post on the Page:
Tidewire 3.4: Protobuf field-number guard, faster checks on large registries (12 s to 2 s on 5,000 subjects), and a fix for nested union types. Changelog in the comments.
```

## Newsletter edition

```
Title: Schema notes, #4 - the consumer lag nobody tracks
Opening: I build schema tooling at Tidewire. This month: why the oldest schema a consumer reads matters more than the newest one a producer writes, a query to find it, and one reader's migration story.
Body: teach the technique fully; no product pitch inside the edition.
Last line: "The checker from section 2 is open source: [link]." One product link, as context, not an offer.
```

## Connection invitation note

Only to someone you have talked with or met. For someone whose posts you only want to read, Follow instead of Connect:

```
Hi Priya, we spoke after your talk on event contracts at the Kafka meetup. Your point about consumer-owned schemas stuck with me; I'd like to compare notes on how you handle retired versions.
```

```
// ✗ Bad - a pitch in an invitation to a stranger
Hi Priya, I help platform teams like yours cut schema incidents by 80%. Open to a quick 15-min call this week?
```

## Employee brief

```
Subject: Checker goes open source on Tuesday - optional

What ships: the compatibility checker, Apache-2.0, runs in any CI.
The hard problem: deciding which versions are "still read" without consumer instrumentation.
Numbers you can quote: 31 breaks in 1,200 topics; 26 missed by latest-only checks.
Who built it: Tomasz, Mei, Jonas.
Lena posts at 10:00 CET; the Page reshares it.

If you want to post, write it from your own work: what you saw, what you built, what surprised you. Say you work here in your first lines, not only in your headline. Skip it if it isn't your topic. Please don't copy Lena's text, and there's no need to like or comment on anyone's post.
```

## Disclosure lines

```
I work at Tidewire.
Tidewire sent me early access for free; I'm writing about it because I used it.
Paid partnership with Tidewire. (Plus the brand partnership label in post settings.)
```

## Event invitation

```
Event description: Live walkthrough: catching breaking schema changes in CI. 45 minutes, Q&A for half of it, recording afterwards. Hosted by Ana Ribeiro and Tomasz Wrobel.
Invitation (to connections who work with Kafka): No slides about Tidewire; we'll run the checks on a public repo and take your schemas as examples.
```

## Group post

Read the group's rules first; post only where promotion is allowed or the content stands without the product.

```
Question for people running Kafka with several consumer versions: how do you decide when an old schema version can be retired? We track the oldest version each consumer group reads, but I'm not sure that catches batch jobs that run monthly.
```
