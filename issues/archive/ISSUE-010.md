# ISSUE-010 — dropbox: ChangeNotify root trimming assumes matching PathDisplay casing

State: Archived
Authorized-Work: Research-and-Reporting
Publication-Target: New-issue
External-Reference: https://github.com/rclone/rclone/issues/9692
Severity: Low
Contribution-Priority: Low
Root-Cause-Confidence: High
Finding-Category: Correctness
Created: 2026-09-23
Updated: 2026-09-23
Source: `upstream/master@90e67915c88d4adf244f1d5251088c339c8b8e23`
Legacy-ID: year=2026; sequence=010

## Root-Cause

Root-Cause [S]: `changeNotifyRunner` removes the configured root from `PathDisplay` with case-sensitive `strings.TrimPrefix`; the pinned Dropbox SDK documents rare casing differences.

## Reach-and-Impact

Reach [A]: Change notifications whose `PathDisplay` root casing differs from the configured root.
Impact [A]: Relative paths may be incorrect; resulting VFS cache behavior is not reproduced.

## Evidence

- [S] Legacy ledger names `changeNotifyRunner`, `strings.TrimPrefix`, and the SDK casing caveat.
- [S] Published report: https://github.com/rclone/rclone/issues/9692

## Prior-Art

Coverage: Current `upstream/master` ChangeNotify root trimming verified; full issue thread not rechecked; checked=2026-09-23.
Gaps: Rare casing behavior and final discussion outcome remain unverified.

- https://github.com/rclone/rclone/issues/9692 — Same reported root cause.

Contribution fit: Existing issue — already reported; no duplicate publication proposed.

## Proposed-Change

Normalize root comparison according to Dropbox path casing semantics, if current source and SDK contract confirm the mismatch.

## Scope-and-Constraints

- Preserve correct relative-path derivation for existing notifications.
- Exclude unrelated VFS cache changes.
- Cache impact is unmeasured.

## Verification

- No mismatched-casing notification scenario is recorded.

## Publication-Blockers

Current source trims Dropbox notification roots using case-insensitive `trimPrefixFold`.

## Next-Action

Summary: —
Action: None.
Done-When: None.

## Publication-Draft

Target: https://github.com/rclone/rclone/issues/9692
Title: Original title is not preserved in the legacy ledger.

Body:

```text
The legacy ledger did not retain the submitted body. The root-cause and evidence sections preserve the available summary; this is not a verbatim copy of the published text.
```

## Archive

Archive-Reason: Fixed-Elsewhere
Detail: Current upstream compares each root rune case-insensitively when trimming notification paths.
Evidence: `backend/dropbox/dropbox.go:1686-1704,1777` at `upstream/master@90e67915c88d4adf244f1d5251088c339c8b8e23`.
Checked: 2026-09-23
