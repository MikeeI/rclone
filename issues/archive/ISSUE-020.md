# ISSUE-020 — smb: DirMove checks the destination using a different path representation

State: Archived
Authorized-Work: Research-and-Reporting
Publication-Target: New-issue
External-Reference: https://github.com/rclone/rclone/issues/9677
Contribution-Priority: Low
Root-Cause-Confidence: High
Finding-Category: Correctness
Created: 2026-09-23
Updated: 2026-09-23
Source: `upstream/master@90e67915c88d4adf244f1d5251088c339c8b8e23`
Legacy-ID: year=2026; sequence=020

## Root-Cause

Root-Cause [S]: `DirMove` checks `Stat(dstPath)` but renames using `f.toSambaPath(dstPath)`; SMB filename encoding can make the representations address different paths.

## Reach-and-Impact

Reach [A]: SMB `DirMove` destinations affected by filename encoding.
Impact [A]: The existence probe may address another path; no encoded-destination failure was reproduced.

## Evidence

- [S] Legacy ledger identifies the probe and rename path representations.
- [S] Published report: https://github.com/rclone/rclone/issues/9677

## Prior-Art

Coverage: legacy record cites its published issue only; current source and final resolution not rechecked; checked=2026-09-23.
Gaps: Encoded destination behavior and fix are unverified.

- https://github.com/rclone/rclone/issues/9677 — Same reported root cause.

Contribution fit: Existing issue — report is closed as completed; no duplicate proposed.

## Proposed-Change

Use one canonical Samba path representation for both probe and rename, if current source confirms the mismatch.

## Scope-and-Constraints

- Preserve SMB encoding rules and destination-exists handling.
- Exclude unrelated move semantics.
- Failure was not reproduced.

## Verification

- No encoded-destination server scenario is recorded.

## Publication-Blockers

Verify current source and completion evidence before treating the issue as resolved.

## Next-Action

Summary: —
Action: None.
Done-When: None.

## Archive

Archive-Reason: Other
Detail: Prior report is closed as completed; exact upstream fix and resolution evidence have not been checked.
Evidence: https://github.com/rclone/rclone/issues/9677
Checked: 2026-09-23
