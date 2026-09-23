# ISSUE-008 — dropbox: backend SDK calls do not propagate caller cancellation

State: Submitted
Authorized-Work: Research-and-Reporting
Publication-Target: New-issue
External-Reference: https://github.com/rclone/rclone/issues/9688
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

Coverage: legacy record cites its published issue only; current source and thread not rechecked; checked=2026-09-23.
Gaps: Current SDK surface, callers, and resolution are unverified.

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

Verify current source, SDK API, and issue outcome before further action.

## Next-Action

Summary: Verify source currentness
Action: Inspect current request wrappers and the complete issue thread.
Done-When: Current context flow and thread outcome are recorded with pinned evidence.

## Publication-Draft

Target: https://github.com/rclone/rclone/issues/9688
Title: Original title is not preserved in the legacy ledger.

Body:

```text
The legacy ledger did not retain the submitted body. The root-cause and evidence sections preserve the available summary; this is not a verbatim copy of the published text.
```
