# ISSUE-018 — smb: Kerberos client cache is recreated for every connection

State: Archived
Authorized-Work: Research-and-Reporting
Publication-Target: New-issue
External-Reference: https://github.com/rclone/rclone/issues/9674
Severity: Low
Contribution-Priority: Low
Root-Cause-Confidence: High
Finding-Category: Performance
Created: 2026-09-23
Updated: 2026-09-23
Source: `upstream/master@90e67915c88d4adf244f1d5251088c339c8b8e23`
Legacy-ID: year=2026; sequence=018

## Root-Cause

Root-Cause [S]: The SMB dial path creates a new `KerberosFactory` for each connection, discarding instance-local client, error, and ccache-mtime caches after one `GetClient` call.

## Reach-and-Impact

Reach [S]: Repeated Kerberos SMB connections.
Impact [S]: Cache reuse is absent across connections; repeated parsing, authentication latency, and KDC requests are not measured.

## Evidence

- [S] Legacy ledger records the per-connection factory lifecycle and instance-local caches.
- [S] Published report: https://github.com/rclone/rclone/issues/9674

## Prior-Art

Coverage: legacy record cites the report only; current source and resolution not rechecked; checked=2026-09-23.
Gaps: Current factory ownership and completed fix are unverified.

- https://github.com/rclone/rclone/issues/9674 — Same reported root cause.

Contribution fit: Existing issue — report is closed as completed; no duplicate proposed.

## Proposed-Change

No separate change proposed; validate that cache reuse was implemented without widening ownership unsafely.

## Scope-and-Constraints

- Preserve Kerberos credential freshness and error caching semantics.
- Exclude global cache changes without lifecycle proof.
- Authentication impact is unmeasured.

## Verification

- Historical latency and KDC request counts are not recorded.

## Publication-Blockers

Inspect the completed issue and current implementation before treating the root cause as resolved.

## Next-Action

Summary: —
Action: None.
Done-When: None.

## Archive

Archive-Reason: Other
Detail: Prior report is closed as completed; exact upstream fix and resolution evidence have not been checked.
Evidence: https://github.com/rclone/rclone/issues/9674
Checked: 2026-09-23
