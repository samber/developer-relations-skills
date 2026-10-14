## Can Ridgeline badge all 180 samples "Verified"? No — only 124 of them.

This is a tier-classification question, not a documentation-polish question, and the skill this maps to (`docs-code-sample-standards`) is explicit that badging is a claim about **execution tier**, not about whether `go test ./...` returns exit code 0.

### The finding

`go test ./...` returning green tells you all 180 samples **compile**. It does not tell you all 180 **run**. Go's `testing` package only invokes an `ExampleXxx` function's body when it has a matching `// Output:` (or `// Unordered output:`) comment. This is documented behavior, not an edge case:

> "Example functions without output comments are compiled but not executed." — Go blog, *Testable Examples in Go*; same statement in `pkg.go.dev/testing`.

Mechanically, `testing.runExample` contains the equivalent of:

```go
if !*chatty.on && eg.Output == "" && !eg.Unordered {
    return true // never calls eg.F()
}
```

So unless your CI invokes `go test -v ./...` (the command you described is `go test ./...`, no `-v`), the 56 `// Output:`-less examples' function bodies are **never called at all** — not "run without a check," literally never executed. A nil-pointer panic, a broken call, a logic error inside any of those 56 would not fail the build today. If CI does happen to run with `-v`, the bodies do execute (a panic would fail the build) but stdout is still compared against nothing, so wrong output is silently accepted.

Either way, the 56 do not meet this skill's definition of tier **Run** ("Executes; documented output is asserted"). At best they are tier **Compile-only** ("type-checks or builds, never executed"). Putting a "Verified" badge on them makes a claim CI cannot back up.

This is exactly the failure mode the skill calls out by name:

> "Watch the matching trap: in ecosystems where the assertion is opt-in, an example without its expected-output marker still compiles and is never actually run, so it passes CI while claiming more than it proves."

And the audit checklist is explicit about severity: **"a 'Run' sample that CI does not run is a P1, not a P2."** So if the 56 ship a "Verified" badge as-is, that's not a cosmetic gap — it's a P1 finding, on par with a broken import or a removed symbol, because the badge asserts a guarantee ("this works as written") that nothing in your pipeline checks.

### Tier classification, all 180

| Tier | Count | % | What CI actually enforces | Reader-facing claim it supports |
|---|---|---|---|---|
| Run | 124 | 68.9% | Compiled, executed, stdout diffed against `// Output:` | "Verified" ✅ |
| Compile-only | 56 | 31.1% | Type-checks/builds only; body not called (or called but unchecked under `-v`) | "Builds against current SDK" — **not** "Verified" |

Baseline framing (self-set by this skill, not an industry standard — cited so you can attribute it correctly if this goes into a report): the skill's bar for a governed corpus is ≥80% of samples at tier Run-or-Compile-only *and* zero P0/primary-adoption-path samples untested at Run. You're at 100% compiled, 68.9% actually Run. The gap isn't a testing-infrastructure problem (you already have one, and it's green) — it's that 56 samples opted out of the assertion instead of being engineered around it.

### Why "non-deterministic output" is not actually a wall

Timestamps and generated IDs are a normalization problem, not a permanent verification blocker — unlike the two cases that legitimately force Compile-only forever (billing side effects, destructive operations, third-party dependencies you don't control). Three fixes, cheapest first:

**1. Don't print the non-deterministic field raw — assert its shape instead.** This is almost always the right fix and requires no SDK changes:

```go
// Before — why it has no Output comment today:
node, err := client.CreateNode(ctx, ridgeline.NodeSpec{Label: "Person"})
if err != nil {
    log.Fatal(err)
}
fmt.Printf("created node %s at %s\n", node.ID, node.CreatedAt)
// node.ID and node.CreatedAt differ every run — no stable Output is possible.

// After — same call, same teaching point, deterministic and actually executed:
node, err := client.CreateNode(ctx, ridgeline.NodeSpec{Label: "Person"})
if err != nil {
    log.Fatal(err)
}
fmt.Printf("label=%s hasID=%t hasTimestamp=%t\n",
    node.Label, node.ID != "", !node.CreatedAt.IsZero())
// Output:
// label=Person hasID=true hasTimestamp=true
```

This now genuinely executes on every `go test` and genuinely fails if `CreateNode` starts returning a zero-value ID or timestamp — real regression coverage, not decoration.

**2. Assert on error only, blank expected output.** For samples where the ID/timestamp isn't the teaching point, `// Output:` with nothing after it is valid — it asserts stdout is empty and still forces the body (including any `log.Fatal`/panic path) to run:

```go
node, err := client.CreateNode(ctx, ridgeline.NodeSpec{Label: "Person"})
if err != nil {
    log.Fatal(err)
}
_ = node
// Output:
```

Weaker signal for the reader (nothing printed), but strictly better than Compile-only, and near-zero rewrite cost.

**3. Inject a deterministic clock/ID generator, if the SDK supports it.** If `ridgeline.Client` takes functional options, a `WithClock`/`WithIDGenerator` used only in the example gets you a *real*, literal, stable value to print (`id=n_00000001`) — the strongest version of "Verified" because the reader sees exactly what a real response looks like, not a redacted stand-in. Worth checking before defaulting to option 1, but don't block the other 55 samples on adding this capability if it doesn't already exist.

None of these need the samples to move out of `example_test.go` or need a CI harness you don't already have — you already run `go test ./...` on every push. This is a rewrite of ~56 print statements, not new infrastructure.

### What to ship now vs. what to fix first

- **This week, ship truthfully:** Badge the 124 with `// Output:` as "Verified." For the 56, either omit the badge or use a second, honestly-different label matching the tier's actual reader signal — e.g. "Compiles against current SDK" — not a visually-similar checkmark. An unlabelled or identically-labelled Compile-only sample next to a Run sample is indistinguishable to the reader from a broken promise, which is the same complaint this skill raises about unlabelled Illustrative samples.
- **Fix queue, ordered by adoption-path impact first (not raw count):** identify which of the 56 sit on the primary adoption path (connect-to-database, create-first-node, first-query) — those get the option-1/option-3 rewrite first regardless of effort ratio, per this skill's rule that Run is promoted ahead of the effort ranking on the adoption path. The remaining long tail can be batched.
- **Re-measure, don't declare victory from the diff:** after the rewrite batch, rerun `go test ./...` and recount `// Output:` occurrences — the 68.9% → target-100% number is the actual gate, not "we edited N files."

**Bottom line for marketing:** put "Verified" on the 124 today. The other 56 need roughly a day or two of engineering (redact-and-assert, not new tooling) before the same badge is true for them — treat that as a scoped fix, not a blocker on the whole launch.