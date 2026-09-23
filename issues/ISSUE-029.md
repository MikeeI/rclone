# ISSUE-029 — local: List performs a redundant stat before opening each directory

State: Investigating
Authorized-Work: Not-Selected
Publication-Target: Not-Selected
External-Reference: Not published.
Severity: Low
Contribution-Priority: Low
Root-Cause-Confidence: High
Finding-Category: Performance
Created: 2026-09-23
Updated: 2026-09-23
Source: `upstream/master@90e67915c88d4adf244f1d5251088c339c8b8e23`
Legacy-ID: year=2026; sequence=029

## Root-Cause

Root-Cause [S]: Local `List` calls `os.Stat` and then `os.Open` for each successfully listed directory without using the stat result.

## Reach-and-Impact

Reach [O]: Linux syscall trace observed paired `newfstatat` and `openat` calls per directory in a recursive local listing.
Impact [S]: One redundant syscall per successfully listed directory is reproduced; representative wall-clock impact on large trees is unmeasured.

## Evidence

- [S] `backend/local/local.go:669` calls `os.Stat(fsDirPath)`, then `:675` calls `os.Open(fsDirPath)` without using stat metadata.
- [O] Legacy Linux trace observed paired `newfstatat` and `openat` calls per listed directory; trace artifact and command were not retained.

## Prior-Art

Coverage: current `List` source checked; trace reproduction and all prior-art channels not checked; checked=2026-09-23.
Gaps: Representative wall-clock impact and duplicate search are unverified.

Contribution fit: Not assessed; publication target remains unselected.

## Proposed-Change

Open the directory and obtain required metadata from the opened handle where compatible with existing platform behavior.

## Scope-and-Constraints

- Preserve symlink, permission, and platform-specific directory semantics.
- Exclude unrelated local listing changes.
- Representative runtime impact is unmeasured.

## Verification

- Reproduce the syscall trace against current upstream, then measure representative trees only if appropriate.

## Publication-Blockers

Current source confirms the redundant stat; trace artifact, runtime benefit, prior-art search, contribution fit, and user authorization remain unresolved.

## Next-Action

Summary: Measure directory-listing overhead
Action: Reproduce the paired syscalls and measure representative local directory trees.
Done-When: Pinned trace and bounded workload measurement are recorded.
