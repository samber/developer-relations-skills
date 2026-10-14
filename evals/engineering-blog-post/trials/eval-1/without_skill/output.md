Here are the fixes, ready to paste into the draft in place of the flagged passages. Each entry shows what breaks and the replacement text. Four of the six need a real number or fact from you before they go live — I've marked those with `[FILL IN: ...]` rather than inventing figures, since a fabricated stat is worse than a missing one in a post that's already making unverifiable claims.

---

**1. Intro — drop the superlative**

Cut: *"Kestrel is the fastest columnar index builder available today."*

Replace with:

> Our index builds were taking `[FILL IN: old duration]`, which was slowing down `[FILL IN: what this blocked — deploys, batch jobs, dashboard refresh, etc.]`. After moving to Kestrel, that dropped to `[FILL IN: new duration]`.

"Fastest available today" is a claim about every competing tool, unfalsifiable, and the kind of line that gets a post torn apart in the comments by anyone who's benchmarked two of them. State what changed for you instead — it's stronger because it's specific, and nobody can fact-check you into a correction.

---

**2. Results section — the chart needs a methodology paragraph**

A bar chart captioned "12x faster" with no accompanying text answers no questions a skeptical reader will ask. Add this immediately before or after the chart:

> We measured build time on `[FILL IN: dataset — size, row count, schema shape]`, running on `[FILL IN: hardware/instance type]`. Before: `[FILL IN: baseline time]`, averaged over `[FILL IN: N runs]`. After: `[FILL IN: new time]`, same conditions. That's where the 12x figure comes from.
>
> This held for `[FILL IN: what kind of workload — cold builds, incremental, a specific table]`. We haven't tested `[FILL IN: other cases, if any]`, so treat this as the number for our workload, not a general benchmark.

If the 12x number can't be traced to a specific before/after pair with a stated dataset and hardware, don't publish it — that's the fastest way for this post to get flagged as marketing rather than engineering.

---

**3. Paragraph four — cut the unverifiable scale claim**

Cut: *"thousands of teams rely on Kestrel in production."*

If you have a real number, use it:

> Kestrel is used in production by `[FILL IN: real number or "several teams," "our data platform team," etc.]`.

If you don't have a number you can stand behind, cut the sentence entirely rather than round up. Nothing in the rest of the post needs it — the post is about your own migration, not Kestrel's install base.

---

**4. Code snippet — add imports and fix the renamed method**

Two separate fixes:

- Add an imports block at the top of the snippet. At minimum this means `import kestrel` (or whatever the package is actually named) plus anything else the 40 lines reference — file/path handling, config objects, etc. A snippet that doesn't run as pasted will get called out immediately by any developer who tries it.
- `kestrel.build_index()` was renamed in 2.0. Find the current name in the 2.0 migration guide and swap every call site in the snippet — check whether the signature changed too (new required args, different return type), not just the name. Test the snippet by actually running it against a current Kestrel install before it goes in the post. A renamed-method bug in a "here's how we did it" post is the single most credibility-damaging kind of error, because it's the one detail readers will actually try to execute.

---

**5. Closing paragraph — replace the boilerplate**

Cut: *"we're thrilled to share this journey with the community."*

Replace with something that gives the reader a next action instead of a feeling:

> If you're evaluating Kestrel for a similar migration, the tradeoffs above are the ones that mattered for us — check whether they apply to your workload first. `[FILL IN: link to repo/docs/issue tracker, or an invitation to reach out with questions]`.

---

**6. New section — where this doesn't help**

Insert before the closing paragraph, titled something like "Where Kestrel didn't help" or "Tradeoffs." A 12x number with zero caveats reads as an ad; the fastest way to fix that is to name where the win doesn't hold. Fill in the real answers — the shape below is what the section needs to cover, not filler text to publish as-is:

> Kestrel isn't a universal win. `[FILL IN — pick whichever actually apply]`
> - On `[small datasets / high-cardinality columns / write-heavy workloads / whatever your actual weak case is]`, we saw `[little improvement / a regression / added complexity]`.
> - Migrating cost us `[FILL IN: time, rewritten queries, retraining, etc.]` — factor that in before you start.
> - We haven't tested `[FILL IN: scenarios outside what you validated]`, so we can't speak to those.

If nothing here is true — if the migration really had zero downsides — say that explicitly instead of leaving the section out, because "we found no drawbacks" is itself a claim worth stating rather than implying by omission.

---

One more thing worth flagging directly: five of these six problems are the same problem wearing different clothes — a claim stated with more confidence than the post can back up (fastest, 12x, thousands, "no downsides" by omission). Once you fill in the real numbers above, skim the rest of the 2,100 words for the same pattern; there's a good chance more of it snuck in past what's summarized here.