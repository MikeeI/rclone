# ISSUE-019 — smb: upload retains one connection while SetModTime acquires another

State: Submitted
Authorized-Work: Research-and-Reporting
Publication-Target: New-issue
External-Reference: https://github.com/rclone/rclone/issues/9675
Contribution-Priority: Low
Root-Cause-Confidence: High
Finding-Category: Performance
Created: 2026-09-23
Updated: 2026-09-23
Source: `upstream/master@90e67915c88d4adf244f1d5251088c339c8b8e23`
Legacy-ID: year=2026; sequence=019

## Root-Cause

Root-Cause [S]: `Object.Update` retains the upload connection through deferred return after closing the file; subsequent `SetModTime` separately acquires a connection for `Chtimes` and `Stat`.

## Reach-and-Impact

Reach [S]: SMB uploads followed by modtime setting.
Impact [S]: Two connections have overlapping lifetimes; connection/session counts and latency are unmeasured.

## Evidence

- [S] Legacy ledger records the retained upload connection and separate `SetModTime` acquisition.
- [S] Published report: https://github.com/rclone/rclone/issues/9675

## Prior-Art

Coverage: legacy record cites published issue only; current source and thread not rechecked; checked=2026-09-23.
Gaps: Current connection ownership and resolution are unverified.

- https://github.com/rclone/rclone/issues/9675 — Same reported root cause.

Contribution fit: Existing issue — already reported; no duplicate publication proposed.

## Proposed-Change

Release the upload connection before follow-up metadata work or reuse it safely, if current pool semantics permit.

## Scope-and-Constraints

- Preserve upload completion, file-close ordering, and connection-pool ownership.
- Exclude unrelated SMB connection pooling.
- Connection and latency impact are unmeasured.

## Verification

- No connection-count or timing measurement is recorded.

## Publication-Blockers

Verify current source, pool ownership, and issue outcome before further action.

## Next-Action

Summary: Verify source currentness
Action: Inspect current upload and `SetModTime` connection lifetimes.
Done-When: Current ownership and thread outcome are recorded with pinned evidence.

## Publication-Draft

Target: https://github.com/rclone/rclone/issues/9675
Title: Original title is not preserved in the legacy ledger.

Body:

```text
The legacy ledger did not retain the submitted body. The root-cause and evidence sections preserve the available summary; this is not a verbatim copy of the published text.
```
