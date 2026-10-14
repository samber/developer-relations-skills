# Sablefish Health — Docs CODEOWNERS + Sample-Fix SLA

## The blocker you need to solve before Friday, not after

GitHub CODEOWNERS routes by **file path**, not by content inside a file. With samples hand-pasted into Markdown and no snippet tags, most of your ~900 pages almost certainly mix multiple languages in one file (tabbed code blocks on a single API-reference page). CODEOWNERS cannot tell "the Python tab changed" from "the Java tab changed" — it will request the same reviewer(s) for the whole file regardless of which language's block a PR touches.

Two places this genuinely works today, and one place it doesn't:

- **Per-language quickstart/getting-started pages** — these are almost always already one file per language. True language-level ownership works here now.
- **Shared API-reference / guide pages with tabbed samples** — one owner per page is the only thing CODEOWNERS can enforce today. Routing a Python-only change to the Python owner is not possible until samples are split into per-language include files (the actual fix for "no snippet tags").
- **Anything without a clean directory/naming convention at all** — falls to a fallback team below; don't leave any path unowned, GitHub silently skips unmatched files.

Ship the interim file below by Friday. Open a follow-up ticket for snippet extraction — that's the only path to true per-language enforcement and automation later.

## CODEOWNERS

Replace every `@handle`/`@org/team` placeholder with real GitHub usernames or teams. Every named owner must have write access to the repo or GitHub silently drops them from the review request.

```
# Sablefish Health docs — sample ownership
# Rules are evaluated last-match-wins: keep broad fallbacks first, specific paths last.

# ---- Fallback: anything not matched below ----
*                                           @sablefish-docs/core-writers

# ---- Per-language quickstarts (true language-level ownership) ----
/quickstart/python/**                      @sablefish-sdk/python-owner
/quickstart/node/**                        @sablefish-sdk/node-owner
/quickstart/java/**                        @sablefish-sdk/java-owner       # unpaid outside contributor — see SLA notes
/quickstart/ruby/**                        @sablefish-sdk/ruby-owner       # unpaid outside contributor — see SLA notes

# ---- SDK reference guides that are already single-language files ----
/reference/python/**                       @sablefish-sdk/python-owner
/reference/node/**                         @sablefish-sdk/node-owner
/reference/java/**                         @sablefish-sdk/java-owner
/reference/ruby/**                         @sablefish-sdk/ruby-owner

# ---- Mixed-language tabbed pages (API reference, guides): page-level only ----
# Cannot route by language until samples are split into per-language snippet files.
/api-reference/**/*.md                     @sablefish-docs/content-owner
/guides/**/*.md                            @sablefish-docs/content-owner

# ---- CI / secrets scanning config, once added ----
/.github/workflows/secrets-scan.yml        @sablefish-security/docs-liaison
```

## Sample-Fix SLA

| Language / SDK | Owner | Employment status | Sample fix SLA | Escalation on breach | Notes |
|---|---|---|---|---|---|
| Python | `@sablefish-sdk/python-owner` | Staff (paid) | 48h business hours: fix or pull the sample | Auto-escalate to eng manager at 48h | Full SLA as directed |
| Node.js | `@sablefish-sdk/node-owner` | Staff (paid) | 48h business hours: fix or pull the sample | Auto-escalate to eng manager at 48h | Full SLA as directed |
| Java | `@sablefish-sdk/java-owner` | Unpaid outside contributor | Best-effort, target 5 business days | At 48h with no response, `@sablefish-docs/content-owner` marks the sample "unverified" and pulls it if broken | No SLA on a volunteer — see below |
| Ruby | `@sablefish-sdk/ruby-owner` | Unpaid outside contributor | Best-effort, target 5 business days | At 48h with no response, `@sablefish-docs/content-owner` marks the sample "unverified" and pulls it if broken | No SLA on a volunteer — see below |

**"Fix" for all four rows means one of two things within the window: correct the sample, or flag it `unverified`/remove it.** Nothing here is tested today, so "confirm it actually still runs" can't be a 48h promise on day one — treat the first pass through each sample as triage, not verification.

**Don't put a 48-hour SLA on the two unpaid maintainers.** There's no employment relationship to enforce it, and a hard clock on volunteer labor is a fast way to lose them. Give the VP the paid-vs-volunteer split above as-is: two SDKs get the real 48h SLA; the other two get a staff-owned fallback that keeps published docs honest (pull or flag, don't silently leave a broken sample live) without making the SLA a demand on someone unpaid.

## Before this merges

- **Run a secrets scan (gitleaks or trufflehog) across all ~900 pages now, not after.** You're about to put named owners' names next to content that's never been checked — if a live key surfaces in a page owned by an unpaid volunteer, that's a problem you want to have found yourself.
- **Add every CODEOWNERS handle as a repo collaborator/team member first** — an owner without write access is silently dropped from review requests, and the SLA table becomes fiction.
- **File the snippet-tagging + per-language sample extraction ticket alongside this merge.** It's the only way "every language's samples" becomes literally true instead of "every language's samples, except the ~90% living in shared tabbed pages."