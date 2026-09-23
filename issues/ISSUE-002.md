# ISSUE-002 — dropbox: known-size chunked uploads do not stop on early EOF

State: Submitted
Authorized-Work: Research-and-Reporting
Publication-Target: New-issue
External-Reference: https://github.com/rclone/rclone/issues/9704
Contribution-Priority: Low
Root-Cause-Confidence: High
Finding-Category: Reliability
Created: 2026-09-23
Updated: 2026-09-23
Source: `upstream/master@90e67915c88d4adf244f1d5251088c339c8b8e23`
Legacy-ID: year=2026; sequence=002

## Root-Cause

Root-Cause [S]: `uploadChunked` rejects readers larger than the declared size but does not detect known-size early EOF; it may finalize a shorter offset or append empty chunks.

## Reach-and-Impact

Reach [S]: Dropbox chunked uploads from sources whose actual length is below their declared positive size.
Impact [S]: Upload termination can disagree with the declared size; no realistic inconsistent source path or resulting Dropbox behavior was reproduced.

## Evidence

- [S] Legacy ledger source finding names `uploadChunked` and the two early-EOF outcomes.
- [S] Published report: https://github.com/rclone/rclone/issues/9704

## Prior-Art

Coverage: legacy record contains one published issue; current source and related work not rechecked; checked=2026-09-23.
Gaps: Current source, final thread, and realistic reproduction are unverified.

- https://github.com/rclone/rclone/issues/9704 — Same reported root cause.

Contribution fit: Existing issue — already reported; no duplicate publication proposed.

## Proposed-Change

Detect and return a suitable error on premature EOF for known-size input, if current source confirms the gap.

## Scope-and-Constraints

- Preserve unknown-size upload behavior and Dropbox session semantics.
- Exclude unrelated upload refactoring.
- User impact is not reproduced.

## Verification

- Historical reproduction is not recorded.

## Publication-Blockers

Verify current upstream source and thread state before taking further action.

## Next-Action

Summary: Verify source currentness
Action: Inspect current upstream `uploadChunked` and the complete issue thread.
Done-When: Current behavior and thread outcome are recorded with pinned evidence.

## Publication-Draft

Target: https://github.com/rclone/rclone/issues/9704
Title: Original title is not preserved in the legacy ledger.

Body:

```text
The legacy ledger did not retain the submitted body. The root-cause and evidence sections preserve the available summary; this is not a verbatim copy of the published text.
```
