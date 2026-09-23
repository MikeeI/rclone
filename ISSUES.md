# Issue and Pull Request Tracking

Read this index at the start of every agent session before repository work.
`FORMAT.md` owns research, lifecycle, drafting, implementation, severity, and publication rules.
Each linked issue record is authoritative for one root cause; `ISSUES.md` projects its current state.
This index owns the next unused canonical ID. Migrated IDs are retained in each record's `Legacy-ID` field.

Next finding ID: ISSUE-032

## Open-Findings

Open rows are ordered by descending `Severity`; the rating and evidence are in each linked record.

| ID | Finding | State | Authorized-Work | Publication-Target | Contribution-Priority | Next-Action | External-Reference |
| --- | --- | --- | --- | --- | --- | --- | --- |
| [ISSUE-031](issues/ISSUE-031.md) | archive: findFs scans every known archive for nested access | Investigating | Not-Selected | Not-Selected | Medium | Reproduce archive-scale benchmark | Not published. |
| [ISSUE-030](issues/ISSUE-030.md) | walk: excluded objects repeatedly scan parent directory entries | Investigating | Not-Selected | Not-Selected | Medium | Reproduce filtered ListR benchmark | Not published. |
| [ISSUE-023](issues/ISSUE-023.md) | smb: Put can return nil when an upload error leaves the object behind | Submitted | Research-and-Reporting | New-issue | Low | Reproduce persisted-object error | https://github.com/rclone/rclone/issues/9679 |
| [ISSUE-021](issues/ISSUE-021.md) | smb: operation contexts do not cancel established SMB I/O | Submitted | Research-and-Reporting | New-issue | Low | Reproduce canceled SMB I/O | https://github.com/rclone/rclone/issues/9708 |
| [ISSUE-025](issues/ISSUE-025.md) | smb: prefer matching-share connections before remounting | Submitted | Research-and-Reporting | Existing-pull-request-comment | Low | Inspect pooled-share contention | https://github.com/rclone/rclone/pull/9388#issuecomment-5108715075 |
| [ISSUE-029](issues/ISSUE-029.md) | local: List performs a redundant stat before opening each directory | Investigating | Not-Selected | Not-Selected | Low | Measure directory-listing overhead | Not published. |
| [ISSUE-016](issues/ISSUE-016.md) | drive: shortcut targets are resolved serially during listing | Submitted | Research-and-Reporting | New-issue | Low | Measure shortcut-listing cost | https://github.com/rclone/rclone/issues/9683 |

## Archived-Findings

