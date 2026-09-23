# ISSUE-004 — dropbox: shared-mode lookup uses case-sensitive name matching

State: Archived
Authorized-Work: Research-and-Reporting
Publication-Target: New-issue
External-Reference: https://github.com/rclone/rclone/issues/9706
Severity: Medium
Contribution-Priority: Low
Root-Cause-Confidence: High
Finding-Category: Correctness
Created: 2026-09-23
Updated: 2026-09-23
Source: `upstream/master@90e67915c88d4adf244f1d5251088c339c8b8e23`
Legacy-ID: year=2026; sequence=004

## Root-Cause

Root-Cause [S]: Dropbox advertises `CaseInsensitive: true`, but `findSharedFolder` and `findSharedFile` compare requested and returned names with exact equality.

## Reach-and-Impact

Reach [A]: Shared-folder or received-file lookups with a case-only name difference.
Impact [A]: Such lookups may fail despite the advertised case-insensitive behavior; no case-only reproduction is recorded.

## Evidence

- [S] Legacy ledger identifies the capability declaration and both exact-equality finders.
- [S] Published report: https://github.com/rclone/rclone/issues/9706

## Prior-Art

Coverage: Current `upstream/master` shared-name comparisons verified; full issue thread not rechecked; checked=2026-09-23.
Gaps: Account-based case-only behavior and final discussion outcome remain unverified.

- https://github.com/rclone/rclone/issues/9706 — Same reported root cause.

Contribution fit: Existing issue — already reported; no duplicate publication proposed.

## Proposed-Change

Use the backend's established case-insensitive name comparison in both shared-mode finders, if current source confirms the mismatch.

## Scope-and-Constraints

- Preserve current shared-file and shared-folder selection semantics beyond case handling.
- Exclude unrelated Dropbox listing changes.
- User-visible failure is not reproduced.

## Verification

- No case-only shared object reproduction is recorded.

## Publication-Blockers

Current shared-folder and shared-file finders use `strings.EqualFold`.

## Next-Action

Summary: —
Action: None.
Done-When: None.

## Publication-Draft

Target: https://github.com/rclone/rclone/issues/9706
Title: Original title is not preserved in the legacy ledger.

Body:

```text
The legacy ledger did not retain the submitted body. The root-cause and evidence sections preserve the available summary; this is not a verbatim copy of the published text.
```

## Archive

Archive-Reason: Fixed-Elsewhere
Detail: Current upstream applies case-insensitive equality to both shared folder and file lookups.
Evidence: `backend/dropbox/dropbox.go:957-963,1039-1043` at `upstream/master@90e67915c88d4adf244f1d5251088c339c8b8e23`.
Checked: 2026-09-23
