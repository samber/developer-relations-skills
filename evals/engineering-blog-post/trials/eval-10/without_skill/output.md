# Readout: Storage-Rewrite Post — Pipeline & Read-Through Target

## 1. Pipeline generated

Don't hand her a single "this post generated $X" number. No model can honestly assign 100% credit for a deal to one blog post in a multi-touch B2B journey — that number gets challenged the first time someone asks how it was calculated, and it doesn't survive the question.

Give her two numbers instead, both with a stated lookback window:

- **First-touch-influenced pipeline** — deals where this post's URL is the very first tracked touch.
- **Any-touch-influenced pipeline** — deals where this post appears anywhere in the touch path before deal creation, within a 90-day lookback.

The gap between the two is itself the insight: wide gap = strong top-of-funnel driver, not a closer. State plainly that any-touch numbers aren't additive across your content calendar — the same pipeline dollar will legitimately show up "influenced" by several posts, so don't let her multiply this figure across every post later.

Fill-in template — plug your CRM/analytics export in:

| Metric | Value | Source |
|---|---|---|
| Pageviews (last month) | 22,000 | GA/PostHog |
| Sessions attributable to this post (UTM/referrer) | ? | GA/PostHog |
| Tracked conversions (signup, demo request) first-touched to this post | ? | CRM, `$initial_utm_*` or lead-source field |
| Deals w/ this post anywhere in touch path (90-day lookback) | ? | CRM |
| First-touch-influenced pipeline $ | ? | CRM, summed deal amount |
| Any-touch-influenced pipeline $ | ? | CRM, summed deal amount |

If UTM tagging or CRM first-touch fields aren't wired up yet, say that instead of guessing a number: "22K pageviews, X identifiable sessions, Y tracked conversions, here's what's in CRM tied to this landing page — full pipeline attribution needs [identify-at-conversion / UTM-to-CRM sync] closed first." That's a stronger position than a fabricated figure.

## 2. Read-through rate — no real industry benchmark exists, tell her that

"Read-through rate" is a defined metric in **email** marketing (opens who click through to read the full piece). It doesn't transfer cleanly to a blog post reached via search/social/direct — there is no standardized, cross-industry benchmark for blog read-through.

Any number floating around ("aim for 40-45%") almost always traces to one vendor's blog post (Chartbeat, Contentsquare, Medium) reporting on their own customer base, with an undisclosed definition of "read-through" — full scroll, time-on-page, or scroll-past-fold, inconsistently. None of it is peer-reviewed, none of it is dev/technical-audience-specific, and a storage-rewrite post read mostly by engineers won't behave like a media site's average article.

Recommend instead:

1. Instrument scroll-depth / active-read-time now if not already tracked (GA4 scroll events, or a custom "article complete" event at ~90% scroll).
2. Measure this post's own read-through this month as your baseline.
3. Set next quarter's target as a delta off that baseline ("our best post reads through at 38%, target the next tier at 30%+") — defensible and actionable, unlike importing a number sourced from a different audience and platform.
4. If she wants an external anchor point regardless, the honest one is: no credible industry benchmark exists for this metric; internal baseline is the only trustworthy input for target-setting here.

## What to say in the room

- Pipeline: influenced-pipeline range (first-touch vs. any-touch), lookback window stated, not a single causal figure — flag now if instrumentation to compute it cleanly doesn't exist yet.
- Read-through: there's no real industry benchmark to cite — say so directly — and propose setting next quarter's target off this post's own measured baseline instead.