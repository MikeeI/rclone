# ISSUE-003 — dropbox: deep shared-folder roots use a path as the folder name

State: Archived
Authorized-Work: Research-and-Reporting
Publication-Target: New-issue
External-Reference: https://github.com/rclone/rclone/issues/9705
Severity: Low
Contribution-Priority: Low
Root-Cause-Confidence: High
Finding-Category: Correctness
Created: 2026-09-23
Updated: 2026-09-23
Source: `upstream/master@90e67915c88d4adf244f1d5251088c339c8b8e23`
Legacy-ID: year=2026; sequence=003

## Root-Cause

Root-Cause [S]: `shared_folders` documentation says the first path component identifies the folder, but `NewFs` passes `path.Dir(f.root)` to a finder matching a single shared-folder `Name`.

## Reach-and-Impact

Reach [A]: Dropbox shared-folder remotes configured with a deep root.
Impact [A]: The path/name mismatch may prevent selecting the intended shared folder; no account reproduction is recorded.

## Evidence

- [S] Legacy ledger documents the mismatch between `NewFs`, `path.Dir(f.root)`, and the finder input.
- [S] Published report: https://github.com/rclone/rclone/issues/9705

## Prior-Art

Coverage: Current `upstream/master` shared-folder root parsing verified; full issue thread not rechecked; checked=2026-09-23.
Gaps: Account-based deep-root behavior and final discussion outcome remain unverified.

- https://github.com/rclone/rclone/issues/9705 — Same reported root cause.

Contribution fit: Existing issue — already reported; no duplicate publication proposed.

## Proposed-Change

Resolve the first component as the shared-folder name and retain the remaining path, if current behavior confirms the mismatch.

## Scope-and-Constraints

- Preserve shared-folder path semantics and behavior for existing roots.
- Exclude unrelated shared-folder lookup changes.
- User-visible failure is not reproduced.

## Verification

- No account-based reproduction is recorded.

## Publication-Blockers

Current source extracts the first root component as the shared-folder name before lookup.

## Next-Action

Summary: —
Action: None.
Done-When: None.

## Publication-Draft

Target: https://github.com/rclone/rclone/issues/9705
Title: Original title is not preserved in the legacy ledger.

Body:

```text
The legacy ledger did not retain the submitted body. The root-cause and evidence sections preserve the available summary; this is not a verbatim copy of the published text.
```

## Archive

Archive-Reason: Fixed-Elsewhere
Detail: Current upstream resolves the shared-folder name from the first path component.
Evidence: `backend/dropbox/dropbox.go:637-638,949-952` at `upstream/master@90e67915c88d4adf244f1d5251088c339c8b8e23`.
Checked: 2026-09-23
