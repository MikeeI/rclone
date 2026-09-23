# ISSUE-025 — smb: prefer matching-share connections before remounting

State: Submitted
Authorized-Work: Research-and-Reporting
Publication-Target: Existing-pull-request-comment
External-Reference: https://github.com/rclone/rclone/pull/9388#issuecomment-5108715075
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

- [S] Legacy ledger identifies FIFO selection and the lock scope around share mounting.
- [S] Published comment: https://github.com/rclone/rclone/pull/9388#issuecomment-5108715075

## Prior-Art

Coverage: legacy record cites one comment on PR #9388; current diff and thread not rechecked; checked=2026-09-23.
Gaps: PR state, current changed paths, and maintainer response are unknown.

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

Recheck PR state, current diff, and comment response before further action.

## Next-Action

Summary: Verify thread currentness
Action: Inspect PR #9388's current diff and the complete comment thread.
Done-When: Current pool flow and thread outcome are recorded with pinned evidence.

## Publication-Draft

Target: https://github.com/rclone/rclone/pull/9388
Title: Original comment text is not preserved in the legacy ledger.

Body:

```text
The legacy ledger did not retain the submitted comment body. The root-cause and evidence sections preserve the available summary; this is not a verbatim copy of the published comment.
```
