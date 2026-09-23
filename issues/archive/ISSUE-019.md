# ISSUE-019 — smb: upload retains one connection while SetModTime acquires another

State: Archived
Authorized-Work: Research-and-Reporting
Publication-Target: New-issue
External-Reference: https://github.com/rclone/rclone/issues/9675
Severity: Low
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

Coverage: Current SMB upload and modtime connection lifetimes verified; full issue thread not rechecked; checked=2026-09-23.
Gaps: Connection-count and latency impact and final discussion outcome remain unmeasured.

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

Current `Update` returns the connection to the pool before calling `SetModTime`.

## Next-Action

Summary: —
Action: None.
Done-When: None.

## Publication-Draft

Target: https://github.com/rclone/rclone/issues/9675
Title: Original title is not preserved in the legacy ledger.

Body:

```text
The legacy ledger did not retain the submitted body. The root-cause and evidence sections preserve the available summary; this is not a verbatim copy of the published text.
```

## Archive

Archive-Reason: Fixed-Elsewhere
Detail: Current upstream returns the upload connection before the follow-up modtime call.
Evidence: `backend/smb/smb.go:889-898` at `upstream/master@90e67915c88d4adf244f1d5251088c339c8b8e23`.
Checked: 2026-09-23
