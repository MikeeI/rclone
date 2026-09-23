# ISSUE-009 — dropbox: direct lookup of an exported Paper file duplicates its extension

State: Archived
Authorized-Work: Research-and-Reporting
Publication-Target: New-issue
External-Reference: https://github.com/rclone/rclone/issues/9691
Severity: Medium
Contribution-Priority: Low
Root-Cause-Confidence: High
Finding-Category: Correctness
Created: 2026-09-23
Updated: 2026-09-23
Source: `upstream/master@90e67915c88d4adf244f1d5251088c339c8b8e23`
Legacy-ID: year=2026; sequence=009

## Root-Cause

Root-Cause [S]: `NewObject` retains the caller-visible export path while resolving metadata from a possible underlying Paper path; `setMetadataForExport` appends the selected extension to the suffixed name.

## Reach-and-Impact

Reach [A]: Direct lookup of an exported Dropbox Paper file.
Impact [S]: The resulting object name can contain the extension twice; downstream command and API effects are not reproduced.

## Evidence

- [S] Legacy ledger identifies the path retained by `NewObject` and extension append in `setMetadataForExport`.
- [S] Published report: https://github.com/rclone/rclone/issues/9691

## Prior-Art

Coverage: Current `upstream/master` export-path detection and metadata conversion verified; full issue thread not rechecked; checked=2026-09-23.
Gaps: Downstream Paper-object behavior and final discussion outcome remain unverified.

- https://github.com/rclone/rclone/issues/9691 — Same reported root cause.

Contribution fit: Existing issue — already reported; no duplicate publication proposed.

## Proposed-Change

Avoid appending an export extension to a path that already represents the exported name, if current source confirms it.

## Scope-and-Constraints

- Preserve Paper metadata lookup and canonical exported naming.
- Exclude unrelated export behavior.
- Downstream effects are unmeasured.

## Verification

- No command-level reproduction is recorded.

## Publication-Blockers

Current source carries `remoteIsExportPath` through lookup, trims the Paper suffix, and avoids appending a second export suffix.

## Next-Action

Summary: —
Action: None.
Done-When: None.

## Publication-Draft

Target: https://github.com/rclone/rclone/issues/9691
Title: Original title is not preserved in the legacy ledger.

Body:

```text
The legacy ledger did not retain the submitted body. The root-cause and evidence sections preserve the available summary; this is not a verbatim copy of the published text.
```

## Archive

Archive-Reason: Fixed-Elsewhere
Detail: Current upstream distinguishes existing export paths and does not append another export extension.
Evidence: `backend/dropbox/dropbox.go:779-780,852,1889-1913,1947-1951` at `upstream/master@90e67915c88d4adf244f1d5251088c339c8b8e23`.
Checked: 2026-09-23
