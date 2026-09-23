# ISSUE-011 — batcher: Commit can be admitted after the shutdown marker

State: Archived
Authorized-Work: Research-and-Reporting
Publication-Target: New-issue
External-Reference: https://github.com/rclone/rclone/issues/9687
Severity: Medium
Contribution-Priority: Low
Root-Cause-Confidence: High
Finding-Category: Reliability
Created: 2026-09-23
Updated: 2026-09-23
Source: `upstream/master@90e67915c88d4adf244f1d5251088c339c8b8e23`
Legacy-ID: year=2026; sequence=011

## Root-Cause

Root-Cause [S]: `Commit` checks `closed` separately from enqueueing its request; `Shutdown` can enqueue the quit marker between those operations, stranding an admitted commit behind it.

## Reach-and-Impact

Reach [A]: Concurrent `Commit` and `Shutdown` interleaving.
Impact [S]: A commit can remain unprocessed; runtime frequency and an observed blocked caller are not measured.

## Evidence

- [S] Legacy ledger records the separate check and channel send interleaving.
- [S] Published report: https://github.com/rclone/rclone/issues/9687

## Prior-Art

Coverage: Current `upstream/master` commit admission and shutdown ordering verified; full issue thread not rechecked; checked=2026-09-23.
Gaps: Runtime race frequency and final discussion outcome remain unverified.

- https://github.com/rclone/rclone/issues/9687 — Same reported root cause.

Contribution fit: Existing issue — already reported; no duplicate publication proposed.

## Proposed-Change

Synchronize admission and shutdown-marker ordering, if current source confirms the race.

## Scope-and-Constraints

- Preserve accepted-commit completion and shutdown semantics.
- Exclude unrelated batcher behavior.
- Runtime frequency is unmeasured.

## Verification

- No live blocked-caller reproduction is recorded.

## Publication-Blockers

Current `Commit` and `Shutdown` serialize admission and the shutdown marker with `admitMu`.

## Next-Action

Summary: —
Action: None.
Done-When: None.

## Publication-Draft

Target: https://github.com/rclone/rclone/issues/9687
Title: Original title is not preserved in the legacy ledger.

Body:

```text
The legacy ledger did not retain the submitted body. The root-cause and evidence sections preserve the available summary; this is not a verbatim copy of the published text.
```

## Archive

Archive-Reason: Fixed-Elsewhere
Detail: Current upstream serializes commit admission with the shutdown marker using `admitMu`.
Evidence: `lib/batcher/batcher.go:243-245,266-273` at `upstream/master@90e67915c88d4adf244f1d5251088c339c8b8e23`.
Checked: 2026-09-23
