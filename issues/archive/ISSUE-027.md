# ISSUE-027 — copyurl: URL batches create a separate HTTP client per entry

State: Archived
Authorized-Work: Research-and-Reporting
Publication-Target: Existing-issue-comment
External-Reference: https://github.com/rclone/rclone/issues/8127#issuecomment-5087488288
Contribution-Priority: Low
Root-Cause-Confidence: High
Finding-Category: Performance
Created: 2026-09-23
Updated: 2026-09-23
Source: `upstream/master@90e67915c88d4adf244f1d5251088c339c8b8e23`
Legacy-ID: year=2026; sequence=027

## Root-Cause

Root-Cause [S]: The legacy comment reports that `copyurl --urls` creates a separate HTTP client and transport per CSV entry rather than reusing a batch-owned client.

## Reach-and-Impact

Reach [A]: Multi-entry `copyurl --urls` batches transferring to the same host.
Impact [A]: Connections may not be reused across entries; performance effect is unmeasured.

## Evidence

- [S] Legacy record describes a batch-owned client as a possible follow-up to PR #8810.
- [S] Published comment: https://github.com/rclone/rclone/issues/8127#issuecomment-5087488288

## Prior-Art

Coverage: legacy record cites one comment on closed issue #8127; current source and thread not rechecked; checked=2026-09-23.
Gaps: Current client ownership, issue outcome, and follow-up status are unknown.

- https://github.com/rclone/rclone/issues/8127 — Closed issue containing the reported comment.

Contribution fit: Existing issue comment — report is already posted; do not cross-post.

## Proposed-Change

No follow-up authorized; inspect current implementation and thread only if the user reopens this finding.

## Scope-and-Constraints

- Preserve per-request authentication, headers, and transport isolation.
- Exclude client reuse without batch ownership and lifecycle evidence.
- Latency impact is unmeasured.

## Verification

- No connection reuse or timing measurement is recorded.

## Publication-Blockers

Closed discussion and source currentness have not been rechecked; no current action is established.

## Next-Action

Summary: —
Action: None.
Done-When: None.

## Archive

Archive-Reason: Other
Detail: Legacy record states the comment is on a closed issue; follow-up PR offer conflicts with current FORMAT.md policy, and no resolution evidence is recorded.
Evidence: https://github.com/rclone/rclone/issues/8127#issuecomment-5087488288
Checked: 2026-09-23
