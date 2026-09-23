# ISSUE-003 — dropbox: deep shared-folder roots use a path as the folder name

State: Submitted
Authorized-Work: Research-and-Reporting
Publication-Target: New-issue
External-Reference: https://github.com/rclone/rclone/issues/9705
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

Coverage: legacy record cites its published issue only; current thread and source not rechecked; checked=2026-09-23.
Gaps: Deep-root behavior and upstream resolution are unverified.

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

Verify current source, issue outcome, and a deep-root reproduction before further action.

## Next-Action

Summary: Verify source currentness
Action: Inspect current upstream lookup and the complete issue thread.
Done-When: Current behavior and thread outcome are recorded with pinned evidence.

## Publication-Draft

Target: https://github.com/rclone/rclone/issues/9705
Title: Original title is not preserved in the legacy ledger.

Body:

```text
The legacy ledger did not retain the submitted body. The root-cause and evidence sections preserve the available summary; this is not a verbatim copy of the published text.
```
