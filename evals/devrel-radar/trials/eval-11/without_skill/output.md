Here's your weekly cloud-native reading diet, sized to 90 minutes, plus CFP targets worth prioritizing for a storage-operator angle. No general DevRel/career content — everything below is Kubernetes/CNCF/eBPF-specific.

## Weekly rotation (90 min)

**Mon — 20 min: KubeWeekly + Kubernetes release notes**
- [KubeWeekly](https://www.cncf.io/kubeweekly/) — CNCF's own curated weekly digest. Single highest signal-to-time source; skim headlines, open 2-3.
- Kubernetes release blog (kubernetes.io/blog) — only during a minor release week (K8s ships ~3x/year); check `CHANGELOG` for any `storage.k8s.io` / CSI API changes.

**Tue — 20 min: CNCF TAG Storage / SIG Storage**
- TAG Storage GitHub repo (`cncf/tag-storage`) — meeting notes, working-group docs. Read notes, don't watch the recording.
- SIG Storage Slack (`#sig-storage` on kubernetes.slack.com) — skim, don't participate weekly. This is also where you build the reputation that makes a CFP submission credible later.

**Wed — 20 min: storage-project blog, rotate weekly**
Pick one per week, cycle through: Rook (rook.io/blog), Longhorn (longhorn.io/blog), OpenEBS, Piraeus/LINSTOR, Ceph. Direct competitors/adjacents to Skaldis — track their positioning, not just features, since that's what informs your own talk angles.

**Thu — 15 min: eBPF**
- Isovalent/Cilium blog — the highest-output eBPF source, frequently touches storage-adjacent topics (observability of block/volume I/O, node-level tracing).
- eBPF.io blog as backup when Isovalent is quiet that week.

**Fri — 15 min: CFP scan**
- [Sessionize](https://sessionize.com/) explore page, filtered to Kubernetes/cloud-native events.
- [Data on Kubernetes Community](https://dok.community/) events page — **this is your single best-fit CFP target**, purpose-built audience for a storage operator. Check for open CFPs and their co-located "Data on Kubernetes Day" at KubeCon.
- Only spend this slot when a CFP window is actually open — otherwise swap for r/kubernetes skim.

## CFP targets, ranked for a storage operator

1. **Data on Kubernetes Day** (co-located with KubeCon NA/EU) — narrowest, most relevant audience you'll find; go here first.
2. **Cloud Native Rejekts** — unofficial KubeCon-adjacent track, historically friendlier to first-time and niche speakers than the main CFP.
3. **KCDs (Kubernetes Community Days)** — regional, lower bar, good place to test a talk before submitting it to KubeCon proper.
4. **KubeCon + CloudNativeCon (NA/EU)** main track — hardest CFP, most competitive; submit once you've validated the talk at a KCD or Rejekts.

CFP dates move every cycle and I can't verify current 2026/2027 deadlines right now (web search wasn't available this session) — check `sessionize.com` and `dok.community` directly before you plan around a specific date; don't trust a remembered date here.

## What I deliberately left off

No general "how to be a DevRel" newsletters, no generic tech-news aggregators (The New Stack, Hacker News), no podcast subscriptions — none of that fits a 90-minute, storage-and-eBPF-focused budget without displacing the higher-signal sources above. If you later get more time, TAG Storage's recorded meetings and the Kubernetes Podcast from Google are the next things to add, in that order.