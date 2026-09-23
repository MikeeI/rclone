# ISSUE-009 — dropbox: direct lookup of an exported Paper file duplicates its extension

State: Submitted
Authorized-Work: Research-and-Reporting
Publication-Target: New-issue
External-Reference: https://github.com/rclone/rclone/issues/9691
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

Coverage: legacy record cites its report only; current source and final thread not rechecked; checked=2026-09-23.
Gaps: Current naming behavior and resolution are unverified.

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

Verify current source, thread outcome, and object naming before further action.

## Next-Action

Summary: Verify source currentness
Action: Inspect current export-name flow and the complete issue thread.
Done-When: Current behavior and thread outcome are recorded with pinned evidence.

## Publication-Draft

Target: https://github.com/rclone/rclone/issues/9691
Title: Original title is not preserved in the legacy ledger.

Body:

```text
The legacy ledger did not retain the submitted body. The root-cause and evidence sections preserve the available summary; this is not a verbatim copy of the published text.
```
