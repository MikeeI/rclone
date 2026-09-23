# ISSUE-012 — batcher: Commit ignores caller cancellation while waiting

State: Archived
Authorized-Work: Research-and-Reporting
Publication-Target: New-issue
External-Reference: https://github.com/rclone/rclone/issues/9690
Severity: Medium
Contribution-Priority: Low
Root-Cause-Confidence: High
Finding-Category: Reliability
Created: 2026-09-23
Updated: 2026-09-23
Source: `upstream/master@90e67915c88d4adf244f1d5251088c339c8b8e23`
Legacy-ID: year=2026; sequence=012

## Root-Cause

Root-Cause [S]: `Commit` accepts a caller context but uses unconditional channel operations for request admission and synchronous response waiting.

## Reach-and-Impact

Reach [S]: Callers whose context is canceled while waiting in `Commit`.
Impact [S]: The caller cannot return promptly on cancellation; blocked duration and teardown impact are unmeasured.

## Evidence

- [S] Historical issue #7025 proposed selecting on `ctx.Done()`; PR #7026 fixed a different signaling condition.
- [S] Published report: https://github.com/rclone/rclone/issues/9690

## Prior-Art

Coverage: legacy record cites historical issue #7025, PR #7026, and duplicate report; current final thread not rechecked; checked=2026-09-23.
Gaps: Current source and duplicate issue disposition are unverified.

- https://github.com/rclone/rclone/issues/9690 — Duplicate of #7025 according to the legacy record.
- https://github.com/rclone/rclone/issues/7025 — Related historical issue.

Contribution fit: Existing issue #7025 owns the same cancellation concern; do not publish a duplicate.

## Proposed-Change

No separate contribution proposed; retain the duplicate relationship and validate current status if reopened.

## Scope-and-Constraints

- Preserve the original issue identity and duplicate relationship.
- Exclude a second report for the same cause.
- Runtime delay is unmeasured.

## Verification

- No blocked-caller runtime scenario is recorded.

## Publication-Blockers

Current source and the complete canonical issue thread have not been checked.

## Next-Action

Summary: —
Action: None.
Done-When: None.

## Archive

Archive-Reason: Duplicate
Detail: Legacy ledger records issue #9690 as a closed duplicate of #7025.
Evidence: https://github.com/rclone/rclone/issues/9690
Checked: 2026-09-23
