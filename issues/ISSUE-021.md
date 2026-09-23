# ISSUE-021 — smb: operation contexts do not cancel established SMB I/O

State: Submitted
Authorized-Work: Research-and-Reporting
Publication-Target: New-issue
External-Reference: https://github.com/rclone/rclone/issues/9708
Severity: Medium
Contribution-Priority: Low
Root-Cause-Confidence: High
Finding-Category: Reliability
Created: 2026-09-23
Updated: 2026-09-23
Source: `upstream/master@90e67915c88d4adf244f1d5251088c339c8b8e23`
Legacy-ID: year=2026; sequence=021

## Root-Cause

Root-Cause [S]: `go-smb2` uses `context.Background()` for established sessions and shares unless `WithContext` is called; rclone operation contexts currently affect connection setup but not later share I/O.

## Reach-and-Impact

Reach [S]: SMB I/O over established sessions and shares.
Impact [S]: Canceling the caller context does not stop later SMB I/O; no stuck-cancellation scenario is reproduced.

## Evidence

- [S] `backend/smb/connpool.go:126` states the context applies only to connection establishment; subsequent SMB session/share I/O has no operation-context wiring.
- [S] Published enhancement: https://github.com/rclone/rclone/issues/9708

## Prior-Art

Coverage: current connection setup path checked; dependency contract, complete issue thread, and broader prior art not rechecked; checked=2026-09-23.
Gaps: Runtime cancellation failure and issue resolution remain unverified.

- https://github.com/rclone/rclone/issues/9708 — Same reported enhancement.
- https://github.com/rclone/rclone/pull/8327 — Historical context behavior.

Contribution fit: Existing enhancement issue — already reported; no duplicate proposed.

## Proposed-Change

Pass caller context to established SMB sessions and shares where supported, after assessing lifecycle and shared-connection effects.

## Scope-and-Constraints

- Preserve pooled-session ownership and cancellation semantics for concurrent users.
- Exclude unrelated SMB protocol changes.
- Runtime blocking impact is unmeasured.

## Verification

- No stuck I/O cancellation reproduction is recorded.

## Publication-Blockers

Current source confirms the connection context boundary; runtime impact and full issue status remain unverified.

## Next-Action

Summary: Reproduce canceled SMB I/O
Action: Reproduce cancellation during established SMB I/O and inspect the dependency context API.
Done-When: Runtime behavior and current library contract are recorded.

## Publication-Draft

Target: https://github.com/rclone/rclone/issues/9708
Title: Original title is not preserved in the legacy ledger.

Body:

```text
The legacy ledger did not retain the submitted body. The root-cause and evidence sections preserve the available summary; this is not a verbatim copy of the published text.
```
