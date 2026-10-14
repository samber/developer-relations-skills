Keep all 3. No dates ≠ abandoned here — all 3 are reference/framework sites, not blogs. Dateless is a design choice for evergreen content, not neglect signal.

**Why drop-fear wrong:**
Blog = dated posts, staleness visible. Framework/guide site = living document, revised in place, no post-date makes sense. Real neglect signal = GitHub repo dead (no commits, no merged PRs), not missing on-page date.

Note: tried to pull live commit history (GitHub API + WebFetch) to confirm current activity — blocked, no tool permission this session. Below = trained knowledge, solid but not freshly verified. Re-run check once you grant web/gh access, or check yourself:

```
gh api repos/evildmp/diataxis-documentation --jq '.pushed_at'
gh api repos/github/opensource.guide --jq '.pushed_at'
```

**Per site:**

1. **diataxis.fr** — Daniele Procida's framework (tutorials/how-to/reference/explanation). Widely adopted (Django, Python packaging docs, Cloudflare, Gatsby cite it). Not abandoned, just stable-by-design. Budget: 5 min/week, batched into 20 min/month.

2. **opensource.guide** — GitHub-owned property, official repo `github/opensource.guide`. Content mature, low edit cadence, but institutionally backed — won't vanish. Budget: 15 min/quarter, skip weekly checks.

3. **developerrelations.com/guides** — community-run (DevRel Collective), thinner institutional backing than the other two, higher risk of silent staleness. Budget: 10 min/month, watch for signs of life (new guide, active socials).

Total: ~5-10 min/week combined if batched. Weekly per-site checking is wasted effort — nothing changes that fast on any of these.

**Better use of that time — one-time moves, not recurring checks:**

1. Diataxis: restructure/label Pellier docs to match framework, link back to diataxis.fr as source. ~1 hr one-time, not recurring.
2. opensource.guide: cite it in Pellier's CONTRIBUTING.md as reference — passive backlink, zero maintenance.
3. developerrelations.com: check if guides section takes contributor submissions — community-run sites often do. Could be a guest-guide slot, not just a link target.

Next: pick one of the 3 above, do it this week.