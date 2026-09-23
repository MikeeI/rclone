# ISSUE-014 — drive: Rmdir lists trashed children when use_trash is enabled

State: Submitted
Authorized-Work: Research-and-Reporting
Publication-Target: New-issue
External-Reference: https://github.com/rclone/rclone/issues/9681
Contribution-Priority: Low
Root-Cause-Confidence: High
Finding-Category: Performance
Created: 2026-09-23
Updated: 2026-09-23
Source: `upstream/master@90e67915c88d4adf244f1d5251088c339c8b8e23`
Legacy-ID: year=2026; sequence=014

## Root-Cause

Root-Cause [S]: `purgeCheck` lists with `includeAll=true`, so the Drive API returns trashed children even when `UseTrash` makes only non-trashed children relevant to directory removal.

## Reach-and-Impact

Reach [S]: Drive `Rmdir` with `UseTrash` enabled.
Impact [S]: The query includes irrelevant trashed children; request count, latency, and trashed-child cardinality are unmeasured.

## Evidence

- [S] Legacy ledger identifies `purgeCheck`, `includeAll=true`, and the `UseTrash` condition.
- [S] Published report: https://github.com/rclone/rclone/issues/9681

## Prior-Art

Coverage: legacy record cites published issue only; current source and thread not rechecked; checked=2026-09-23.
Gaps: Current query and final resolution are unverified.

- https://github.com/rclone/rclone/issues/9681 — Same reported root cause.

Contribution fit: Existing issue — already reported; no duplicate publication proposed.

## Proposed-Change

Exclude trashed children from the emptiness check when `UseTrash` is enabled, if current behavior confirms this.

## Scope-and-Constraints

- Preserve non-trash child detection and `Rmdir` semantics.
- Exclude unrelated Drive listing changes.
- Request and latency impact are unmeasured.

## Verification

- No request-level or integration measurement is recorded.

## Publication-Blockers

Verify current source, issue outcome, and Drive query semantics before further action.

## Next-Action

Summary: Verify source currentness
Action: Inspect current `purgeCheck` and the complete issue thread.
Done-When: Current query behavior and thread outcome are recorded with pinned evidence.

## Publication-Draft

Target: https://github.com/rclone/rclone/issues/9681
Title: Original title is not preserved in the legacy ledger.

Body:

```text
The legacy ledger did not retain the submitted body. The root-cause and evidence sections preserve the available summary; this is not a verbatim copy of the published text.
```
