Two things up front, both load-bearing for what follows, not caveats to skip:

**This is the exact sequencing mistake the governance model warns against.** Samples are hand-pasted Markdown — no snippet tags, no metadata, nothing compiled or tested. CODEOWNERS assigns a name to fix a red sample, but nothing here can turn red: there's no build, no compile check, no test. Merging this Friday buys you *pull-request review gating* (real, immediate) but not *breakage detection* (not yet possible). Say that explicitly to the VP rather than let the file imply more than it does.

**A secret scan cannot wait for CODEOWNERS.** "Never scanned for secrets" across ~900 hand-pasted pages, at a health company, is the one finding class that outranks everything else in this whole exercise. Run it in parallel this week, not after the file merges — a P0 credential leak doesn't wait for org process.

Both deliverables below, ready to use.

---

## 1. `.github/CODEOWNERS`

```
# CODEOWNERS — documentation code samples
# Requires: repo setting "Require review from Code Owners" enabled on the
# default branch, or this file approves nothing and blocks nothing.
#
# ASSUMPTION — confirm before merging: samples live under a per-language
# directory or file-suffix convention (/docs/<language>/..., *.py.md, etc).
# If a page mixes languages in tabs on one file, CODEOWNERS cannot attribute
# a single broken sample to one owner — the whole page falls to joint
# ownership (docs-platform-owners) until pages are split or tagged.
#
# Fix-SLA table lives in docs/SAMPLE-OWNERSHIP.md — this file is the merge
# gate, that file is the clock.
#
# Replace every <GH_HANDLE_*> and <LANGUAGE_*> below with real values before
# merging. Nothing here is enforceable with a placeholder in it.

# ---- <LANGUAGE_1>, e.g. Python — internal, paid engineering ----
/docs/<language_1>/                      @sablefish-health/<language_1>-sdk-team
/docs/**/examples/<language_1>/          @sablefish-health/<language_1>-sdk-team
/docs/**/*.<language_1>.md               @sablefish-health/<language_1>-sdk-team

# ---- <LANGUAGE_2>, e.g. JavaScript/TypeScript — internal, paid engineering ----
/docs/<language_2>/                      @sablefish-health/<language_2>-sdk-team
/docs/**/examples/<language_2>/          @sablefish-health/<language_2>-sdk-team
/docs/**/*.<language_2>.md               @sablefish-health/<language_2>-sdk-team

# ---- <LANGUAGE_3> — OUTSIDE, UNPAID contributor(s) ----
# Named individual required as accountable owner, per SLA table note below.
# The community maintainer is CC'd for context, not held to the SLA clock.
/docs/<language_3>/                      @<GH_HANDLE_internal_backstop_1> @<GH_HANDLE_community_maintainer_1>
/docs/**/examples/<language_3>/          @<GH_HANDLE_internal_backstop_1> @<GH_HANDLE_community_maintainer_1>
/docs/**/*.<language_3>.md               @<GH_HANDLE_internal_backstop_1> @<GH_HANDLE_community_maintainer_1>

# ---- <LANGUAGE_4> — OUTSIDE, UNPAID contributor(s) ----
/docs/<language_4>/                      @<GH_HANDLE_internal_backstop_2> @<GH_HANDLE_community_maintainer_2>
/docs/**/examples/<language_4>/          @<GH_HANDLE_internal_backstop_2> @<GH_HANDLE_community_maintainer_2>
/docs/**/*.<language_4>.md               @<GH_HANDLE_internal_backstop_2> @<GH_HANDLE_community_maintainer_2>

# ---- Cross-cutting: shared auth setup, scripts, future metadata ----
/docs/_shared/auth-setup.md              @sablefish-health/docs-platform-owners
/scripts/                                @sablefish-health/docs-platform-owners
/sample-metadata/                        @sablefish-health/docs-platform-owners

# ---- Fallback for anything unmatched above ----
/docs/                                   @sablefish-health/docs-platform-owners
```

Why an internal name sits first on the two unpaid languages: a 48-hour SLA is a demand on someone's paid time. An unpaid outside contributor cannot be held to it — Twilio's own community sample catalogue was backstopped by *contracted, paid* engineers, not run on volunteer labor alone. Put a named, paid engineer as the accountable owner on `<LANGUAGE_3>` and `<LANGUAGE_4>`, with the community maintainer as co-reviewer. If no paid backstop is fundable, say so to the VP now and demote those two SDKs to a labeled "Community" tier — present, unowned, no SLA — rather than publish an SLA you already know will breach.

---

## 2. Fix SLA

### 2a. What was asked — flat 48-hour SLA per owner

| Language | Tier | Owner of record | Backup | Fix SLA |
|---|---|---|---|---|
| `<LANGUAGE_1>` | Internal | `<GH_HANDLE_1>` | `@sablefish-health/<language_1>-sdk-team` | 48h |
| `<LANGUAGE_2>` | Internal | `<GH_HANDLE_2>` | `@sablefish-health/<language_2>-sdk-team` | 48h |
| `<LANGUAGE_3>` | Community, unpaid | `<GH_HANDLE_internal_backstop_1>` (paid) | `<GH_HANDLE_community_maintainer_1>` (best-effort, not bound) | 48h *(owner of record only — see 2b)* |
| `<LANGUAGE_4>` | Community, unpaid | `<GH_HANDLE_internal_backstop_2>` (paid) | `<GH_HANDLE_community_maintainer_2>` (best-effort, not bound) | 48h *(owner of record only — see 2b)* |

### 2b. Recommended refinement — SLA by severity, not one flat number

A flat 48-hour clock either overpays trivial fixes or underpays a live credential. Split it:

| Severity | Definition | SLA | Rationale |
|---|---|---|---|
| P0 | Reader-harming: leaked credential, destructive command, disabled TLS, insecure default | 4 business hours, any owner | Security/credential fixes outrank everything else — cannot wait for the normal queue |
| P1 | Broken as written: compile error, removed symbol, wrong output | 48 hours on primary adoption path (quickstart/auth/first call); 5 business days elsewhere | Matches the VP's number where it matters most; relaxes it where a reader is less likely to hit it first |
| P2 | Works but violates policy: no tier, missing expected output, parity gap | Next sprint, batched | Not urgent; batching avoids owner fatigue on low-severity noise |

Escalation: no owner action within SLA → auto-escalate to backup → unresolved 24h past backup's window → docs-platform-owners takes the fix directly rather than re-escalating again.

### 2c. What starts the clock today

No CI exists yet, so nothing currently *detects* a red sample automatically. Until a compile/build gate exists, the SLA clock starts on: a filed GitHub issue, a support ticket quoting the sample, or a manual reviewer catch on PR. Say this to the VP directly — an SLA with no automatic trigger is a promise about response time to *reported* breaks, not a guarantee nothing is broken. Closing that gap is the next investment (move samples into compilable source + snippet tags + a compile-only CI gate), and it's what turns this SLA from aspirational into enforceable.