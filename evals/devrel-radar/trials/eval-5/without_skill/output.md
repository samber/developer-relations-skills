No — almost none of it is funnel-free. But the axis you're measuring on is wrong, and that's fixable.

**The real axis isn't "has a commercial angle." It's "does being wrong cost the author anything."**

A vendor's "State of IoT 2026" report costs the author nothing if it's wrong — the leads already converted before anyone checked the methodology. A podcast host's consultancy *can* cost them something if they say something dumb on air, because the same peers who'd hire them are the ones listening and who'll call it out. Motive never disappears. What varies is whether there's a feedback loop that punishes bad claims. That's the filter to use for anything new you find too, since any fixed list — including mine — will rot as hosts start companies and blogs get acquired.

Five questions, in order of how fast they disqualify something:

1. **Would this exist with the commercial upside removed?** A podcast two engineers have run for a decade, funded by hobbyist-gear ads, would still exist. A "developer ecosystem report" from a devtools company would not.
2. **Is the claim checkable, or just confident?** "We measured 340mA draw in deep sleep on rev C silicon, here's the scope trace" beats "developers are increasingly adopting X." Numbers with a method beat narrative every time.
3. **Who corrects errors?** A comment thread of practicing engineers who'll say "your linker script example is wrong" is a real check. A blog with comments off, or a LinkedIn post, is not.
4. **Does the author ship something that can break?** Code, a teardown, a board they built — something a stranger can go falsify. Not a survey of "1,000 developers say."
5. **Is the cadence explainable by the cost of producing it?** Real silicon-level testing is slow and expensive, so it's rare. A "weekly newsletter" with polished cadence is being paid for by something — worth asking what.

Content marketing that passes 2-4 anyway (e.g. a vendor engineering blog where the writer clearly did the debugging and would be embarrassed to be caught faking a fault-handler trace) is worth reading *as content marketing you understand the incentive of* — that's different from pretending it's neutral. The dishonest move isn't "vendor writes a blog," it's "vendor blog pretends to be neutral research."

**Your 75 minutes, split three ways, weekly:**

**~30 min — one long-form podcast episode, played at 1.5x while doing something else.**
Pick from working engineers who were doing this before they had anything to sell you:
- *The Amp Hour* (Chris Gammell, Dave Jones) — two practicing EEs, ad revenue is component/tool sponsorships, not lead gen for a consultancy. Long enough track record that a bad take gets remembered.
- *Embedded.fm* (Elecia & Christopher White) — worth naming honestly: they run a consultancy, so it's not incentive-free. But the format (walking through real past projects, not "trends") keeps it close to axis 2 and 4 above.
Skip anything titled "the future of IoT" — that's a panel, not an episode.

**~20 min — primary source, not commentary.**
This is the one category with genuinely zero funnel, because nobody's trying to persuade you of anything: pick one datasheet errata section, one Zephyr/CoAP/Matter spec section, or one silicon vendor forum thread that touches a part your SDK actually wraps. You already have to read this stuff for your job — the reframe is to treat 20 min/week of it as your "trustworthy content," not a chore that competes with the trustworthy content. It's the only category where being closer to ground truth is structurally guaranteed.

**~25 min — one post from an individual practitioner who self-funds the writing.**
- Jay Carlson's microcontroller comparisons (jaycarlson.net) — rare, expensive-to-produce, hands-on-hardware testing; he's not selling the comparison, he built the boards.
- EmbeddedRelated.com / DSPRelated.com (Stephane Boucher) — ad-supported community aggregator with many contributing engineers, not owned by one vendor trying to fill one product's funnel; the incentive is page views across a community, not conversions for one company.
- Read one vendor engineering blog (e.g. Memfault's Interrupt) *if* the specific post is a real technical postmortem (fault handler internals, linker script mechanics) — knowingly, as content marketing that happens to clear the bar, not as neutral research.

**What to cut entirely, on sight:** anything with "report," "trends," or "state of" in the title from a company that sells into that market; any newsletter whose value-add is "we read it so you don't have to" (that's a curation layer with its own incentives stacked on top of the original one); LinkedIn thought leadership, full stop.

One caveat on the named sources above: my knowledge has a cutoff, and hosts/blogs do sell out or go quiet — a 30-second RSS-date check before you commit a week's slot to something is consistent with the whole method here, not a contradiction of it.