# Dropbox Issues

This file tracks the Dropbox findings, their publication state, and their external location when published.
`CP4` is split into `CP4a` and `CP4b` because they have independent root causes.

## #9663 — dropbox: avoid redundant metadata request in Rmdir

- `Rmdir` currently performs `GetMetadata`, `ListFolder`, and `DeleteV2` when removing an empty directory.
- The metadata result is unused, while `ListFolder` can also identify missing paths and non-directory paths.
- The report asks whether the redundant request can be removed without changing error mapping or observable behavior.

Status: Published as an open GitHub issue.
Location: [rclone/rclone#9663](https://github.com/rclone/rclone/issues/9663)

## P1 — dropbox: known-size chunked uploads do not stop on early EOF

- `uploadChunked` rejects readers that exceed their declared size but does not detect a known-size reader ending early.
- Early EOF can finalize at a shorter offset or repeatedly append empty chunks, depending on the declared remainder.
- A realistic size-inconsistent source path and the resulting Dropbox behavior have not been reproduced.

Status: Hold; not published.
Location: Not published.

## P2 — dropbox: deep shared-folder roots use a path as the folder name

- The `shared_folders` documentation says the first path component identifies the shared folder.
- `NewFs` instead passes `path.Dir(f.root)` to a finder that compares it with a single shared-folder `Name`.
- The source mismatch is established, but no deep-root failure has been reproduced against a Dropbox account.

Status: Hold; not published.
Location: Not published.

## CP4a — dropbox: shared-mode lookup uses case-sensitive name matching

- The Dropbox backend advertises `CaseInsensitive: true` for its filesystem behavior.
- `findSharedFolder` and `findSharedFile` nevertheless compare requested and returned names with exact equality.
- No shared folder or received shared file has been tested with a case-only difference in its requested name.

Status: Hold; not published.
Location: Not published.

## CP4b — dropbox: received shared-file names bypass standard encoding conversion

- `listSharedFolders` applies `ToStandardName`, while `listReceivedFiles` stores the API-provided `Name` directly.
- `findSharedFile` then compares that raw remote name with the requested rclone name across the encoding boundary.
- No received shared file whose name requires conversion has been used to reproduce a listing or lookup failure.

Status: Hold; not published.
Location: Not published.
