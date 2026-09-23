# ISSUE-028 — lsjson: requested hashes reread local files

State: Archived
Authorized-Work: Research-and-Reporting
Publication-Target: Existing-issue-comment
External-Reference: https://github.com/rclone/rclone/issues/4181#issuecomment-5138225391
Contribution-Priority: Medium
Root-Cause-Confidence: High
Finding-Category: Performance
Created: 2026-09-23
Updated: 2026-09-23
Source: `upstream/master@90e67915c88d4adf244f1d5251088c339c8b8e23`
Legacy-ID: year=2026; sequence=028

## Root-Cause

Root-Cause [S]: `ListJSON` calls `Object.Hash` separately for each requested type; local backend reads the file again for each uncached hash.

## Reach-and-Impact

Reach [O]: Measurements report one, four, and thirteen requested hashes produce the same counts of full file-read passes.
Impact [O]: More requested uncached hashes cause proportionally more file reads in the measured scenario; command, file-size, and environment details are not retained here.

## Evidence

- [S] Legacy ledger records separate `Object.Hash` calls and local uncached reads.
- [O] Legacy measurements: 1, 4, and 13 requested hashes matched 1, 4, and 13 full read passes; measurement setup/output was not retained.
- [S] Published comment: https://github.com/rclone/rclone/issues/4181#issuecomment-5138225391

## Prior-Art

Coverage: legacy record cites comment on a closed issue; measurement artifact and current source not rechecked; checked=2026-09-23.
Gaps: Reproduction details, issue outcome, and current implementation are unavailable.

- https://github.com/rclone/rclone/issues/4181 — Closed issue containing the reported comment.

Contribution fit: Existing issue comment — report is already posted; do not cross-post.

## Proposed-Change

Compute requested local hashes in one file pass if the current API and cache ownership support it.

## Scope-and-Constraints

- Preserve requested hash results, caching, and error behavior.
- Exclude unrequested hashes and unrelated local I/O changes.
- Broader performance impact is not quantified.

## Verification

- Historical hash-count measurements lack command, version, and environment details.

## Publication-Blockers

Reproduction details and current thread/source status are missing; no current follow-up action is established.

## Next-Action

Summary: —
Action: None.
Done-When: None.

## Archive

Archive-Reason: Other
Detail: Legacy record says the comment is on a closed issue; measured effect is preserved, but resolution and reproducibility are not established.
Evidence: https://github.com/rclone/rclone/issues/4181#issuecomment-5138225391
Checked: 2026-09-23
