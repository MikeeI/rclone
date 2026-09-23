# ISSUE-016 — drive: shortcut targets are resolved serially during listing

State: Submitted
Authorized-Work: Research-and-Reporting
Publication-Target: New-issue
External-Reference: https://github.com/rclone/rclone/issues/9683
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

- [S] Legacy ledger identifies the inline call and separate `Files.Get` request.
- [S] Published report: https://github.com/rclone/rclone/issues/9683

## Prior-Art

Coverage: legacy record cites published issue only; current source and thread not rechecked; checked=2026-09-23.
Gaps: Current listing flow and final resolution are unverified.

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

Verify current source, API constraints, and issue outcome before further action.

## Next-Action

Summary: Verify source currentness
Action: Inspect current shortcut listing flow and the complete issue thread.
Done-When: Current request sequence and thread outcome are recorded with pinned evidence.

## Publication-Draft

Target: https://github.com/rclone/rclone/issues/9683
Title: Original title is not preserved in the legacy ledger.

Body:

```text
The legacy ledger did not retain the submitted body. The root-cause and evidence sections preserve the available summary; this is not a verbatim copy of the published text.
```
