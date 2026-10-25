# Curators, swaps and republishing

Read this when writing the runbook for item pitches, a swap, unpaid recommendations or republishing.

## Contents

1. What curated developer newsletters publish about selection
2. Item pitch template
3. Swap and recommendation templates, disclosure label
4. Republishing and posting the lead item

## 1. What curated developer newsletters publish about selection

Pitch an item, not your newsletter. Curators link the release notes or the post, not your archive, so the pitch grows your list only if that linked page ends with a signup line carrying its own source tag. Make that line a precondition of every pitch. Check each curator's current page before pitching: these rules change.

| Newsletter | Route | Published rules worth checking |
|---|---|---|
| Console.dev | email the editors, address on its selection-criteria page | tools only, not articles; the primary user is a developer, self-service signup, clear docs; beta tools only when pre-1.0 or labelled beta; no sponsored reviews |
| This Week in Rust | a pull request to the draft issue for most sections; the project and tooling updates section no longer takes pull requests, so tag the newsletter's social account there | Rust community work; no paywalled links, including members-only posts; commercial offerings generally excluded from events |
| KubeWeekly (CNCF) | the submission form on its page | of interest beyond one commercial platform, no press releases without feature substance, no gated content, recent; check whether it still publishes separately from the CNCF's monthly newsletter |
| Cooperpress titles (JavaScript Weekly, Golang Weekly, Node Weekly and others) | the editors' contact on each title's site | no published criteria; sponsored items are labelled |
| Others with no published process | the editor's public contact | assume the same: useful, ungated, recent, not a press release |

Rules that hold across them:

- Link the ungated page: the post, the release notes, the repository. Never a form.
- One pitch per curator per real piece of news. Repeated pitches for minor releases teach the curator to skip you.
- Never offer payment for an editorial pick. A paid slot is a separate purchase with a label.
- Accept no as the answer. Curator independence is what makes the placement worth having.

## 2. Item pitch template

```
Subject: <what it is> - <the one fact a reader cares about>

Hi <name>,

<Project> <version> <does X>. The part your readers may care about:
<one concrete change, with the number or flag the release notes state>.

Link: <ungated URL to the post or release notes>
Who it's for: <one line>
Disclosure: I work on <project> at <company>.

No reply needed if it doesn't fit.
<name>
```

Keep it to these lines. Curators scan many pitches per issue and read the first two lines.

## 3. Swap and recommendation templates, disclosure label

Check overlap before proposing: same primary reader (language, stack, role), and a peer whose issues you would recommend without the swap. A swap with a list that does not overlap brings subscribers who leave in their first issues.

```
Proposal: we feature <peer> in our <date> issue, you feature us in yours.
Each side writes its own two-line description and approves the other's.
Each side labels it in the issue: "Swap: we recommend <peer>, they recommend us."
Each side tags the link (swap-<peer>) and shares confirmed signups after 30 days.
One swap first; repeat only if the 30-day numbers hold.
```

Unpaid platform recommendations follow the same overlap check. Label the recommendations you publish ("Newsletters we recommend, some of which recommend us"), and confirm the platform's signup choice names your newsletter, unticked.

Label every swap, recommendation exchange or paid recommendation you publish. A reciprocal deal is very likely a material connection under the US endorsement guides even with no money, UK and EU advertising rules also want a label, and readers who find an undisclosed swap stop trusting the editorial picks around it.

## 4. Republishing and posting the lead item

- Republish a full issue only where you can set a canonical link to the archive page. dev.to supports a `canonical_url` front-matter field; check each other platform's current settings before relying on one.
- Where no canonical link can be set, post an excerpt of the lead item plus a link to the archive.
- Read each community's self-promotion rule before posting there; many forums and subreddits ban newsletter links outright.
- End every republished copy with the signup line and its own source tag (`republish-<platform>`).
- Republish only after the archive page is live.
