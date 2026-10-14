Reading the skill's `published-findings.md` (the reference it points to for exactly this kind of ask) changes the answer you're going to get — there is no single industry-standard number for any of the three metrics you named. Here's what actually exists, sourced, so you don't put a fabricated benchmark on a board slide.

## The honest picture

**There is no industry body, standard, or vendor spec that sets a required compile-rate, test-coverage rate, or cross-language parity rate for documentation code samples.** The numbers that circulate in this space are either (a) academic measurements of how bad *ungoverned* public corpora are, or (b) baselines individual teams or this skill set for themselves. Presenting either as "the bar we're being measured against" would be citing something that doesn't exist — the skill's own reference file states plainly: "a fabricated benchmark in a sample-quality audit destroys the credibility of the whole report."

## What you can actually cite, by category

**1. The one number with a real external source and a defensible use as a gate:**
- **43% deprecated-API rate** — Zhou & Walker, FSE 2016, measured in Stack Overflow posts they analyzed. This skill uses "above 43%" as the trigger to prioritize a deprecation-detection gate. It is a measured research finding repurposed as a threshold — not a compliance standard, and not about compile rate or parity.

**2. Measured baselines — these describe how bad *unmaintained public* snippets are, and are commonly (mis)quoted as if they were targets. They are not targets. They're the floor, not the bar:**
- 1.00% of Java Stack Overflow snippets compile; 0.12% of C# ones do (Yang, Hussain & Lopes, "From Query to Usable Code," MSR 2016, ~3M snippets)
- 25.61% of Python / 20.00% of JavaScript Stack Overflow snippets are runnable (same study)
- Only 24.4% of public Python gists run out of the box with no fixes (Horton & Parnin, "Gistable," ICSME 2018)

If anything, these numbers argue the opposite of a high bar — they show what happens with zero governance, and are useful only as "don't be this."

**3. Self-set baselines — used across teams that adopt this kind of policy, explicitly *not* published industry standards:**
- ≥90% compile/run pass rate before you stop new authoring and run a remediation sprint (this skill's own choice, "well supported by" the MSR 2016 numbers but not itself a standard)
- ≥80% of the corpus at tier Run or Compile-only, executed in CI
- ≥90% parity coverage across tier-1 languages (tested cells ÷ scenarios × tier-1 languages) — this is the number closest to what you called "the accepted cross-language parity number," and it is **not accepted anywhere externally**; it's this skill's own recommended default
- 100% of primary-adoption-path samples (quickstart, auth, first call) at tier Run, zero P0 findings

**4. The nearest thing to an actual vendor requirement:** Google Cloud requires integration tests for critical-path samples. That's a policy from one vendor, not an industry-wide figure, and it doesn't come with a percentage.

**5. Named on the "never repeat" list** — flagging in case it surfaces elsewhere in your prep: the "94% of developers consider API documentation quality a key factor" stat is widely attributed to SlashData but unconfirmed in any actual SlashData report. Don't use it.

## What I'd put on the slide instead

Given the above, a defensible board slide has two rows, not one "industry standard" row:

| Metric | What we set as our target | Why (source) |
|---|---|---|
| Compile/run pass rate | ≥90%, remediation sprint if below | Self-set threshold; supported by MSR 2016 data on how low ungoverned corpora fall (1–25% depending on language) |
| Tested-sample coverage | ≥80% of corpus at Run/Compile-only tier | Self-set; no external standard exists |
| Cross-language parity | ≥90% of scenarios covered across tier-1 languages | Self-set; no external standard exists |
| Deprecated-API rate | Keep below 43% | Zhou & Walker, FSE 2016 (measured comparator) |

Frame it to the board as "the bar **we've** set, informed by the worst-case data from ungoverned public corpora" — not as an external standard you're being measured against, because no such external standard exists to be measured against.