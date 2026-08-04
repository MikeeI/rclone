# Issue Publication Status

This file is the sole source of truth for every finding's ID, lifecycle status, and published location.
Read and update status here instead of inferring it from chat history or earlier search output.

Next finding ID: ISSUE-2026-032

## Dropbox Issues

### ISSUE-2026-001 — dropbox: avoid redundant metadata request in Rmdir

- `Rmdir` currently performs `GetMetadata`, `ListFolder`, and `DeleteV2` when removing an empty directory.
- The metadata result is unused, while `ListFolder` can also identify missing paths and non-directory paths.
- The report asks whether the redundant request can be removed without changing error mapping or observable behavior.

Status: Published as a closed GitHub issue (completed).
Location: [rclone/rclone#9663](https://github.com/rclone/rclone/issues/9663)

### ISSUE-2026-002 — dropbox: known-size chunked uploads do not stop on early EOF

- `uploadChunked` rejects readers that exceed their declared size but does not detect a known-size reader ending early.
- Early EOF can finalize at a shorter offset or repeatedly append empty chunks, depending on the declared remainder.
- A realistic size-inconsistent source path and the resulting Dropbox behavior have not been reproduced.

Status: Published as an open GitHub issue.
Location: [rclone/rclone#9704](https://github.com/rclone/rclone/issues/9704)

### ISSUE-2026-003 — dropbox: deep shared-folder roots use a path as the folder name

- The `shared_folders` documentation says the first path component identifies the shared folder.
- `NewFs` instead passes `path.Dir(f.root)` to a finder that compares it with a single shared-folder `Name`.
- The source mismatch is established, but no deep-root failure has been reproduced against a Dropbox account.

Status: Published as an open GitHub issue.
Location: [rclone/rclone#9705](https://github.com/rclone/rclone/issues/9705)

### ISSUE-2026-004 — dropbox: shared-mode lookup uses case-sensitive name matching

- The Dropbox backend advertises `CaseInsensitive: true` for its filesystem behavior.
- `findSharedFolder` and `findSharedFile` nevertheless compare requested and returned names with exact equality.
- No shared folder or received shared file has been tested with a case-only difference in its requested name.

Status: Published as an open GitHub issue.
Location: [rclone/rclone#9706](https://github.com/rclone/rclone/issues/9706)

### ISSUE-2026-005 — dropbox: received shared-file names bypass standard encoding conversion

- `listSharedFolders` applies `ToStandardName`, while `listReceivedFiles` stores the API-provided `Name` directly.
- `findSharedFile` then compares that raw remote name with the requested rclone name across the encoding boundary.
- No received shared file whose name requires conversion has been used to reproduce a listing or lookup failure.

Status: Published as an open GitHub issue.
Location: [rclone/rclone#9707](https://github.com/rclone/rclone/issues/9707)

### ISSUE-2026-006 — dropbox: small batched uploads allocate a full chunk-size retry buffer

- Default synchronous batching routes known small files through `uploadChunked`.
- `uploadChunked` allocates the configured 48 MiB chunk buffer without considering the known source size.
- The heap allocation is source-proven; peak RSS, garbage collection, and upload-time impact are not measured.

Status: Published as a closed GitHub issue (completed).
Location: [rclone/rclone#9685](https://github.com/rclone/rclone/issues/9685)

### ISSUE-2026-007 — dropbox: known single-chunk uploads send an extra empty append request

- A known positive size up to one chunk is first appended with `Close=false`.
- The following loop iteration sends an empty `UploadSessionAppendV2` call with `Close=true`.
- The extra data-transport call is source-proven; latency and rate-limit impact are not measured.

Status: Published as a closed GitHub issue (completed).
Location: [rclone/rclone#9686](https://github.com/rclone/rclone/issues/9686)

### ISSUE-2026-008 — dropbox: backend SDK calls do not propagate caller cancellation

- Dropbox client fields use non-context SDK interfaces even though backend methods receive a caller context.
- The SDK wrappers run requests with `context.Background()`, while rclone checks cancellation only after each call returns.
- Missing in-flight cancellation is source-proven; shutdown delay and transfer impact are not measured.

Status: Published as an open GitHub issue.
Location: [rclone/rclone#9688](https://github.com/rclone/rclone/issues/9688)

### ISSUE-2026-009 — dropbox: direct lookup of an exported Paper file duplicates its extension

- `NewObject` preserves the caller-visible export path while resolving metadata from a possible underlying Paper path.
- `setMetadataForExport` then appends the selected export extension to that already suffixed remote name.
- The duplicate object name is source-proven; downstream command and API effects have not been reproduced.

Status: Published as an open GitHub issue.
Location: [rclone/rclone#9691](https://github.com/rclone/rclone/issues/9691)

### ISSUE-2026-010 — dropbox: ChangeNotify root trimming assumes matching PathDisplay casing

- `changeNotifyRunner` removes the configured root from `PathDisplay` with case-sensitive `strings.TrimPrefix`.
- The pinned Dropbox SDK documents that `PathDisplay` casing may not match the user's filesystem in rare instances.
- The relative-path mismatch is source-proven; resulting VFS cache behavior has not been reproduced.

Status: Published as an open GitHub issue.
Location: [rclone/rclone#9692](https://github.com/rclone/rclone/issues/9692)

## Local Issues

This section tracks findings in the local filesystem backend.

### ISSUE-2026-029 — local: List performs a redundant stat before opening each directory

- `List` calls `os.Stat` and then `os.Open` for every successfully listed directory without using the stat metadata.
- A Linux syscall trace confirmed paired `newfstatat` and `openat` calls for each directory in a recursive local listing.
- The redundant syscall is reproduced, but representative wall-clock impact across large directory trees is not measured.

Status: Not published.
Location: Not published.

## Core Issues

This section tracks findings in backend-independent core packages.

### ISSUE-2026-011 — batcher: Commit can be admitted after the shutdown marker

- `Commit` checks `closed` separately from sending its request to the batcher's input channel.
- `Shutdown` can enqueue the quit request between those operations, leaving the admitted commit behind the marker.
- The interleaving is source-proven; runtime frequency and a live blocked caller have not been measured.

Status: Published as an open GitHub issue.
Location: [rclone/rclone#9687](https://github.com/rclone/rclone/issues/9687)

### ISSUE-2026-012 — batcher: Commit ignores caller cancellation while waiting

- `Commit` accepts a caller context but uses unconditional channel operations for admission and synchronous response waiting.
- Historical issue #7025 proposed selecting on `ctx.Done()`, while PR #7026 fixed a different signaling condition.
- Missing cancellation is source-proven; blocked-caller duration and teardown impact have not been measured.

Status: Published as a closed GitHub issue (duplicate of #7025).
Location: [rclone/rclone#9690](https://github.com/rclone/rclone/issues/9690)

### ISSUE-2026-030 — walk: excluded objects repeatedly scan parent directory entries

- The filtered `ListR` path calls `DirTree.Find` for every excluded object whose parent directory must remain visible.
- `DirTree.Find` linearly scans the parent's entries, repeating the scan for excluded objects that share a parent.
- An isolated benchmark took 4.95 seconds for 50,000 missing root lookups across 50,000 retained entries.

Status: Not published.
Location: Not published.

## Archive Issues

This section tracks findings in the archive backend.

### ISSUE-2026-031 — archive: findFs scans every known archive for nested access

- `findFs` scans the complete `archives` map for every `List` or `NewObject` lookup below a known archive.
- The map already keys archives by path, so walking the requested path's ancestors can select the longest match directly.
- Latest-beta runs took 0.23 seconds for 1,000 archives and 2.16 seconds for 5,000; `findFs` held 72.1% CPU.

Status: Not published.
Location: Not published.

## VFS Issues

This section tracks findings in the virtual filesystem layer.

### ISSUE-2026-013 — vfs: poll interval update can race with VFS shutdown

- The `vfs/poll-interval` handler checks `pollChan` and later sends to it without lifecycle-spanning synchronization.
- `VFS.Shutdown` can close and clear the same channel between the handler's check and send.
- The send-on-closed-channel race is source-proven; a live panic and its runtime frequency have not been measured.

Status: Published as an open GitHub issue.
Location: [rclone/rclone#9689](https://github.com/rclone/rclone/issues/9689)

## Drive Issues

This section tracks the Google Drive findings, their publication state, and their external location when published.

### ISSUE-2026-014 — drive: Rmdir lists trashed children when use_trash is enabled

- `purgeCheck` lists the directory with `includeAll=true`, causing the Drive API to return trashed children.
- When `UseTrash` is enabled, only non-trashed child existence affects whether `Rmdir` may remove the directory.
- The full listing is source-proven; request count, listing latency, and trashed-child cardinality are not measured.

Status: Published as an open GitHub issue.
Location: [rclone/rclone#9681](https://github.com/rclone/rclone/issues/9681)

### ISSUE-2026-015 — drive: permission cache mutex serializes metadata fetches

- `parseMetadata` starts one bounded goroutine for every permission ID that needs fetching.
- `getPermission` holds `permissionsMu` across the complete `Permissions.Get` API call, serializing those goroutines.
- The serialization is source-proven; permission cardinality, request concurrency, and latency impact are not measured.

Status: Published as an open GitHub issue.
Location: [rclone/rclone#9682](https://github.com/rclone/rclone/issues/9682)

### ISSUE-2026-016 — drive: shortcut targets are resolved serially during listing

- The Drive listing loop calls `resolveShortcut` inline before processing the next listed item.
- `resolveShortcut` performs a separate `Files.Get` request for each shortcut target.
- The request chain is source-proven; shortcut density, listing latency, and pacer impact are not measured.

Status: Published as an open GitHub issue.
Location: [rclone/rclone#9683](https://github.com/rclone/rclone/issues/9683)

### ISSUE-2026-017 — drive: resumable uploads allocate a new chunk buffer per file

- `resumableUpload.Upload` allocates a full `chunk_size` byte slice when each chunked upload starts.
- The buffer is reused for that file's chunks but is not reused by later uploads.
- Allocation reuse is source-proven; allocation volume, GC cost, and throughput impact are not measured.

Status: Published as an open GitHub issue.
Location: [rclone/rclone#9684](https://github.com/rclone/rclone/issues/9684)

## SMB Issues

This section tracks the SMB findings, their publication state, and their external location when published.

### ISSUE-2026-018 — smb: Kerberos client cache is recreated for every connection

- The SMB dial path creates a new `KerberosFactory` for every Kerberos connection.
- Its client, error, and ccache-mtime caches are instance-local and are discarded after one `GetClient` call.
- Repeated parsing and client construction are source-proven; authentication latency and KDC requests are not measured.

Status: Published as a closed GitHub issue (completed).
Location: [rclone/rclone#9674](https://github.com/rclone/rclone/issues/9674)

### ISSUE-2026-019 — smb: upload retains one connection while SetModTime acquires another

- `Object.Update` retains the upload connection until its deferred return after the file has been closed.
- The following `SetModTime` call acquires a separate connection for `Chtimes` and `Stat`.
- The overlapping lifetime is source-proven; connection-count, session-count, and latency impact are not measured.

Status: Published as an open GitHub issue.
Location: [rclone/rclone#9675](https://github.com/rclone/rclone/issues/9675)

### ISSUE-2026-020 — smb: DirMove checks the destination using a different path representation

- `DirMove` calls `Stat(dstPath)` before renaming with `f.toSambaPath(dstPath)`.
- The probe and rename can address different paths when SMB filename encoding transforms the destination.
- The mismatch is source-proven, but no encoded-destination failure has been reproduced against an SMB server.

Status: Published as a closed GitHub issue (completed).
Location: [rclone/rclone#9677](https://github.com/rclone/rclone/issues/9677)

### ISSUE-2026-021 — smb: operation contexts do not cancel established SMB I/O

- `go-smb2` uses `context.Background()` for established sessions and shares unless `WithContext` is called.
- SMB operation contexts currently affect connection setup but do not cancel later share I/O.
- The limitation is documented in merged PR #8327; no stuck cancellation scenario has been reproduced.

Status: Published as an open GitHub enhancement issue.
Location: [rclone/rclone#9708](https://github.com/rclone/rclone/issues/9708)

### ISSUE-2026-022 — smb: failed connection setup paths do not close the TCP connection

- `Fs.dial` opens `tconn` before password decoding, Kerberos client creation, and the SMB handshake.
- Errors from `obscure.Reveal`, `GetClient`, or `DialConn` return without explicitly closing the caller-owned connection.
- The ownership gap is source-proven; file-descriptor or connection growth has not been measured.

Status: Published as a closed GitHub issue (completed).
Location: [rclone/rclone#9678](https://github.com/rclone/rclone/issues/9678)

### ISSUE-2026-023 — smb: Put can return nil when an upload error leaves the object behind

- `Put` and `PutStream` return `nil, err` for every `Object.Update` failure.
- The destination can remain after failed cleanup or a `SetModTime` error following a completed upload.
- The return-contract mismatch is source-proven; downstream retry and cleanup effects have not been reproduced.

Status: Published as an open GitHub issue.
Location: [rclone/rclone#9679](https://github.com/rclone/rclone/issues/9679)

### ISSUE-2026-024 — smb: dead pooled connections can be discarded without closing the TCP transport

- Open PR #9388 changes the exact connection-pool and file-pool discard lifecycle.
- Its current dead-connection paths discard references without explicitly closing the caller-owned TCP transport.
- The ownership gap is source-proven; transport-resource growth has not been measured.

Status: Published as a comment on open GitHub pull request #9388.
Location: [rclone/rclone#9388 comment 5108852112](https://github.com/rclone/rclone/pull/9388#issuecomment-5108852112)

### ISSUE-2026-025 — smb: prefer matching-share connections before remounting

- `getConnection` removes the FIFO head without first looking for a later connection with the requested `shareName`.
- It calls `mountShare` while holding `poolMu`, so a share change may perform `Umount` and `Mount` under the lock.
- The flow is source-proven; mixed-share remount counts and latency impact have not been measured.

Status: Published as a comment on open GitHub pull request #9388.
Location: [rclone/rclone#9388 comment 5108715075](https://github.com/rclone/rclone/pull/9388#issuecomment-5108715075)

### ISSUE-2026-026 — smb: DirMove reports destination exists for unrelated Stat errors

- `DirMove` performs the rename only when the destination `Stat` returns `os.IsNotExist`.
- A successful `Stat` and every other error both produce `fs.ErrorDirExists`.
- The mapping is source-proven, but permission and transport failure cases have not been reproduced.

Status: Published as a closed GitHub issue (completed).
Location: [rclone/rclone#9680](https://github.com/rclone/rclone/issues/9680)

## Command Comments

This section tracks published command-related comments and their external location.

### ISSUE-2026-027 — copyurl: URL batches create a separate HTTP client per entry

- The comment identifies a separate HTTP client and transport for every CSV entry processed by `copyurl --urls`.
- It proposes one batch-owned client so completed transfers to the same host can reuse pooled connections.
- It asks whether that design would be acceptable as a small follow-up to the implementation from PR #8810.

Status: Published as a comment on a closed GitHub issue.
Location: [rclone/rclone#8127 comment](https://github.com/rclone/rclone/issues/8127#issuecomment-5087488288)
Format note: Its follow-up PR offer conflicts with the current [FORMAT.md](FORMAT.md) prohibition on PR offers.

### ISSUE-2026-028 — lsjson: requested hashes reread local files

- `ListJSON` calls `Object.Hash` separately for every requested hash type.
- The local backend opens and reads the file again whenever that individual hash is not cached.
- Current measurements show one, four, and thirteen requested hashes produce matching numbers of full read passes.

Status: Published as a comment on a closed GitHub issue.
Location: [rclone/rclone#4181 comment 5138225391](https://github.com/rclone/rclone/issues/4181#issuecomment-5138225391)
