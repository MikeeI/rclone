# ISSUE-025 — smb: prefer matching-share connections before remounting

State: Submitted
Authorized-Work: Research-and-Reporting
Publication-Target: Existing-pull-request-comment
External-Reference: https://github.com/rclone/rclone/pull/9388#issuecomment-5108715075
Severity: Medium
Contribution-Priority: Low
Root-Cause-Confidence: High
Finding-Category: Performance
Created: 2026-09-23
Updated: 2026-09-23
Source: `upstream/master@90e67915c88d4adf244f1d5251088c339c8b8e23`
Legacy-ID: year=2026; sequence=025

## Root-Cause

Root-Cause [S]: `getConnection` removes the FIFO head without first searching for a later matching `shareName`; `mountShare` runs under `poolMu` and may perform `Umount` and `Mount` while locked.

## Reach-and-Impact

Reach [A]: Mixed-share SMB connection pools with a requested share available behind the FIFO head.
Impact [S]: Avoidable remount work and lock-held network operations may occur; remount counts and latency are unmeasured.

## Evidence

- [S] `backend/smb/connpool.go:168-178` holds `poolMu` while selecting only the FIFO head and calling `mountShare`.
- [S] `mountShare:148-159` may perform `Umount` and `Mount` while that pool lock remains held.
- [S] Published comment: https://github.com/rclone/rclone/pull/9388#issuecomment-5108715075

## Prior-Art

Coverage: current pool and mount source checked; complete PR diff/thread and broader prior art not rechecked; checked=2026-09-23.
Gaps: Pool wait, remount count, PR state, and maintainer response remain unknown.

- https://github.com/rclone/rclone/pull/9388 — Active diff was the reported context.

Contribution fit: Existing pull request — the comment owns the pool-selection concern; don't duplicate it.

## Proposed-Change

Select a matching-share connection before remounting and avoid holding `poolMu` across mount I/O, if current design confirms this is safe.

## Scope-and-Constraints

- Preserve FIFO fairness where required and connection exclusivity.
- Exclude unrelated pool changes.
- Remount counts and latency are unmeasured.

## Verification

- No mixed-share remount measurement is recorded.

## Publication-Blockers

Current source confirms FIFO-head remounting under the shared pool lock; contention magnitude and current PR status remain unverified.

## Next-Action

Summary: Inspect pooled-share contention
Action: Measure pool wait and remount behavior with mixed-share concurrent requests.
Done-When: Representative contention results and current PR outcome are recorded.

## Publication-Draft

Target: https://github.com/rclone/rclone/pull/9388
Title: Original comment text is not preserved in the legacy ledger.

Body:

```text
The legacy ledger did not retain the submitted comment body. The root-cause and evidence sections preserve the available summary; this is not a verbatim copy of the published comment.
```
