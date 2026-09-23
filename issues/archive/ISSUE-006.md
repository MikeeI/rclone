# ISSUE-006 — dropbox: small batched uploads allocate a full chunk-size retry buffer

State: Archived
Authorized-Work: Research-and-Reporting
Publication-Target: New-issue
External-Reference: https://github.com/rclone/rclone/issues/9685
Contribution-Priority: Low
Root-Cause-Confidence: High
Finding-Category: Performance
Created: 2026-09-23
Updated: 2026-09-23
Source: `upstream/master@90e67915c88d4adf244f1d5251088c339c8b8e23`
Legacy-ID: year=2026; sequence=006

## Root-Cause

Root-Cause [S]: Default synchronous batching routes known small files through `uploadChunked`, which allocates a configured 48 MiB buffer without considering source size.

## Reach-and-Impact

Reach [S]: Small known-size Dropbox uploads using default synchronous batching.
Impact [S]: Full-size allocation is established; peak RSS, GC, and upload-time impact were not measured.

## Evidence

- [S] Legacy ledger records the routing and 48 MiB allocation.
- [S] Published report: https://github.com/rclone/rclone/issues/9685

## Prior-Art

Coverage: legacy ledger cites its report only; current source and final resolution not rechecked; checked=2026-09-23.
Gaps: Current allocation behavior and fix are unverified.

- https://github.com/rclone/rclone/issues/9685 — Same reported root cause.

Contribution fit: Existing issue — report is closed as completed; no duplicate proposed.

## Proposed-Change

Bound the retry buffer by known source size when safe, if current source confirms the allocation remains.

## Scope-and-Constraints

- Preserve chunked-upload retry behavior and unknown-size handling.
- Exclude general buffer pooling.
- Runtime impact remains unmeasured.

## Verification

- Historical allocation measurement is not recorded.

## Publication-Blockers

Revalidate current implementation and the issue's completion evidence before any follow-up.

## Next-Action

Summary: —
Action: None.
Done-When: None.

## Archive

Archive-Reason: Other
Detail: Prior report is closed as completed; exact upstream fix and resolution evidence have not been checked.
Evidence: https://github.com/rclone/rclone/issues/9685
Checked: 2026-09-23
