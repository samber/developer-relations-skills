# Mastodon templates

Examples for a fictional company, Tidewell (feature flags for Go services), and its advocate Marta Lindqvist. Adapt every line to the person posting and rewrite it in their own voice; never post these as written.

## Contents

- Profiles
- Verification snippets
- Hachyderm approval request
- #introduction post
- Replies under a hashtag
- Original teaching post
- Release post and status post
- Launch day
- Bot account
- Quote posts
- Cross-post rewrite
- Disclosure lines

## Profiles

```
Account: @marta@hachyderm.io
Display name: Marta Lindqvist
Bio: Developer advocate at Tidewell (feature flags for Go). I write about safe rollouts, flaky tests and Go tooling. Posts are mine.
Fields:
  Website  https://tidewell.dev/team/marta   (verified with rel=me)
  Employer Tidewell
  Pronouns she/her

Account: @tidewell@hachyderm.io  (corporate tier, approved)
Display name: Tidewell
Bio: Feature flags for Go services. Releases, security advisories, status. The people who build it: @marta@hachyderm.io @jonas@fosstodon.org
Fields:
  Website  https://tidewell.dev              (verified with rel=me)
  Docs     https://tidewell.dev/docs
  APPROVED https://community.hachyderm.io/approved
```

```
// ✗ Bad - brand on a server that bans promotion, no humans named, no verification
Account: @tidewell@fosstodon.org
Bio: 🚀 The #1 feature flag platform! #DevOps #Go #Golang #FeatureFlags #SaaS #Startup #Cloud
```

## Verification snippets

```html
<!-- On https://tidewell.dev/team/marta, in raw HTML, not inside an iframe -->
<a href="https://hachyderm.io/@marta" rel="me">Marta on Mastodon</a>

<!-- In the head of each blog post Marta writes -->
<meta name="fediverse:creator" content="@marta@hachyderm.io" />
```

Then in Marta's account: add `tidewell.dev` under Verification > Author attribution, and save the profile after the page is live.

## Hachyderm approval request

```
Subject: Specialized account approval - corporate - @tidewell

Company: Tidewell (12 employees), feature flags for Go services, commercial with a free tier.
Account: @tidewell@hachyderm.io (we will post nothing as Tidewell until you approve).
Purpose: release notes, security advisories, outage status. People accounts do the conversation.
Automation: one release post per release from this account, under 5 automated or scheduled posts a day. No separate bot account.
We have read and agree to the Specialized Account Expectations, including Say It Once.
Contact: marta@tidewell.dev
```

## #introduction post

```
Hi, I'm Marta. #Introduction

I work on Go tooling and safe rollouts at Tidewell (feature flags). Before that, eight years running deploys for a payments team, so I collect stories about the rollback that did not roll back.

Here for #Golang, #DevOps, #SRE talk, and bad-diagram appreciation.
```

Pin it. One per person; the brand account's introduction names the humans behind it.

## Replies under a hashtag

```
// ✓ Good - answers the question, no product link
@sam@fosstodon.org the data race shows up because the test reads the flag map while the refresher writes it. Wrapping reads in an atomic.Pointer swap fixed it for us; go test -race catches it in about a second.

// ✓ Good - product mentioned only because they asked for a tool, with disclosure
@lea@hachyderm.io if you want a library rather than rolling it yourself: we build Tidewell (I work there), open-source Go SDK, https://github.com/tidewell/go-sdk. Unleash and Flagd also do this.

// ✗ Bad - drive-by link
@sam@fosstodon.org Tidewell solves this! https://tidewell.dev?utm_source=mastodon
```

## Original teaching post

```
A feature flag check inside a hot loop cost us 9% CPU last month. Three things fixed it:

1. Evaluate once per request, not per item
2. Cache the evaluated value in the request context
3. Benchmark with -count=10 before believing any number

Write-up with the flame graphs: https://tidewell.dev/blog/flag-hot-loop?utm_source=mastodon&utm_medium=social

#Golang #Performance
```

Alt text for the attached flame graph:

```
Flame graph of a Go service. The widest frame, 9% of samples, is flag.Evaluate called from a loop in handler.ServeHTTP. After the fix, flag.Evaluate is under 0.1%.
```

## Release post and status post

```
Tidewell Go SDK v2.4.0 is out.

- Context-aware evaluation, no per-call allocations
- Fixes a race in the refresher (thanks @sam@fosstodon.org for the report)

Changelog: https://tidewell.dev/changelog/v2-4-0?utm_source=mastodon&utm_medium=social
#Golang
```

```
CW: Incident post-mortem, 41 minutes of stale flags on 2026-10-03

[body: timeline, cause, fix, what changes]
```

Post each release once. Never repost it on a timer.

## Launch day

```
Marta (person who built it):
We shipped flag rollouts that pause themselves when error rates climb. I spent four months on the part that decides "climb". Here is how it works, with the false positives we hit: [link with UTM]
#Golang #SRE #DevOps

Brand account: boost Marta's post once. No separate copy.
Staff: answer questions in replies; no mass favourites.
```

## Bot account

On Hachyderm, skip the bot: the approved corporate account posts each release itself, within 5 automated or scheduled posts a day, and a project may not run a release-only bot there. Elsewhere, a separate bot on a server whose rules admit bots:

```
Account: @tidewell_releases@mastodon.social
Flag: Automated (bot)
Bio: Release feed for the Tidewell Go SDK, from our GitHub releases. Not monitored; talk to @marta@hachyderm.io. Operator: Marta Lindqvist.
Visibility: Quiet public. One post per release. Never replies to keywords.
```

## Quote posts

```
// ✓ Good - quote adds something, credits the author
Quoting @jonas@fosstodon.org: this is the clearest explanation of percentage rollouts I have read. One addition from our side: hash on a stable user ID, never the session, or users flip between variants.

// ✗ Bad - quote-dunk a critic
"Feature flags are tech debt with a UI" lol, someone hasn't used Tidewell 🙃
```

Expect the author to revoke any quote they dislike; never quote to argue.

## Cross-post rewrite

```
Source (X): Big news 🚀 Tidewell v2.4 is LIVE! Faster, safer, better. 👇 x.com/tidewell/status/123 #featureflags

Mastodon: Tidewell Go SDK v2.4.0: evaluation no longer allocates, and a refresher race is fixed. Changelog: https://tidewell.dev/changelog/v2-4-0?utm_source=mastodon&utm_medium=social #Golang
```

Rules: plain wording, no x.com links, CamelCase tags in the body, alt text on media, a person reads replies.

## Disclosure lines

```
Bio: Developer advocate at Tidewell. Posts are mine.
In a reply: (I work at Tidewell.)
Affiliate link on Hachyderm: ... #AffiliateLinks
AI-assisted image: Image generated with an AI tool.
```
