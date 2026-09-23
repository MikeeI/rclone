# ISSUE-031 — archive: findFs scans every known archive for nested access

State: Investigating
Authorized-Work: Not-Selected
Publication-Target: Not-Selected
External-Reference: Not published.
Severity: Medium
Contribution-Priority: Medium
Root-Cause-Confidence: High
Finding-Category: Performance
Created: 2026-09-23
Updated: 2026-09-23
Source: `upstream/master@90e67915c88d4adf244f1d5251088c339c8b8e23`
Legacy-ID: year=2026; sequence=031

## Root-Cause

Root-Cause [S]: `findFs` scans the complete `archives` map for every nested `List` or `NewObject` lookup even though the map keys archives by path.

## Reach-and-Impact

Reach [S]: Nested archive lookups when multiple archives are known.
Impact [O]: Latest-beta runs took 0.23 seconds for 1,000 archives and 2.16 seconds for 5,000; `findFs` accounted for 72.1% CPU. Benchmark source and environment are not retained.

## Evidence

- [S] `backend/archive/archive.go:459-477` scans the complete `archives` map for every lookup; `List` calls it at `:494` and `NewObject` is the other recorded caller.
- [O] Legacy latest-beta benchmark took 0.23 seconds for 1,000 archives and 2.16 seconds for 5,000; `findFs` held 72.1% CPU; setup was not retained.

## Prior-Art

Coverage: current lookup source checked; benchmark reproduction and all prior-art channels not checked; checked=2026-09-23.
Gaps: Benchmark repeatability, current `NewObject` call path, and duplicate search remain unverified.

Contribution fit: Not assessed; publication target remains unselected.

## Proposed-Change

Walk requested-path ancestors to select the longest matching known archive, if path normalization and nested semantics support direct lookup.

## Scope-and-Constraints

- Preserve archive nesting, path normalization, and longest-prefix semantics.
- Exclude unrelated archive cache redesign.
- Benchmark repeatability and broader workload impact are unverified.

## Verification

- Reproduce timing and CPU profile against current source and compare nested lookup results.

## Publication-Blockers

Current source confirms map-wide lookup; benchmark reproduction, prior-art search, contribution fit, and user authorization remain unresolved.

## Next-Action

Summary: Reproduce archive-scale benchmark
Action: Reproduce timing and CPU profile across increasing archive counts.
Done-When: Repeatable workload, lookup equivalence, and current profile are recorded.
