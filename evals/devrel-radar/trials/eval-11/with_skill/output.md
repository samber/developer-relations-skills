**DevRel radar — Skaldis (Kubernetes storage operator), cloud-native only**
Role: DevRel, owns advocacy + community (not marketing, not enablement — you didn't claim those). Ecosystem: cloud-native / Kubernetes / CNCF / eBPF, hard-scoped per your request. Budget: 90 min/week.

**Two things before the list:**

1. **This is a reading list verified on one date, not a monitoring service** — nothing here alerts you when a source dies. Re-run the refresh routine quarterly for that.
2. **I could not browse the web this session** (WebFetch/WebSearch permission wasn't granted, and I stopped retrying rather than guess dates). This skill's own reference file is general-DevRel-practice-heavy and only has one entry that's actually cloud-native (KubeCon NA), so almost everything below is real, well-established cloud-native infrastructure I have high confidence exists from training knowledge — CNCF, Kubernetes, and eBPF's canon is small and stable — but I have **not fetched a dated page for it this session**. Every such entry is marked `unverified this session` below, never presented as verified. Treat this as a strong draft that needs one ~10-minute click-through before you rely on it (grant me WebFetch/WebSearch and I'll finish that pass, or run it yourself/via Claude in Chrome — 18 URLs, checklist at the bottom).

Assumptions made instead of asking (swap anything wrong): standing weekly habit, not catching-up (no signal you're new to the role); medium = text + peer community, no video (unstated, and video channels can't be verified by this skill's own method anyway) or audio (unstated, no commute signal to halve podcast cost against).

---

## Weekly core — 76 of 90 min

Ordered by value per attention-minute: blogs/newsletters tie highest efficiency, community sits mid-table but gets extra weight here because it's a pillar you own, not just this skill's usual 10% renewal slot.

| Source | URL | Medium | Pillar | Min | Why it earned the slot | Selling | Status |
|---|---|---|---|---|---|---|---|
| CNCF Blog | cncf.io/blog | blog | advocacy, community | 8 | Foundation's own channel for ambassador spotlights, TAG/community-program news, project graduations — the closest thing cloud-native has to a DevRel signal | foundation (CNCF, member-company funded) | unverified this session |
| Kubernetes Blog | kubernetes.io/blog | blog | community | 7 | Release notes + SIG updates, including SIG Storage — what's shipping that Skaldis has to react to | foundation/project | unverified this session |
| Last Week in Kubernetes Development | lwkd.info | newsletter | advocacy, community | 8 | Volunteer-run weekly digest of Kubernetes dev activity (KEPs, SIG notes) — cheapest way to stay current without reading raw SIG minutes | independent/volunteer | unverified this session |
| The New Stack | thenewstack.io | news site | all four, cloud-native-heavy | 10 | Daily cloud-native trade press; this skill's own audit already verified it active and it's the budget compressor by design | ad-supported media | **verified-active, 26 Aug 2026** (carried from this skill's reference audit) |
| Cilium / Isovalent Blog | cilium.io/blog (or isovalent.com/blog) | blog | ecosystem (eBPF-technical) | 8 | Cilium is the CNCF-graduated reference implementation for eBPF networking in Kubernetes — deepest, most frequent eBPF-in-k8s writing available | tool vendor (Isovalent, acquired by Cisco) | unverified this session |
| Data on Kubernetes Community | dok.community | community (Slack + mailing list) | community | 25 | The one community built specifically around storage/data workloads on Kubernetes — the exact peer network for Skaldis' niche, and your renewal slot for discovering new sources | community, sponsored by K8s storage/data vendors — **likely includes Skaldis' competitors, worth knowing who** | unverified this session |
| Kubernetes SIG Storage | github.com/kubernetes/community (sig-storage) | mailing list + meeting notes | advocacy, community | 10 | Where Kubernetes storage standards (CSI, volume snapshots) actually get decided — the standards-body seat a storage vendor needs a voice in. CNCF also runs a separate cross-project TAG Storage; check which is more relevant to your integration surface | open governance (CNCF-hosted project) | unverified this session |

## Monthly — reference, not a weekly feed

| Source | URL | Pillar | Why | Status |
|---|---|---|---|---|
| CNCF Ambassador Program | cncf.io (Community → Ambassadors) | advocacy | Directory of recognized cloud-native advocates + application cycle — check who's active and whether applications are open | unverified this session |
| eBPF.io | ebpf.io | ecosystem reference | Hub for eBPF: docs, project catalog (Cilium, Falco, Pixie…) — the field map, not a feed | unverified this session — evergreen by nature |
| CNCF Landscape | landscape.cncf.io | ecosystem reference | Full cloud-native ecosystem map — useful for spotting adjacent/competing storage projects before a talk or post | unverified this session — evergreen by nature |

## Annual — cite the edition, not a bare number

- **CNCF Annual Survey** (cncf.io) — cloud-native adoption data. Edition/date unconfirmed this session.
- **State of Data on Kubernetes** report (via dok.community) — historically co-sponsored by Kubernetes storage/data vendors. Edition/date unconfirmed — **check the current sponsor list before citing it**, since it may be funded by a Skaldis competitor.

## Events — one CFP target, one attend

You said you're submitting talks this year, so this section is core, not optional.

- **CFP target:** KubeCon + CloudNativeCon Europe, next edition (likely 2027) — CFPs for this event historically open in autumn and close in December/January. Not confirmed this session; check before you write the abstract.
- **Lower-barrier CFP alternative:** Cloud Native Rejekts — community-run unconference co-located with KubeCon, historically friendlier to first-time cloud-native speakers. Dates unconfirmed this session.
- **Attend:** KubeCon + CloudNativeCon North America, Salt Lake City, 9–12 Nov — carried from this skill's own reference file, which already flags "year not rendered on the page, confirm before booking." At ~7 weeks out from today, its CFP is certainly closed — attend-only target, not a submission target.

## Bench (verified substitutes — didn't make the weekly cut, or status is genuinely uncertain)

- **Kubernetes Podcast from Google** (kubernetespodcast.com) — I'm not confident this show is still actively publishing after past host changes. Confirm before adding ~45 min/week for it.
- **eBPF Summit** — occasional eBPF-focused event, historically Isovalent-run. Current cadence unconfirmed.
- **CNCF Slack** (slack.cncf.io) — broader general cloud-native chat than Data on Kubernetes Community. Good secondary if DoK feels too narrow; didn't fit the budget.

---

**Totals** — 76 of 90 weekly minutes (16% headroom, not force-filled). 7 weekly core + 3 monthly + 2 annual + 3 events + 3 bench = 18 entries.
**Verified-active: 1 of 18** (The New Stack) + 1 carried-active-with-a-standing-caveat (KubeCon NA's date). The other 16 are real, well-known cloud-native institutions but **unverified this session** — this list does not yet clear this skill's own pass bar ("every owned pillar has a verified-active source"), only "every owned pillar has a strong unverified candidate."
**Pillar coverage:** advocacy — candidates present, none verified-active yet. Community — candidates present, none verified-active yet. **Not met at the verified-active bar until the click-through below happens.**

**Dropped** — the entire generic-DevRel-practice source set this skill's reference file otherwise recommends: DeveloperRelations.com guides, Community Pulse, DevRel Weekly, Scaling DevTools, Console.dev, CMX, Write the Docs, DevRel Collective Slack, and practitioners like Mary Thengvall, Wesley Faulkner, Tessa Kriesel. Constraint: cloud-native-only scope, your explicit request. Naming the tradeoff this skill normally warns against: dropping every practice-of-DevRel source means you stay current on Kubernetes/CNCF/eBPF but not on DevRel itself as a discipline — say the word and I'll add one back (Community Pulse is the strongest single pick for advocacy+community).

**Not now** — CNCF TAG App Delivery / TAG Runtime: no signal you own platform-engineering standards work beyond storage; revisit if that scope grows.

---

**Verification checklist** (18 URLs, ~10 min) — for each: open it, find the newest dated item, confirm who runs it, classify as active (≤3mo) / stale (>6mo) / dead: cncf.io/blog · kubernetes.io/blog · lwkd.info · cilium.io/blog · dok.community · github.com/kubernetes/community (sig-storage) · cncf.io ambassadors page · ebpf.io · landscape.cncf.io · CNCF Annual Survey page · dok.community's State of Data on Kubernetes report · KubeCon+CloudNativeCon Europe CFP page · Cloud Native Rejekts · kubecon NA 2026 event page · kubernetespodcast.com · eBPF Summit page · slack.cncf.io. Grant WebFetch/WebSearch next time and I'll run this pass myself in one go.