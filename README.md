# Developer relations skills

Skills for the people who run **developer relations**: documentation, open source, community, events, technical content, and the program strategy and measurement holding it together.

Written for **developer advocates, DevRel leads, community managers and OSS maintainers**. Every skill ends in **a decision or a shippable artifact**, never a list of best practices.

## Install

Install every skill in this repo, not just one. Skills here are atomic by design and reference each other freely — picking a single skill leaves its sibling skills uninstalled, so cross-references and routed handoffs go nowhere.

**skills.sh (universal)** — works with any Agent Skills-compatible tool:

```bash
npx skills add samber/developer-relations-skills
```

**Claude Code** — install the plugin:

```bash
/plugin marketplace add samber/cc
/plugin install developer-relations-skills@samber
```

**Codex (OpenAI)** — install via the Codex CLI:

```bash
codex plugin add github:samber/developer-relations-skills
```

**Cursor** — copy into Cursor's skills directory:

```bash
git clone https://github.com/samber/developer-relations-skills.git ~/.cursor/skills/developer-relations-skills
```

Cursor auto-discovers skills from `.agents/skills/` and `.cursor/skills/`.

**Gemini CLI** — install as a Gemini extension:

```bash
gemini extensions install https://github.com/samber/developer-relations-skills
```

Update with `gemini extensions update developer-relations-skills`.

## 📚 Related Collections

- [`developer-platform-skills`](https://github.com/samber/developer-platform-skills) — Platform & SDK developer experience — _for platform engineers, DX engineers, SDK authors, API product managers, DevRel engineers_
- [`dev-event-organizer-skills`](https://github.com/samber/dev-event-organizer-skills) — Technical event operations — _for event organizers, conference producers, hackathon leads, community builders_

_Part of the [samber skills ecosystem](https://github.com/samber?tab=repositories&q=skills)_

## 📦 Skills

This collection covers the full developer-relations surface. Start here:

- [`developer-relations-kickoff`](./developer-relations-kickoff) — Routes the current DevRel task to the right skill of this collection, or says plainly that none fits, then bootstraps or resumes the project's shared context artifact.
- [`devrel-career`](./devrel-career) — Plans a DevRel career from the candidate side: portfolio audit against real hiring signals, the six-rung IC ladder, interview-loop prep, and offer evaluation.
- [`devrel-team-structure`](./devrel-team-structure) — Designs the DevRel org: the reporting line and what it starves, the team shape, the coverage map, the interlocks, and the trigger for the next re-org.
- [`devrel-radar`](./devrel-radar) — Builds a time-budgeted watch list of DevRel podcasts, newsletters, communities, conferences and practitioners, plus the routine that keeps it verified and fresh.

Browse all skills and their descriptions in [`references/skill-catalog.md`](./references/skill-catalog.md).

## 📄 License

MIT © 2026 Samuel Berthe