| ID | Finding | Authorized-Work | Publication-Target | Contribution-Priority | Archive-Reason | External-Reference |
| --- | --- | --- | --- | --- | --- | --- |
| [ISSUE-001](issues/archive/ISSUE-001.md) | dropbox: avoid redundant metadata request in Rmdir | Research-and-Reporting | New-issue | Low | Other | https://github.com/rclone/rclone/issues/9663 |
| [ISSUE-002](issues/archive/ISSUE-002.md) | dropbox: known-size chunked uploads do not stop on early EOF | Research-and-Reporting | New-issue | Low | Fixed-Elsewhere | https://github.com/rclone/rclone/issues/9704 |
| [ISSUE-003](issues/archive/ISSUE-003.md) | dropbox: deep shared-folder roots use a path as the folder name | Research-and-Reporting | New-issue | Low | Fixed-Elsewhere | https://github.com/rclone/rclone/issues/9705 |
| [ISSUE-004](issues/archive/ISSUE-004.md) | dropbox: shared-mode lookup uses case-sensitive name matching | Research-and-Reporting | New-issue | Low | Fixed-Elsewhere | https://github.com/rclone/rclone/issues/9706 |
| [ISSUE-005](issues/archive/ISSUE-005.md) | dropbox: received shared-file names bypass standard encoding conversion | Research-and-Reporting | New-issue | Low | Fixed-Elsewhere | https://github.com/rclone/rclone/issues/9707 |
| [ISSUE-006](issues/archive/ISSUE-006.md) | dropbox: small batched uploads allocate a full chunk-size retry buffer | Research-and-Reporting | New-issue | Low | Other | https://github.com/rclone/rclone/issues/9685 |
| [ISSUE-007](issues/archive/ISSUE-007.md) | dropbox: known single-chunk uploads send an extra empty append request | Research-and-Reporting | New-issue | Low | Other | https://github.com/rclone/rclone/issues/9686 |
| [ISSUE-008](issues/archive/ISSUE-008.md) | dropbox: backend SDK calls do not propagate caller cancellation | Research-and-Reporting | New-issue | Low | Fixed-Elsewhere | https://github.com/rclone/rclone/issues/9688 |
| [ISSUE-009](issues/archive/ISSUE-009.md) | dropbox: direct lookup of an exported Paper file duplicates its extension | Research-and-Reporting | New-issue | Low | Fixed-Elsewhere | https://github.com/rclone/rclone/issues/9691 |
| [ISSUE-010](issues/archive/ISSUE-010.md) | dropbox: ChangeNotify root trimming assumes matching PathDisplay casing | Research-and-Reporting | New-issue | Low | Fixed-Elsewhere | https://github.com/rclone/rclone/issues/9692 |
| [ISSUE-011](issues/archive/ISSUE-011.md) | batcher: Commit can be admitted after the shutdown marker | Research-and-Reporting | New-issue | Low | Fixed-Elsewhere | https://github.com/rclone/rclone/issues/9687 |
| [ISSUE-012](issues/archive/ISSUE-012.md) | batcher: Commit ignores caller cancellation while waiting | Research-and-Reporting | New-issue | Low | Duplicate | https://github.com/rclone/rclone/issues/9690 |
| [ISSUE-013](issues/archive/ISSUE-013.md) | vfs: poll interval update can race with VFS shutdown | Research-and-Reporting | New-issue | Low | Fixed-Elsewhere | https://github.com/rclone/rclone/issues/9689 |
| [ISSUE-014](issues/archive/ISSUE-014.md) | drive: Rmdir lists trashed children when use_trash is enabled | Research-and-Reporting | New-issue | Low | Fixed-Elsewhere | https://github.com/rclone/rclone/issues/9681 |
| [ISSUE-015](issues/archive/ISSUE-015.md) | drive: permission cache mutex serializes metadata fetches | Research-and-Reporting | New-issue | Low | Fixed-Elsewhere | https://github.com/rclone/rclone/issues/9682 |
| [ISSUE-017](issues/archive/ISSUE-017.md) | drive: resumable uploads allocate a new chunk buffer per file | Research-and-Reporting | New-issue | Low | Fixed-Elsewhere | https://github.com/rclone/rclone/issues/9684 |
| [ISSUE-018](issues/archive/ISSUE-018.md) | smb: Kerberos client cache is recreated for every connection | Research-and-Reporting | New-issue | Low | Other | https://github.com/rclone/rclone/issues/9674 |
| [ISSUE-019](issues/archive/ISSUE-019.md) | smb: upload retains one connection while SetModTime acquires another | Research-and-Reporting | New-issue | Low | Fixed-Elsewhere | https://github.com/rclone/rclone/issues/9675 |
| [ISSUE-020](issues/archive/ISSUE-020.md) | smb: DirMove checks the destination using a different path representation | Research-and-Reporting | New-issue | Low | Other | https://github.com/rclone/rclone/issues/9677 |
| [ISSUE-022](issues/archive/ISSUE-022.md) | smb: failed connection setup paths do not close the TCP connection | Research-and-Reporting | New-issue | Low | Other | https://github.com/rclone/rclone/issues/9678 |
| [ISSUE-024](issues/archive/ISSUE-024.md) | smb: dead pooled connections can be discarded without closing the TCP transport | Research-and-Reporting | Existing-pull-request-comment | Low | Fixed-Elsewhere | https://github.com/rclone/rclone/pull/9388#issuecomment-5108852112 |
| [ISSUE-026](issues/archive/ISSUE-026.md) | smb: DirMove reports destination exists for unrelated Stat errors | Research-and-Reporting | New-issue | Low | Other | https://github.com/rclone/rclone/issues/9680 |
| [ISSUE-027](issues/archive/ISSUE-027.md) | copyurl: URL batches create a separate HTTP client per entry | Research-and-Reporting | Existing-issue-comment | Low | Other | https://github.com/rclone/rclone/issues/8127#issuecomment-5087488288 |
| [ISSUE-028](issues/archive/ISSUE-028.md) | lsjson: requested hashes reread local files | Research-and-Reporting | Existing-issue-comment | Medium | Other | https://github.com/rclone/rclone/issues/4181#issuecomment-5138225391 |
