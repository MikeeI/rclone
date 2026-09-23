# ISSUE-001 — dropbox: avoid redundant metadata request in Rmdir

State: Archived
Authorized-Work: Research-and-Reporting
Publication-Target: New-issue
External-Reference: https://github.com/rclone/rclone/issues/9663
Contribution-Priority: Low
Root-Cause-Confidence: High
Finding-Category: Performance
Created: 2026-09-23
Updated: 2026-09-23
Source: `upstream/master@90e67915c88d4adf244f1d5251088c339c8b8e23`
Legacy-ID: year=2026; sequence=001

## Root-Cause

Root-Cause [S]: `Rmdir` performs `GetMetadata`, `ListFolder`, and `DeleteV2` for an empty directory; the metadata result is unused and `ListFolder` can identify missing and non-directory paths.

## Reach-and-Impact

Reach [S]: Dropbox empty-directory removal.
Impact [S]: One metadata request is redundant; error mapping and observable behavior were required to remain unchanged.

## Evidence

- [S] Legacy finding ledger identified the unused metadata result and the subsequent folder listing.
- [S] Historical report: https://github.com/rclone/rclone/issues/9663

## Prior-Art

Coverage: legacy ledger cites the published issue; current issue and source state not rechecked after rebase; checked=2026-09-23.
Gaps: Current upstream implementation and final issue resolution have not been revalidated.

- https://github.com/rclone/rclone/issues/9663 — Published report; same root cause.

Contribution fit: Existing issue — historical report is already published; no duplicate report is proposed.

## Proposed-Change

Remove only the unused metadata request if current source and error behavior confirm the original diagnosis.

## Scope-and-Constraints

- Preserve error mapping and observable `Rmdir` behavior.
- Exclude unrelated Dropbox request or listing changes.
- Cost: none measured.

## Verification

- Historical verification is not recorded.

## Publication-Blockers

Revalidate current upstream source and the issue's final outcome before further work.

## Next-Action

Summary: —
Action: None.
Done-When: None.

## Archive

Archive-Reason: Other
Detail: Prior report is closed as completed; exact upstream fix and resolution evidence have not been checked.
Evidence: https://github.com/rclone/rclone/issues/9663
Checked: 2026-09-23
