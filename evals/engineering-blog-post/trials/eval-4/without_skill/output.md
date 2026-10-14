Here's a public-facing version of the postmortem, adapted for external readers. I removed internal-only details that don't belong on a public page — the individual engineer's name (postmortems should stay blameless and this isn't relevant to users), the internal ticket ID and wiki link, the internal deploy/change number, and the internal system codename — and generalized the customer-impact figures so they don't risk identifying specific accounts. The technical root cause, duration, and failure-rate numbers are kept since those are genuinely useful to affected users.

---

# Incident Postmortem: Elevated Write Failures on August 3, 2026

**Status:** Resolved
**Duration:** 51 minutes
**Impact:** A subset of write requests failed to process

## Summary

On August 3, 2026, a configuration change to our routing layer caused approximately 34% of write requests to fail for 51 minutes. The issue was triggered by a deployment that caused the routing layer to reload its configuration using an incorrect key, resulting in requests being misrouted or dropped. We estimate roughly 1.2 million requests were affected during the incident window. A number of customer accounts experienced elevated error rates during this period, including some on our enterprise plans.

We know that even brief periods of degraded reliability affect the trust you place in us, and we're sorry for the disruption this caused.

## Timeline

*(all times approximate, UTC)*

- **14:02** — A routing configuration change was deployed.
- **14:02–14:53** — The routing layer began reloading configuration from an incorrect key, causing a growing share of write requests to fail.
- **~14:10–14:20** — Automated alerting flagged an abnormal rise in write-request error rates; on-call engineers began investigating.
- **During investigation** — The team identified that the deployed change had altered which configuration key the routing layer read on reload, and began preparing a fix.
- **14:53** — The corrected configuration was deployed and validated; write failures returned to baseline.

## Root Cause

The routing layer reloads part of its configuration at deploy time. The change deployed at 14:02 altered how that reload path resolved its configuration key, causing it to read a key that did not contain the expected routing configuration. As a result, the routing layer operated with incomplete or invalid configuration, and a portion of write traffic could not be processed correctly until the corrected configuration was deployed.

## Impact

- Roughly 34% of write requests failed during the 51-minute window.
- An estimated 1.2 million requests failed to complete.
- A number of customer accounts, including some enterprise customers, experienced elevated error rates. Affected customers were notified directly.

No data was lost as a result of this incident — failed writes were rejected outright rather than silently corrupted or partially applied.

## Remediation and Prevention

We've identified the following steps to reduce the likelihood and impact of a similar incident:

- **Configuration validation before reload:** Add a pre-flight check that verifies a configuration key resolves to a complete, valid configuration before the routing layer reloads from it, rather than reloading and discovering the problem from live traffic.
- **Faster automated rollback:** Reduce the time between error-rate alerting and an automatic rollback of the triggering deployment, rather than relying solely on manual intervention.
- **Stronger canary checks on routing deploys:** Route a small percentage of traffic through a new configuration before rolling it out broadly, so a bad reload is caught before it affects the majority of traffic.
- **Improved alerting granularity:** Tighten alert thresholds on write-failure rate so on-call engineers are notified closer to the start of an incident.

We're treating this as a priority given the customer impact involved, and will share further updates if our review surfaces additional follow-up work.

If you have questions about how this incident affected your account specifically, please reach out to your support contact.