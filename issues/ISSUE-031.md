# ISSUE-031 — archive: findFs scans every known archive for nested access

State: Investigating
Authorized-Work: Not-Selected
Publication-Target: Not-Selected
External-Reference: Not published.
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

- [S] Legacy ledger records complete-map scanning for both callers and path-keyed map ownership.
- [O] Latest-beta benchmark: 0.23 seconds at 1,000 archives; 2.16 seconds at 5,000; `findFs` held 72.1% CPU. Exact version, environment, and benchmark artifact are unavailable.

## Prior-Art

Coverage: no issue, PR, discussion, release, or forum search recorded; checked=2026-09-23.
Gaps: Current source, benchmark reproduction, and prior-art search are required.

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

Current source verification, benchmark artifact, prior-art search, contribution fit, and user authorization remain unresolved.

## Next-Action

Summary: Verify source currentness
Action: Reproduce the archive-cardinality benchmark on current `upstream/master`.
Done-When: Pinned source and repeatable benchmark confirm or invalidate the finding.
