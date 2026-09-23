# ISSUE-026 — smb: DirMove reports destination exists for unrelated Stat errors

State: Archived
Authorized-Work: Research-and-Reporting
Publication-Target: New-issue
External-Reference: https://github.com/rclone/rclone/issues/9680
Severity: Medium
Contribution-Priority: Low
Root-Cause-Confidence: High
Finding-Category: Correctness
Created: 2026-09-23
Updated: 2026-09-23
Source: `upstream/master@90e67915c88d4adf244f1d5251088c339c8b8e23`
Legacy-ID: year=2026; sequence=026

## Root-Cause

Root-Cause [S]: `DirMove` performs the rename only when destination `Stat` returns `os.IsNotExist`; success and every other error become `fs.ErrorDirExists`.

## Reach-and-Impact

Reach [S]: SMB directory moves where destination `Stat` succeeds or fails for a reason other than not-exist.
Impact [S]: Unrelated errors are mapped to destination-exists; permission and transport failures were not reproduced.

## Evidence

- [S] Legacy ledger records the `os.IsNotExist` gate and fallback error mapping.
- [S] Published report: https://github.com/rclone/rclone/issues/9680

## Prior-Art

Coverage: legacy report cited only; current source and completed resolution not rechecked; checked=2026-09-23.
Gaps: Current mapping and fix are unverified.

- https://github.com/rclone/rclone/issues/9680 — Same reported root cause.

Contribution fit: Existing issue — report is closed as completed; no duplicate proposed.

## Proposed-Change

Propagate unrelated `Stat` errors and report destination-exists only when the destination exists, if current code confirms the defect.

## Scope-and-Constraints

- Preserve not-exist rename behavior and destination-exists semantics.
- Exclude unrelated SMB move changes.
- Runtime failures were not reproduced.

## Verification

- No permission or transport failure reproduction is recorded.

## Publication-Blockers

Inspect current mapping and completion evidence before treating the issue as resolved.

## Next-Action

Summary: —
Action: None.
Done-When: None.

## Archive

Archive-Reason: Other
Detail: Prior report is closed as completed; exact upstream fix and resolution evidence have not been checked.
Evidence: https://github.com/rclone/rclone/issues/9680
Checked: 2026-09-23
