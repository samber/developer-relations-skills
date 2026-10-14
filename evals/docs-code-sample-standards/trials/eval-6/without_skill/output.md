## Short answer: No — not for all 180, not as things stand today.

A green `go test ./...` run does not mean all 180 examples were executed. It means something much weaker for 56 of them, and marketing's "Verified" badge would be a false claim if applied uniformly.

## Why: Go only *executes* an Example if it has an `// Output:` comment

This is documented `testing` package behavior, not a project-specific quirk:

> Example functions without output comments are compiled but not executed.

Concretely, `go test` builds a wrapper that calls an `ExampleXxx` function and diffs its captured stdout against the `// Output:` comment — but only if that comment exists. If it doesn't, the compiler still type-checks the function (so it must compile against the current API), but **the function body is never invoked at test time.** No wrapper is generated, nothing calls it, its `fmt.Println`/API calls never execute.

So the 180 samples actually split into two very different guarantee levels:

| | 124 samples (have `// Output:`) | 56 samples (no `// Output:`) |
|---|---|---|
| Compiles against current API | ✅ | ✅ |
| Function body actually runs in CI | ✅ | ❌ never called |
| Output/behavior checked against expectation | ✅ | ❌ nothing to check |
| A panic, wrong return value, broken auth call, or bad API usage inside it would fail CI | ✅ | ❌ silently invisible |

"CI is green on every push" is true for both groups, but it's proving two different things: for the 124, it's proving *correctness*; for the 56, it's only proving *the code still parses and type-checks*. If the SDK's `CreateNode` call, say, started silently returning a malformed object tomorrow, any of the 56 examples exercising it could be dead wrong and CI would never notice.

## What this means for the badge

Putting "Verified" next to all 180 overstates what's true for the 56. If a customer copy-pastes one of those 56 samples and it panics at runtime, "Verified" becomes a credibility problem, not a marketing win.

## Two real options

**Option A — badge only what's actually verified today.**
Ship "Verified" on the 124, leave the other 56 unbadged (or with a lesser label like "Compiles" / "Syntax-checked"). Zero engineering work, ships today, but permanently caps the badge at 124/180 unless someone does Option B later.

**Option B — make the 56 genuinely executable, then badge all 180.**
The non-determinism (timestamps, generated IDs) is in the *values*, not in whether the call can be verified at all. The standard fix is to stop printing the raw non-deterministic value and instead print a deterministic assertion derived from it, while still making the real call:

```go
func ExampleClient_CreateNode() {
    node, err := client.CreateNode(ctx, ridgeline.NodeSpec{Label: "Person"})
    if err != nil {
        fmt.Println(err)
        return
    }
    fmt.Println(node.Label)
    fmt.Println(len(node.ID) > 0)       // ID is generated — assert shape, not value
    fmt.Println(node.CreatedAt.IsZero()) // timestamp is generated — assert it's set, not the value
    // Output:
    // Person
    // true
    // false
}
```

This still runs the real SDK call, still exercises the real code path, and now has a deterministic `// Output:` that `go test` genuinely checks — so it earns the badge honestly. It requires editing each of the 56 (not just relabeling them), rewriting their last few lines to print derived/normalized facts instead of raw dynamic fields. `Unordered output:` doesn't help here — it only handles non-deterministic *ordering* of otherwise-fixed lines (e.g., map iteration), not non-deterministic *values* like IDs and timestamps.

## Recommendation

Ship Option A now — badge the 124 today, since that's true. Treat the 56 as a tracked follow-up (Option B), since it's mechanical, bounded, and turns "180 examples, 124 actually tested" into "180 examples, 180 actually tested." Don't let marketing badge the 56 in the meantime by pointing at "CI is green" — that phrase is true but is being used to prove something CI isn't actually checking.