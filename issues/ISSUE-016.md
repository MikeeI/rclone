# ISSUE-016 — drive: shortcut targets are resolved serially during listing

State: Submitted
Authorized-Work: Research-and-Reporting
Publication-Target: New-issue
External-Reference: https://github.com/rclone/rclone/issues/9683
Severity: Low
Contribution-Priority: Low
Root-Cause-Confidence: High
Finding-Category: Performance
Created: 2026-09-23
Updated: 2026-09-23
Source: `upstream/master@90e67915c88d4adf244f1d5251088c339c8b8e23`
Legacy-ID: year=2026; sequence=016

## Root-Cause

Root-Cause [S]: The Drive listing loop calls `resolveShortcut` inline, and each shortcut target resolution makes a separate `Files.Get` request before processing the next item.

## Reach-and-Impact

Reach [S]: Drive listings containing shortcuts.
Impact [S]: Target requests are serial in the listing loop; shortcut density, listing latency, and pacer impact are unmeasured.

## Evidence

- [S] `backend/drive/drive.go:1168` calls `resolveShortcut` inline during listing; `resolveShortcut:2422` fetches target metadata before the next item.
- [S] Published report: https://github.com/rclone/rclone/issues/9683

## Prior-Art

Coverage: current `upstream/master` source checked; complete issue thread and broader prior-art search not rechecked; checked=2026-09-23.
Gaps: Shortcut-density impact and issue resolution remain unverified.

- https://github.com/rclone/rclone/issues/9683 — Same reported root cause.

Contribution fit: Existing issue — already reported; no duplicate publication proposed.

## Proposed-Change

Resolve shortcut targets concurrently or through a supported batch API, if current source and compatibility constraints justify it.

## Scope-and-Constraints

- Preserve listing order, error behavior, and API pacing.
- Exclude unrelated Drive listing work.
- Performance impact is unmeasured.

## Verification

- No shortcut-density or latency measurement is recorded.

## Publication-Blockers

Current source confirms serial target lookup; the complete issue thread and representative shortcut workload remain unverified.

## Next-Action

Summary: Measure shortcut-listing cost
Action: Measure request and listing-time cost across representative shortcut counts.
Done-When: Repeatable workload and current issue outcome are recorded.

## Publication-Draft

Target: https://github.com/rclone/rclone/issues/9683
Title: Original title is not preserved in the legacy ledger.

Body:

```text
The legacy ledger did not retain the submitted body. The root-cause and evidence sections preserve the available summary; this is not a verbatim copy of the published text.
```
