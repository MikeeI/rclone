# ISSUE-017 — drive: resumable uploads allocate a new chunk buffer per file

State: Submitted
Authorized-Work: Research-and-Reporting
Publication-Target: New-issue
External-Reference: https://github.com/rclone/rclone/issues/9684
Contribution-Priority: Low
Root-Cause-Confidence: High
Finding-Category: Performance
Created: 2026-09-23
Updated: 2026-09-23
Source: `upstream/master@90e67915c88d4adf244f1d5251088c339c8b8e23`
Legacy-ID: year=2026; sequence=017

## Root-Cause

Root-Cause [S]: `resumableUpload.Upload` allocates a full `chunk_size` buffer for each chunked upload and reuses it only for that file.

## Reach-and-Impact

Reach [S]: Drive chunked uploads across multiple files.
Impact [S]: The per-upload allocation is source-proven; allocation volume, GC cost, and throughput impact are unmeasured.

## Evidence

- [S] Legacy ledger identifies the allocation in `resumableUpload.Upload` and per-file reuse.
- [S] Published report: https://github.com/rclone/rclone/issues/9684

## Prior-Art

Coverage: legacy record cites published issue only; current source and thread not rechecked; checked=2026-09-23.
Gaps: Current buffer ownership and final resolution are unverified.

- https://github.com/rclone/rclone/issues/9684 — Same reported root cause.

Contribution fit: Existing issue — already reported; no duplicate publication proposed.

## Proposed-Change

Reuse bounded upload buffers across compatible sequential uploads, if ownership and concurrency remain safe in current code.

## Scope-and-Constraints

- Preserve concurrent upload isolation and buffer lifetime safety.
- Exclude unmeasured pooling abstractions.
- GC and throughput effects are unmeasured.

## Verification

- No allocation or throughput measurement is recorded.

## Publication-Blockers

Verify current source, ownership constraints, and issue outcome before further action.

## Next-Action

Summary: Verify source currentness
Action: Inspect current upload buffer allocation and the complete issue thread.
Done-When: Current ownership and thread outcome are recorded with pinned evidence.

## Publication-Draft

Target: https://github.com/rclone/rclone/issues/9684
Title: Original title is not preserved in the legacy ledger.

Body:

```text
The legacy ledger did not retain the submitted body. The root-cause and evidence sections preserve the available summary; this is not a verbatim copy of the published text.
```
