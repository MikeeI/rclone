# ISSUE-015 — drive: permission cache mutex serializes metadata fetches

State: Submitted
Authorized-Work: Research-and-Reporting
Publication-Target: New-issue
External-Reference: https://github.com/rclone/rclone/issues/9682
Contribution-Priority: Low
Root-Cause-Confidence: High
Finding-Category: Performance
Created: 2026-09-23
Updated: 2026-09-23
Source: `upstream/master@90e67915c88d4adf244f1d5251088c339c8b8e23`
Legacy-ID: year=2026; sequence=015

## Root-Cause

Root-Cause [S]: `parseMetadata` launches bounded goroutines for permission IDs, but `getPermission` holds `permissionsMu` across the complete `Permissions.Get` API call.

## Reach-and-Impact

Reach [S]: Drive metadata parsing with multiple uncached permission IDs.
Impact [S]: Concurrent goroutines serialize at the API call; permission cardinality, request concurrency, and latency impact are unmeasured.

## Evidence

- [S] Legacy ledger records the goroutine fan-out and mutex scope.
- [S] Published report: https://github.com/rclone/rclone/issues/9682

## Prior-Art

Coverage: legacy record cites published issue only; current source and thread not rechecked; checked=2026-09-23.
Gaps: Current cache locking and final resolution are unverified.

- https://github.com/rclone/rclone/issues/9682 — Same reported root cause.

Contribution fit: Existing issue — already reported; no duplicate publication proposed.

## Proposed-Change

Limit mutex ownership to cache access and coordinate duplicate fetches without serializing independent API calls, if current source confirms the bottleneck.

## Scope-and-Constraints

- Preserve cache correctness and avoid duplicate same-ID requests.
- Exclude unrelated permission parsing changes.
- Latency impact is unmeasured.

## Verification

- No representative permission cardinality or latency measurement is recorded.

## Publication-Blockers

Verify current source, issue outcome, and concurrency behavior before further action.

## Next-Action

Summary: Verify source currentness
Action: Inspect current permission cache locking and the complete issue thread.
Done-When: Current locking and thread outcome are recorded with pinned evidence.

## Publication-Draft

Target: https://github.com/rclone/rclone/issues/9682
Title: Original title is not preserved in the legacy ledger.

Body:

```text
The legacy ledger did not retain the submitted body. The root-cause and evidence sections preserve the available summary; this is not a verbatim copy of the published text.
```
