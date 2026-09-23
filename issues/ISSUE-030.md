# ISSUE-030 — walk: excluded objects repeatedly scan parent directory entries

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
Legacy-ID: year=2026; sequence=030

## Root-Cause

Root-Cause [S]: Filtered `ListR` calls `DirTree.Find` for each excluded object whose parent must remain visible; `DirTree.Find` linearly scans that parent's entries.

## Reach-and-Impact

Reach [S]: Filtered listings with many excluded objects sharing a parent.
Impact [O]: An isolated benchmark took 4.95 seconds for 50,000 missing root lookups across 50,000 retained entries; workload realism and end-to-end impact are unmeasured.

## Evidence

- [S] `fs/walk/walk.go:507` calls `dirs.Find` for each excluded object's parent; `fs/dirtree/dirtree.go:72-80` linearly scans that parent's entries.
- [O] Legacy isolated benchmark took 4.95 seconds for 50,000 missing root lookups across 50,000 retained entries; benchmark source is unavailable.

## Prior-Art

Coverage: current walker and `DirTree.Find` source checked; benchmark reproduction and all prior-art channels not checked; checked=2026-09-23.
Gaps: Workload representativeness, end-to-end impact, and duplicate search remain unverified.

Contribution fit: Not assessed; publication target remains unselected.

## Proposed-Change

Avoid rescanning the same parent entry set for every excluded object, if current source and representative workloads confirm the cost.

## Scope-and-Constraints

- Preserve visible-parent construction and filtered listing output.
- Exclude unrelated tree-walker redesign.
- Benchmark representativeness and production impact are unmeasured.

## Verification

- Reproduce the isolated benchmark against current source and confirm output equivalence.

## Publication-Blockers

Current source confirms repeated linear parent lookups; benchmark reproduction, prior-art search, contribution fit, and user authorization remain unresolved.

## Next-Action

Summary: Reproduce filtered ListR benchmark
Action: Reproduce the repeated-parent workload and compare a representative filtered `ListR`.
Done-When: Benchmark source, output equivalence, and representative timing are recorded.
