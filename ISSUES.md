# Issue Publication Status

This file is the sole source of truth for every finding's ID, lifecycle status, and published location.
Read and update status here instead of inferring it from chat history or earlier search output.

## Dropbox Issues

`CP4` is split into `CP4a` and `CP4b` because they have independent root causes.

### #9663 — dropbox: avoid redundant metadata request in Rmdir

- `Rmdir` currently performs `GetMetadata`, `ListFolder`, and `DeleteV2` when removing an empty directory.
- The metadata result is unused, while `ListFolder` can also identify missing paths and non-directory paths.
- The report asks whether the redundant request can be removed without changing error mapping or observable behavior.

Status: Published as an open GitHub issue.
Location: [rclone/rclone#9663](https://github.com/rclone/rclone/issues/9663)

### P1 — dropbox: known-size chunked uploads do not stop on early EOF

- `uploadChunked` rejects readers that exceed their declared size but does not detect a known-size reader ending early.
- Early EOF can finalize at a shorter offset or repeatedly append empty chunks, depending on the declared remainder.
- A realistic size-inconsistent source path and the resulting Dropbox behavior have not been reproduced.

Status: Hold; not published.
Location: Not published.

### P2 — dropbox: deep shared-folder roots use a path as the folder name

- The `shared_folders` documentation says the first path component identifies the shared folder.
- `NewFs` instead passes `path.Dir(f.root)` to a finder that compares it with a single shared-folder `Name`.
- The source mismatch is established, but no deep-root failure has been reproduced against a Dropbox account.

Status: Hold; not published.
Location: Not published.

### CP4a — dropbox: shared-mode lookup uses case-sensitive name matching

- The Dropbox backend advertises `CaseInsensitive: true` for its filesystem behavior.
- `findSharedFolder` and `findSharedFile` nevertheless compare requested and returned names with exact equality.
- No shared folder or received shared file has been tested with a case-only difference in its requested name.

Status: Hold; not published.
Location: Not published.

### CP4b — dropbox: received shared-file names bypass standard encoding conversion

- `listSharedFolders` applies `ToStandardName`, while `listReceivedFiles` stores the API-provided `Name` directly.
- `findSharedFile` then compares that raw remote name with the requested rclone name across the encoding boundary.
- No received shared file whose name requires conversion has been used to reproduce a listing or lookup failure.

Status: Hold; not published.
Location: Not published.

## SMB Issues

This section tracks the SMB findings, their publication state, and their external location when published.

### #9674 — smb: Kerberos client cache is recreated for every connection

- The SMB dial path creates a new `KerberosFactory` for every Kerberos connection.
- Its client, error, and ccache-mtime caches are instance-local and are discarded after one `GetClient` call.
- Repeated parsing and client construction are source-proven; authentication latency and KDC requests are not measured.

Status: Published as an open GitHub issue.
Location: [rclone/rclone#9674](https://github.com/rclone/rclone/issues/9674)

### #9675 — smb: upload retains one connection while SetModTime acquires another

- `Object.Update` retains the upload connection until its deferred return after the file has been closed.
- The following `SetModTime` call acquires a separate connection for `Chtimes` and `Stat`.
- The overlapping lifetime is source-proven; connection-count, session-count, and latency impact are not measured.

Status: Published as an open GitHub issue.
Location: [rclone/rclone#9675](https://github.com/rclone/rclone/issues/9675)

### P1 — smb: DirMove checks the destination using a different path representation

- `DirMove` calls `Stat(dstPath)` before renaming with `f.toSambaPath(dstPath)`.
- The probe and rename can address different paths when SMB filename encoding transforms the destination.
- The mismatch is source-proven, but no encoded-destination failure has been reproduced against an SMB server.

Status: Drafted as an enhancement issue; not published.
Location: Not published.

### P2 — smb: operation contexts do not cancel established SMB I/O

- `go-smb2` uses `context.Background()` for established sessions and shares unless `WithContext` is called.
- SMB operation contexts currently affect connection setup but do not cancel later share I/O.
- The limitation is documented in merged PR #8327; no stuck cancellation scenario has been reproduced.

Status: Hold; not published.
Location: Not published.

### P3 — smb: failed connection setup paths do not close the TCP connection

- `Fs.dial` opens `tconn` before password decoding, Kerberos client creation, and the SMB handshake.
- Errors from `obscure.Reveal`, `GetClient`, or `DialConn` return without explicitly closing the caller-owned connection.
- The ownership gap is source-proven; file-descriptor or connection growth has not been measured.

Status: Drafted as an enhancement issue; not published.
Location: Not published.

### P4 — smb: Put can return nil when an upload error leaves the object behind

- `Put` and `PutStream` return `nil, err` for every `Object.Update` failure.
- The destination can remain after failed cleanup or a `SetModTime` error following a completed upload.
- The return-contract mismatch is source-proven; downstream retry and cleanup effects have not been reproduced.

Status: Drafted as an enhancement issue; not published.
Location: Not published.

### P5 — smb: dead pooled connections can be discarded without closing the TCP transport

- Open PR #9388 changes the exact connection-pool and file-pool discard lifecycle.
- Its current dead-connection paths discard references without explicitly closing the caller-owned TCP transport.
- The ownership gap is source-proven; transport-resource growth has not been measured.

Status: Published as a comment on open GitHub pull request #9388.
Location: [rclone/rclone#9388 P1 comment](https://github.com/rclone/rclone/pull/9388#issuecomment-5108852112)

### PR #9388 P4 comment — smb: prefer matching-share connections before remounting

- `getConnection` removes the FIFO head without first looking for a later connection with the requested `shareName`.
- It calls `mountShare` while holding `poolMu`, so a share change may perform `Umount` and `Mount` under the lock.
- The flow is source-proven; mixed-share remount counts and latency impact have not been measured.

Status: Published as a comment on open GitHub pull request #9388.
Location: [rclone/rclone#9388 P4 comment](https://github.com/rclone/rclone/pull/9388#issuecomment-5108715075)

### P6 — smb: DirMove reports destination exists for unrelated Stat errors

- `DirMove` performs the rename only when the destination `Stat` returns `os.IsNotExist`.
- A successful `Stat` and every other error both produce `fs.ErrorDirExists`.
- The mapping is source-proven, but permission and transport failure cases have not been reproduced.

Status: Drafted as an enhancement issue; not published.
Location: Not published.

## Command Comments

This section tracks published command-related comments and their external location.

### #8127 comment — Support --files-from for copyUrl command

- The comment identifies a separate HTTP client and transport for every CSV entry processed by `copyurl --urls`.
- It proposes one batch-owned client so completed transfers to the same host can reuse pooled connections.
- It asks whether that design would be acceptable as a small follow-up to the implementation from PR #8810.

Status: Published as a comment on a closed GitHub issue.
Location: [rclone/rclone#8127 comment](https://github.com/rclone/rclone/issues/8127#issuecomment-5087488288)
Format note: Its follow-up PR offer conflicts with the current [FORMAT.md](FORMAT.md) prohibition on PR offers.
