# ISSUE-007 — dropbox: known single-chunk uploads send an extra empty append request

State: Archived
Authorized-Work: Research-and-Reporting
Publication-Target: New-issue
External-Reference: https://github.com/rclone/rclone/issues/9686
Severity: Low
Contribution-Priority: Low
Root-Cause-Confidence: High
Finding-Category: Performance
Created: 2026-09-23
Updated: 2026-09-23
Source: `upstream/master@90e67915c88d4adf244f1d5251088c339c8b8e23`
Legacy-ID: year=2026; sequence=007

## Root-Cause

Root-Cause [S]: A known positive upload of at most one chunk is first appended with `Close=false`, then the loop sends an empty `UploadSessionAppendV2` request with `Close=true`.

## Reach-and-Impact

Reach [S]: Known-size positive Dropbox uploads no larger than one chunk.
Impact [S]: An extra data-transport request is source-proven; latency and rate-limit impact are unmeasured.

## Evidence

- [S] Legacy ledger records the first append and empty final append sequence.
- [S] Published report: https://github.com/rclone/rclone/issues/9686

## Prior-Art

Coverage: legacy ledger cites its report only; current source and final resolution not rechecked; checked=2026-09-23.
Gaps: Current request sequence and fix are unverified.

- https://github.com/rclone/rclone/issues/9686 — Same reported root cause.

Contribution fit: Existing issue — report is closed as completed; no duplicate proposed.

## Proposed-Change

Finalize the known single-chunk session on its data append, if current source confirms the extra call.

## Scope-and-Constraints

- Preserve Dropbox session finalization and data integrity.
- Exclude unrelated chunked-upload changes.
- Network impact remains unmeasured.

## Verification

- Historical request-level verification is not recorded.

## Publication-Blockers

Revalidate current implementation and issue completion evidence before any follow-up.

## Next-Action

Summary: —
Action: None.
Done-When: None.

## Archive

Archive-Reason: Other
Detail: Prior report is closed as completed; exact upstream fix and resolution evidence have not been checked.
Evidence: https://github.com/rclone/rclone/issues/9686
Checked: 2026-09-23
