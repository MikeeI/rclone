# ISSUE-005 — dropbox: received shared-file names bypass standard encoding conversion

State: Archived
Authorized-Work: Research-and-Reporting
Publication-Target: New-issue
External-Reference: https://github.com/rclone/rclone/issues/9707
Severity: Medium
Contribution-Priority: Low
Root-Cause-Confidence: High
Finding-Category: Correctness
Created: 2026-09-23
Updated: 2026-09-23
Source: `upstream/master@90e67915c88d4adf244f1d5251088c339c8b8e23`
Legacy-ID: year=2026; sequence=005

## Root-Cause

Root-Cause [S]: `listSharedFolders` applies `ToStandardName`, but `listReceivedFiles` stores the API `Name` directly; `findSharedFile` compares it across the encoding boundary.

## Reach-and-Impact

Reach [A]: Received shared files whose names require standard-name conversion.
Impact [A]: Listing or lookup may fail for such names; no affected file was reproduced.

## Evidence

- [S] Legacy ledger records the differing conversion paths and lookup comparison.
- [S] Published report: https://github.com/rclone/rclone/issues/9707

## Prior-Art

Coverage: Current `upstream/master` received-file conversion verified; full issue thread not rechecked; checked=2026-09-23.
Gaps: Conversion-sensitive account reproduction and final discussion outcome remain unverified.

- https://github.com/rclone/rclone/issues/9707 — Same reported root cause.

Contribution fit: Existing issue — already reported; no duplicate publication proposed.

## Proposed-Change

Apply the standard-name conversion consistently before matching received shared files, if current source confirms the gap.

## Scope-and-Constraints

- Preserve Dropbox API names and existing encoding behavior elsewhere.
- Exclude unrelated shared-file lookup changes.
- User-visible failure is not reproduced.

## Verification

- No conversion-sensitive received file reproduction is recorded.

## Publication-Blockers

Current listing converts received file names with `ToStandardName` before matching.

## Next-Action

Summary: —
Action: None.
Done-When: None.

## Publication-Draft

Target: https://github.com/rclone/rclone/issues/9707
Title: Original title is not preserved in the legacy ledger.

Body:

```text
The legacy ledger did not retain the submitted body. The root-cause and evidence sections preserve the available summary; this is not a verbatim copy of the published text.
```

## Archive

Archive-Reason: Fixed-Elsewhere
Detail: Current upstream converts received shared-file names into standard encoding before exposing entries.
Evidence: `backend/dropbox/dropbox.go:1016-1019` at `upstream/master@90e67915c88d4adf244f1d5251088c339c8b8e23`.
Checked: 2026-09-23
