# ISSUE-024 — smb: dead pooled connections can be discarded without closing the TCP transport

State: Archived
Authorized-Work: Research-and-Reporting
Publication-Target: Existing-pull-request-comment
External-Reference: https://github.com/rclone/rclone/pull/9388#issuecomment-5108852112
Severity: Low
Contribution-Priority: Low
Root-Cause-Confidence: High
Finding-Category: Reliability
Created: 2026-09-23
Updated: 2026-09-23
Source: `upstream/master@90e67915c88d4adf244f1d5251088c339c8b8e23`
Legacy-ID: year=2026; sequence=024

## Root-Cause

Root-Cause [S]: The legacy comment reports that open PR #9388's dead-connection paths discard connection references without explicitly closing the caller-owned TCP transport.

## Reach-and-Impact

Reach [A]: Dead SMB pooled connections on paths changed by PR #9388.
Impact [S]: TCP ownership may be lost without transport close; resource growth is unmeasured.

## Evidence

- [S] Legacy ledger identifies the connection-pool and file-pool discard lifecycle in the PR diff.
- [S] Published comment: https://github.com/rclone/rclone/pull/9388#issuecomment-5108852112

## Prior-Art

Coverage: Current SMB dead-connection discard path verified; full PR #9388 diff and comment thread not rechecked; checked=2026-09-23.
Gaps: Original PR lifecycle and maintainer response are unknown.

- https://github.com/rclone/rclone/pull/9388 — Active diff was the reported context.

Contribution fit: Existing pull request — the comment owns the reported lifecycle concern; don't duplicate it.

## Proposed-Change

Close the owned transport when dead pooled connections are discarded, if the current PR diff confirms the ownership gap.

## Scope-and-Constraints

- Preserve caller ownership transfer and pooled connection lifecycle.
- Exclude unrelated pool redesign.
- Resource impact is unmeasured.

## Verification

- No transport-growth measurement is recorded.

## Publication-Blockers

Current `putConnection` closes a connection when the SMB `Echo` health check fails.

## Next-Action

Summary: —
Action: None.
Done-When: None.

## Publication-Draft

Target: https://github.com/rclone/rclone/pull/9388
Title: Original comment text is not preserved in the legacy ledger.

Body:

```text
The legacy ledger did not retain the submitted comment body. The root-cause and evidence sections preserve the available summary; this is not a verbatim copy of the published comment.
```

## Archive

Archive-Reason: Fixed-Elsewhere
Detail: Current upstream closes the failed pooled connection before discarding it.
Evidence: `backend/smb/connpool.go:207-215` at `upstream/master@90e67915c88d4adf244f1d5251088c339c8b8e23`.
Checked: 2026-09-23
