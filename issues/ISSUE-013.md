# ISSUE-013 — vfs: poll interval update can race with VFS shutdown

State: Submitted
Authorized-Work: Research-and-Reporting
Publication-Target: New-issue
External-Reference: https://github.com/rclone/rclone/issues/9689
Contribution-Priority: Low
Root-Cause-Confidence: High
Finding-Category: Reliability
Created: 2026-09-23
Updated: 2026-09-23
Source: `upstream/master@90e67915c88d4adf244f1d5251088c339c8b8e23`
Legacy-ID: year=2026; sequence=013

## Root-Cause

Root-Cause [S]: The `vfs/poll-interval` handler checks `pollChan` and later sends without lifecycle-spanning synchronization; `VFS.Shutdown` can close and clear the channel between them.

## Reach-and-Impact

Reach [A]: Concurrent poll-interval update and VFS shutdown.
Impact [S]: A send-on-closed-channel panic is source-proven; live panic and runtime frequency are unmeasured.

## Evidence

- [S] Legacy ledger records the check/send and close/clear interleaving.
- [S] Published report: https://github.com/rclone/rclone/issues/9689

## Prior-Art

Coverage: legacy record cites published issue only; current source and thread not rechecked; checked=2026-09-23.
Gaps: Current lifecycle and runtime reproduction are unverified.

- https://github.com/rclone/rclone/issues/9689 — Same reported root cause.

Contribution fit: Existing issue — already reported; no duplicate publication proposed.

## Proposed-Change

Serialize poll interval updates with shutdown channel lifecycle, if current source confirms the race.

## Scope-and-Constraints

- Preserve poll interval update behavior and shutdown completion.
- Exclude unrelated VFS lifecycle changes.
- Runtime panic frequency is unknown.

## Verification

- No live panic reproduction is recorded.

## Publication-Blockers

Verify current source, issue outcome, and a lifecycle race reproduction before further action.

## Next-Action

Summary: Verify source currentness
Action: Inspect current poll interval update and VFS shutdown synchronization.
Done-When: Current lifecycle and issue outcome are recorded with pinned evidence.

## Publication-Draft

Target: https://github.com/rclone/rclone/issues/9689
Title: Original title is not preserved in the legacy ledger.

Body:

```text
The legacy ledger did not retain the submitted body. The root-cause and evidence sections preserve the available summary; this is not a verbatim copy of the published text.
```
