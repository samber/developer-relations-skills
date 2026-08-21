# Developer relations skills

Skills for the people who run **developer relations**: documentation, open source, community, events, technical content, and the program strategy and measurement holding it together.

Written for **developer advocates, DevRel leads, community managers and OSS maintainers**. Every skill ends in **a decision or a shippable artifact**, never a list of best practices.

## 📚 Related Collections

- [`developer-platform-skills`](https://github.com/samber/developer-platform-skills): Platform & SDK developer experience: _for platform engineers, DX engineers, SDK authors, API product managers, DevRel engineers_
- [`dev-event-organizer-skills`](https://github.com/samber/dev-event-organizer-skills): Technical event operations: _for event organizers, conference producers, hackathon leads, community builders_

_Part of the [samber skills ecosystem](https://github.com/samber?tab=repositories&q=skills)_

## Install

Install every skill in this repo, not just one. Skills here are atomic by design and reference each other freely: picking a single skill leaves its sibling skills uninstalled, so cross-references and routed handoffs go nowhere.

**skills.sh (universal)**: works with any Agent Skills-compatible tool:

```bash
npx skills add samber/developer-relations-skills
```

**Claude.ai**:

1. add as a plugin marketplace: open **Settings -> Capabilities -> Plugins**
2. click **Add -> Add marketplace -> Add from a repository**
3. enter `samber/developer-relations-skills`
4. then **Sync**

**Claude Code**: install the plugin:

```bash
/plugin marketplace add samber/cc
/plugin install developer-relations-skills@samber
```

**Codex (OpenAI)**: install via the Codex CLI:

```bash
codex plugin add github:samber/developer-relations-skills
```

**Cursor**: copy into Cursor's skills directory:

```bash
git clone https://github.com/samber/developer-relations-skills.git ~/.cursor/skills/developer-relations-skills
```

Cursor auto-discovers skills from `.agents/skills/` and `.cursor/skills/`.

**Gemini CLI**: install as a Gemini extension:

```bash
gemini extensions install https://github.com/samber/developer-relations-skills
```

Update with `gemini extensions update developer-relations-skills`.

## 📦 Skills

This collection covers the full developer-relations surface. Start here:

- [`developer-relations-kickoff`](./developer-relations-kickoff): Routes the current DevRel task to the right skill of this collection, or says plainly that none fits, then bootstraps or resumes the project's shared context artifact.
- [`devrel-career`](./devrel-career): Plans a DevRel career from the candidate side: portfolio audit against real hiring signals, the six-rung IC ladder, interview-loop prep, and offer evaluation.
- [`devrel-hiring`](./devrel-hiring): Plans DevRel hiring from the employer side: job posting and scorecard, interview loop and question bank, portfolio scoring, and a 30-60-90 ramp plan.
- [`devrel-team-structure`](./devrel-team-structure): Designs the DevRel org: the reporting line and what it starves, the team shape, the coverage map, the interlocks, and the trigger for the next re-org.
- [`devrel-radar`](./devrel-radar): Builds a time-budgeted watch list of DevRel podcasts, newsletters, communities, conferences and practitioners, plus the routine that keeps it verified and fresh.

### Strategy & planning

| Skill                                                            | Description                                                                                                                                                                    |
| ---------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| [`devrel-strategy`](./devrel-strategy)                           | Designs a DevRel program from the top: the business driver that funds it, the two goals it serves, the pillar mix, the staffing sequence, and a written refused list.          |
| [`developer-first-gtm`](./developer-first-gtm)                   | Designs the go-to-market motion for a developer product: the adoption model, the self-serve entry, the developer-to-buyer handoff rule, and the land-and-expand path.          |
| [`developer-ecosystem-strategy`](./developer-ecosystem-strategy) | Decides whether and how far a developer product opens into a platform other companies build on, and what that permanently obliges you to.                                      |
| [`developer-segmentation`](./developer-segmentation)             | Cuts a developer audience into a few named, sized and ranked segments, ending in one primary segment, one secondary, and a written anti-segment.                               |
| [`developer-journey-map`](./developer-journey-map)               | Maps one segment's journey from discovery to advocacy, each stage carrying an exit event, an owner and a signal, then names the single leak worth fixing next.                 |
| [`devrel-budget-allocation`](./devrel-budget-allocation)         | Splits a DevRel budget into line items that each carry a cost, a return threshold, a review date and a reallocation rule, plus a ranked cut list.                              |
| [`developer-education-strategy`](./developer-education-strategy) | Decides whether to invest in structured developer education (learning paths, labs, certification), then designs its operating model, staffing, refresh cadence and kill rules. |

### Measurement

| Skill                                                        | Description                                                                                                                                                            |
| ------------------------------------------------------------ | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| [`devrel-metrics`](./devrel-metrics)                         | Builds a DevRel measurement framework: metrics tiered from reach to business impact, each with an attribution rule, a baseline-derived target, an owner and an action. |
| [`devrel-analytics`](./devrel-analytics)                     | Builds the tracking plan behind those metrics: an event taxonomy, an identity spine, link-tagging discipline, source-confidence labelling, and funnel views.           |
| [`devrel-competitor-analysis`](./devrel-competitor-analysis) | Benchmarks a competitor's DevRel motion from publicly observable signals and returns a gap plan with a close, ignore or counter verdict per row.                       |

### Content & editorial

| Skill                                                          | Description                                                                                                                                                                        |
| -------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| [`devrel-content-calendar`](./devrel-content-calendar)         | Plans a quarter of DevRel content as dated slots with named owners and reviewers, mixed by surface, pillar and shelf life, sized against real writing capacity.                    |
| [`engineering-blog-post`](./engineering-blog-post)             | Writes or edits a technical post a skeptical developer audience believes: evidence behind every claim, published trade-offs, runnable snippets, and no marketing voice.            |
| [`developer-case-study`](./developer-case-study)               | Turns a customer's production deployment into a technical case study engineers believe: measured numbers, before-and-after architecture, published limitations, cleared approvals. |
| [`build-in-public`](./build-in-public)                         | Designs a sustainable build-in-public practice: how far up the disclosure ladder to go, a cadence the maintainer can hold, the platform mix, and the boundaries.                   |
| [`technical-video-script`](./technical-video-script)           | Writes a shooting-ready two-column script for a technical video: a 30-second hook, code-on-screen pacing, chapters, a runtime budget, and a pre-decided cut list.                  |
| [`tech-podcast-interview-prep`](./tech-podcast-interview-prep) | Prepares a guest for someone else's podcast, interview or panel: show reconnaissance, a message spine, a self-contained opening answer, and clip-safe sound bites.                 |

### Documentation

| Skill                                                                | Description                                                                                                                                                                        |
| -------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| [`developer-docs-structure-audit`](./developer-docs-structure-audit) | Audits a documentation set's structure against the Diátaxis modes, scores it on a TechDocs rubric, and returns a prioritized remediation queue.                                    |
| [`developer-quickstart-guide`](./developer-quickstart-guide)         | Writes or audits a quickstart that carries a reader from zero to one verified success: a minimal path, expected output per step, fail branches, and a cold-run time budget.        |
| [`developer-tutorial`](./developer-tutorial)                         | Writes or audits a teaching tutorial: one concept per step, checkpoints the learner can verify and resume from, guidance that fades, and a demonstrable skill at the end.          |
| [`developer-troubleshooting-docs`](./developer-troubleshooting-docs) | Turns support tickets and error telemetry into pages a developer finds by pasting the error string, plus an error-code catalog wired back into the product's own output.           |
| [`docs-code-sample-standards`](./docs-code-sample-standards)         | Defines the policy every documentation code sample must meet, then audits the existing sample corpus against it and returns a ranked fix queue.                                    |
| [`changelog-writing`](./changelog-writing)                           | Turns raw commits, pull requests and tickets into release notes developers actually read: what shipped, what it means in practice, and what breaks.                                |
| [`version-migration-guide`](./version-migration-guide)               | Writes or audits a breaking-change migration guide ordered by blast radius, with per-entry detection signals, before/after code, and a deprecation timeline.                       |
| [`coding-agent-docs-optimization`](./coding-agent-docs-optimization) | Makes SDK, API or protocol docs something a coding agent integrates from unattended: crawler access, machine-readable entry points, self-contained pages, a measured success rate. |

### Discoverability & SEO

| Skill                                                          | Description                                                                                                                                                                 |
| -------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| [`docs-seo`](./docs-seo)                                       | Runs on-page and technical SEO for a documentation site: indexability, canonicals and hreflang across versions and translations, redirects, and an owner-assigned fix list. |
| [`developer-keyword-research`](./developer-keyword-research)   | Builds a prioritized keyword list for technical queries, mined from docs search logs, support tickets and issue trackers rather than keyword-tool volume.                   |
| [`readme-optimization`](./readme-optimization)                 | Audits and rewrites a repository README into a bail-fast funnel, with every claim verified against the source and badges that earn nothing pruned.                          |
| [`github-profile-optimization`](./github-profile-optimization) | Rebuilds a personal or organization profile on a code host as a DevRel surface: the profile README, the pinned items, the bio fields, and the contribution signals.         |

### Community

| Skill                                                                | Description                                                                                                                                                                           |
| -------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| [`developer-community-launch`](./developer-community-launch)         | Decides whether, where and when to launch a developer community, then plans its seeding, first 90 days, go/no-go criteria and shutdown criteria.                                      |
| [`developer-community-health`](./developer-community-health)         | Designs a community health measurement framework: activity, responsiveness, contributor-funnel and sentiment metrics, honest instrumentation, and a report ending in decisions.       |
| [`developer-community-moderation`](./developer-community-moderation) | Writes a community's code of conduct and the moderation playbook behind it: enforcement ladder, reporting channels, incident runbook, moderator roster, platform controls.            |
| [`developer-champions`](./developer-champions)                       | Designs an unpaid, perks-only champions program: intake, published selection criteria, behaviour-based obligations, an access-first perk ladder, fixed terms, and a cohort scorecard. |
| [`developer-meetup-program`](./developer-meetup-program)             | Designs a recurring developer meetup or user group: host model, format menu, cadence, speaker pipeline, in-kind sponsors, the co-organizer ladder, and a health scorecard.            |

### Events & speaking

| Skill                                                          | Description                                                                                                                                                                 |
| -------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| [`conference-cfp-submission`](./conference-cfp-submission)     | Turns a talk idea into a submission-ready proposal for one specific event: track fit, title options, abstract, takeaways, credibility package, and a committee self-review. |
| [`tech-talk-outline`](./tech-talk-outline)                     | Turns an accepted abstract into a rehearsable outline: the one-sentence takeaway, a narrative arc, a minute-by-minute time budget, demo placement, and a slide skeleton.    |
| [`developer-live-demo-design`](./developer-live-demo-design)   | Engineers a technical demo so it survives the stage: risk triage, enterable checkpoints, a one-command reset, offline mode, and a recorded fallback, shipped as a runbook.  |
| [`developer-event-sponsorship`](./developer-event-sponsorship) | Builds a developer-event sponsorship plan: which events to sponsor at which tier, the on-site activation, and how to prove the money worked.                                |

### Open source

| Skill                                                            | Description                                                                                                                                                                              |
| ---------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| [`oss-launch`](./oss-launch)                                     | Plans and runs an open-source launch end to end: name and license clearance, the readiness gate, positioning, channel sequencing, the launch-day war room, and measurement.              |
| [`oss-license-strategy`](./oss-license-strategy)                 | Chooses a project's license and contribution policy as one decision: copyleft strength, dependency compatibility, DCO versus CLA, dual licensing, and relicensing fork risk.             |
| [`oss-governance`](./oss-governance)                             | Chooses and documents a project's governance model: decision rights, maintainer promotion, voting rules, conflict escalation, succession, trademark control, and foundation options.     |
| [`oss-contributor-onboarding`](./oss-contributor-onboarding)     | Designs and verifies the path from stranger to first merged contribution: the CONTRIBUTING file, the good-first-issue queue, a cold clone-to-passing-tests command, and the review loop. |
| [`oss-issue-triage`](./oss-issue-triage)                         | Designs a triage system maintainers can sustain: response targets sized to real capacity, a label taxonomy, intake cuts, triage duty, and a closing and staleness policy.                |
| [`oss-distribution-strategy`](./oss-distribution-strategy)       | Designs a project's ongoing distribution mix after launch: registries, discovery surfaces, curated lists, downstream packaging and release cadence, ranked against maintainer capacity.  |
| [`open-source-company-strategy`](./open-source-company-strategy) | Decides what a company open-sources and what stays proprietary, names the motive for each side of the line, says who owns the decision, and what it never closes.                        |
| [`open-standards-strategy`](./open-standards-strategy)           | Decides how a company engages a named standard or protocol, in which venue, under which patent-licensing mode, with a conformance plan and a kill rule.                                  |

### OSS funding

| Skill                                                          | Description                                                                                                                                                                     |
| -------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| [`oss-sponsors-fundraising`](./oss-sponsors-fundraising)       | Designs a maintainer-side sponsorship program: the tier ladder and its pricing, rewards that stay deliverable at scale, and the invoice-and-entity path a company needs to pay. |
| [`oss-sponsors-brand-strategy`](./oss-sponsors-brand-strategy) | Builds a company's open-source sponsorship portfolio: which projects and maintainers to fund, through which allocation model, at what amount each, and how to prove it worked.  |

### Business model

| Skill                                                      | Description                                                                                                                                                                 |
| ---------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| [`devtools-business-model`](./devtools-business-model)     | Chooses the business model for a developer tool, from open core to consumption metering to OEM licensing, and the go-to-market each one forces.                             |
| [`devtools-pricing-strategy`](./devtools-pricing-strategy) | Designs a developer tool's pricing and packaging: the value metric, free-tier limits, the tier ladder, price points bounded by real ceilings, and a safe price-change plan. |

### Brand & press

| Skill                                                | Description                                                                                                                                                                   |
| ---------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| [`tech-press-relations`](./tech-press-relations)     | Runs press and light analyst relations for a developer product: news qualification, the angle, a reporter-to-beat media map, the pitch, embargo handling, and the press page. |
| [`tech-employer-branding`](./tech-employer-branding) | Designs an employer-brand strategy for attracting software engineers: the engineering EVP, the channel plan, a verification-surface audit, and the measurement baseline.      |

## 👤 Contributors

![Contributors](https://contrib.rocks/image?repo=samber/developer-relations-skills)

## 💫 Show your support

Give a ⭐️ if this project helped you!

[![GitHub Sponsors](https://img.shields.io/github/sponsors/samber?style=for-the-badge)](https://github.com/sponsors/samber)

## 📝 License

Copyright © 2026 [Samuel Berthe](https://github.com/samber).

This project is under [MIT](./LICENSE) license.
