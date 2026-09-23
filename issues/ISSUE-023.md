# ISSUE-023 — smb: Put can return nil when an upload error leaves the object behind

State: Submitted
Authorized-Work: Research-and-Reporting
Publication-Target: New-issue
External-Reference: https://github.com/rclone/rclone/issues/9679
Contribution-Priority: Low
Root-Cause-Confidence: High
Finding-Category: Correctness
Created: 2026-09-23
Updated: 2026-09-23
Source: `upstream/master@90e67915c88d4adf244f1d5251088c339c8b8e23`
Legacy-ID: year=2026; sequence=023

## Root-Cause

Root-Cause [S]: `Put` and `PutStream` return `nil, err` for every `Object.Update` failure, even when failed cleanup leaves the destination or `SetModTime` fails after upload completion.

## Reach-and-Impact

Reach [S]: SMB uploads that fail after destination creation or during post-upload modtime setting.
Impact [S]: The returned object can be nil while the destination remains; downstream retry and cleanup effects are not reproduced.

## Evidence

- [S] Legacy ledger identifies both return paths and destination persistence conditions.
- [S] Published report: https://github.com/rclone/rclone/issues/9679

## Prior-Art

Coverage: legacy record cites published issue only; current source and thread not rechecked; checked=2026-09-23.
Gaps: Current cleanup behavior and resolution are unverified.

- https://github.com/rclone/rclone/issues/9679 — Same reported root cause.

Contribution fit: Existing issue — already reported; no duplicate publication proposed.

## Proposed-Change

Return an object when it exists despite a post-creation error, consistent with the public `Put` contract, if current behavior confirms it.

## Scope-and-Constraints

- Preserve the original upload error and accurate destination state.
- Exclude unrelated SMB retry behavior.
- Downstream impact is not reproduced.

## Verification

- No consumer retry or cleanup scenario is recorded.

## Publication-Blockers

Verify current source, `Put` contract, and issue outcome before further action.

## Next-Action

Summary: Verify source currentness
Action: Inspect current SMB `Put`, cleanup, and modtime error paths.
Done-When: Current return behavior and thread outcome are recorded with pinned evidence.

## Publication-Draft

Target: https://github.com/rclone/rclone/issues/9679
Title: Original title is not preserved in the legacy ledger.

Body:

```text
The legacy ledger did not retain the submitted body. The root-cause and evidence sections preserve the available summary; this is not a verbatim copy of the published text.
```
