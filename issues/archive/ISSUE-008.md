# ISSUE-008 — dropbox: backend SDK calls do not propagate caller cancellation

State: Archived
Authorized-Work: Research-and-Reporting
Publication-Target: New-issue
External-Reference: https://github.com/rclone/rclone/issues/9688
Severity: Medium
Contribution-Priority: Low
Root-Cause-Confidence: High
Finding-Category: Reliability
Created: 2026-09-23
Updated: 2026-09-23
Source: `upstream/master@90e67915c88d4adf244f1d5251088c339c8b8e23`
Legacy-ID: year=2026; sequence=008

## Root-Cause

Root-Cause [S]: Dropbox client fields use non-context SDK interfaces; wrappers issue requests with `context.Background()` although backend methods receive caller contexts.

## Reach-and-Impact

Reach [S]: In-flight Dropbox SDK requests made by backend methods with caller contexts.
Impact [S]: Caller cancellation does not stop an active request; shutdown delay and transfer impact are unmeasured.

## Evidence

- [S] Legacy ledger records non-context interfaces and background request contexts.
- [S] Published report: https://github.com/rclone/rclone/issues/9688

## Prior-Art

Coverage: Current `upstream/master` Dropbox client construction and request context use verified; full issue thread not rechecked; checked=2026-09-23.
Gaps: Cancellation timing and final discussion outcome remain unverified.

- https://github.com/rclone/rclone/issues/9688 — Same reported root cause.

Contribution fit: Existing issue — already reported; no duplicate publication proposed.

## Proposed-Change

Propagate operation contexts through Dropbox SDK calls where supported, if current API and source confirm the gap.

## Scope-and-Constraints

- Preserve request retries, authentication, and operation cancellation semantics.
- Exclude unrelated client replacement.
- Runtime delay is unmeasured.

## Verification

- No in-flight cancellation scenario is recorded.

## Publication-Blockers

Current Dropbox SDK clients are context-capable and request wrappers pass caller contexts.

## Next-Action

Summary: —
Action: None.
Done-When: None.

## Publication-Draft

Target: https://github.com/rclone/rclone/issues/9688
Title: Original title is not preserved in the legacy ledger.

Body:

```text
The legacy ledger did not retain the submitted body. The root-cause and evidence sections preserve the available summary; this is not a verbatim copy of the published text.
```

## Archive

Archive-Reason: Fixed-Elsewhere
Detail: Current upstream constructs context-capable SDK clients and propagates request contexts.
Evidence: `backend/dropbox/dropbox.go:391-395,607-610,1730-1754` at `upstream/master@90e67915c88d4adf244f1d5251088c339c8b8e23`.
Checked: 2026-09-23
