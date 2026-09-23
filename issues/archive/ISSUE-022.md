# ISSUE-022 — smb: failed connection setup paths do not close the TCP connection

State: Archived
Authorized-Work: Research-and-Reporting
Publication-Target: New-issue
External-Reference: https://github.com/rclone/rclone/issues/9678
Severity: Medium
Contribution-Priority: Low
Root-Cause-Confidence: High
Finding-Category: Reliability
Created: 2026-09-23
Updated: 2026-09-23
Source: `upstream/master@90e67915c88d4adf244f1d5251088c339c8b8e23`
Legacy-ID: year=2026; sequence=022

## Root-Cause

Root-Cause [S]: `Fs.dial` opens `tconn` before password decoding, Kerberos client creation, and SMB handshake; errors from `obscure.Reveal`, `GetClient`, or `DialConn` return without explicitly closing it.

## Reach-and-Impact

Reach [S]: SMB connection setup that fails after TCP dial.
Impact [S]: The path lacks explicit TCP cleanup; descriptor or connection growth was not measured.

## Evidence

- [S] Legacy ledger names `Fs.dial`, `tconn`, and all three setup error paths.
- [S] Published report: https://github.com/rclone/rclone/issues/9678

## Prior-Art

Coverage: legacy record cites its report only; current source and fix not rechecked; checked=2026-09-23.
Gaps: Current ownership and resolution are unverified.

- https://github.com/rclone/rclone/issues/9678 — Same reported root cause.

Contribution fit: Existing issue — report is closed as completed; no duplicate proposed.

## Proposed-Change

Close the owned TCP connection on every setup failure, if current code still leaves ownership outstanding.

## Scope-and-Constraints

- Preserve connection ownership transfer on successful setup.
- Exclude unrelated dialing changes.
- Resource growth is unmeasured.

## Verification

- No descriptor-growth or live connection measurement is recorded.

## Publication-Blockers

Inspect current cleanup paths and completion evidence before treating the issue as resolved.

## Next-Action

Summary: —
Action: None.
Done-When: None.

## Archive

Archive-Reason: Other
Detail: Prior report is closed as completed; exact upstream fix and resolution evidence have not been checked.
Evidence: https://github.com/rclone/rclone/issues/9678
Checked: 2026-09-23
