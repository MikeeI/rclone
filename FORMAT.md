# Issue, Comment, and Pull Request Format

## Authority

`AGENTS.md` owns repository identity, contribution intent, branch roles, and repository rules.
`ISSUES.md` owns the next finding ID and projects every finding's current state.
`issues/ISSUE-NNN.md` is authoritative for one root cause, its evidence, lifecycle, drafts, and next action.
This file owns finding fields and the research, implementation, and publication workflow.
`skill-fork-contribution-tracking` owns workflow details; `skill-maintainer-communication` owns external research and writing quality.
`skill-semantic-compression-3` owns meaning-preserving compression of tracking content.
Current upstream contribution guides, forms, and templates override generic external formats.

- Keep one durable record per independent root cause and retain its identifier through research, implementation, and publication.
- Migrate legacy `ISSUE-YYYY-NNN` IDs to `ISSUE-NNN` by preserving the sequence; store the old year and sequence as `Legacy-ID: year=YYYY; sequence=NNN`.
- Keep fork-only tracking files and commits out of upstream contribution diffs.
- Verify source claims against the current canonical upstream branch.
- Never infer reproduction, impact, resolution, or maintainer intent.

## Evidence

Label each material claim at the point of use:

- `[O]` Observed: behavior reproduced with command, version, environment, and result.
- `[S]` Source-proven: current control flow, API ownership, or deterministic data flow proves the claim.
- `[A]` Assumed: an unverified premise required by the claim.

Keep impact explicitly unmeasured when no representative measurement exists.
Never convert a source-proven invariant into observed user impact.
Preserve exact paths, symbols, commands, outputs, URLs, revisions, dates, and drafts.

## Severity ranking

- `High`: likely to cause a broad, material correctness, reliability, or safety failure during ordinary supported use.
- `Medium`: causes a meaningful failure or cost on a bounded but realistic path, or demonstrated substantial cost on a large workload.
- `Low`: affects a narrow edge case or adds limited overhead, with no established broad user harm.
- Rank by current user and operational impact, not merely report status, code complexity, or contribution convenience.
- `ISSUES.md` open rows are ordered by descending `Severity`; each issue record owns its rating and current-source evidence.

## Finding IDs and index

- New IDs use `ISSUE-NNN`, start at `ISSUE-001`, contain at least three digits, and are never reused or renumbered.
- `Next finding ID: ISSUE-NNN` in `ISSUES.md` is the only allocator.
- Before allocating, search the full index and every plausible open or archived record for the same root cause.
- Update an existing record when it already owns that root cause; external numbers never replace internal IDs.
- Create the record, add its index row, and advance the allocator in one change.
- `ISSUES.md` is a projection; correct it from the authoritative record whenever they disagree.
- Open rows project ID, title, State, Authorized-Work, Publication-Target, Contribution-Priority, Next-Action/Summary, and External-Reference.
- Archived rows project ID, title, Authorized-Work, Publication-Target, Contribution-Priority, Archive-Reason, and External-Reference.

## Record schema

Every `issues/ISSUE-NNN.md` starts with these fields in this order:

```text
State: Investigating | Draft-Ready | Implementing | PR-Ready | Submitted | Archived
Authorized-Work: Research-and-Reporting | Pull-Request-Implementation | Not-Selected
Publication-Target: New-issue | Existing-issue-comment | New-pull-request | Existing-pull-request-comment | Not-Selected
External-Reference: <exact URL or identifier | Not published.>
Contribution-Priority: High | Medium | Low
Severity: High | Medium | Low
Root-Cause-Confidence: High | Medium | Low
Finding-Category: Correctness | Reliability | Performance | Maintainability | API | UI | Build | Test | Other
Created: <YYYY-MM-DD>
Updated: <YYYY-MM-DD>
Source: `upstream/<branch>@<commit>`
Legacy-ID: <year and sequence of prior ID | None.>
```

Required sections, in order: `Root-Cause`, `Reach-and-Impact`, `Evidence`, `Prior-Art`, `Proposed-Change`, `Scope-and-Constraints`, `Verification`, `Publication-Blockers`, and `Next-Action`.
Every open record has one `Next-Action` with `Summary`, `Action`, and `Done-When` fields.
Use `Bug-Reproduction`, `Performance-Evidence`, `Shared-Change-Pressure`, and `API-and-Compatibility` only when applicable.
Use `Pull-Request-Implementation`, `Publication-Draft`, and `Submitted-Text` only when applicable.
Archived records additionally have `Archive` with `Archive-Reason`, `Detail`, `Evidence`, and `Checked`.

## Prior art and contribution decision

For a new thread, search open and closed issues, pull requests, discussions, release notes, and project-linked forums by symptom, component, error, symbol, cause, and proposed fix.
Read every plausible match and linked context; classify it as duplicate, related, fixed or superseded, or distinct.
Record search coverage and gaps honestly; a search result alone does not establish shared root cause.
Before replying, read the complete thread, linked context, later patches, and releases.
Recommend in order: a verified bounded pull request when no active implementation owns it; otherwise a useful comment on the canonical thread; otherwise a new issue; otherwise continued investigation.
The user selects `Authorized-Work` and `Publication-Target`; the recommendation is not authorization.

## Lifecycle and authorization

- `Investigating`: evidence, currentness, authorization, target, or direction is unresolved; state blockers and one bounded next action.
- `Draft-Ready`: research, authorization, target, and exact draft are complete; this state does not authorize publication.
- `Implementing`: authorized `Pull-Request-Implementation` is in progress; record branch, base, scope, commit, push, and checks.
- `PR-Ready`: authorized implementation is complete, verified, committed, pushed, and has an exact pull request draft and target.
- `Submitted`: an observable external issue, comment, or pull request exists; record its exact URL and submitted text if it differs from the draft.
- `Archived`: no current action remains and an evidence-backed `Archive-Reason` is recorded.
- Map legacy Hold to Investigating, Drafted to Draft-Ready, Ready to PR-Ready, and Published to Submitted.
- Map Closed and Rejected to Archived while preserving the exact prior outcome in `Archive-Reason` or `Detail`.
- Map Fixed to Fixed-Elsewhere, Declined to Upstream-Declined, and Invalid to Finding-Invalidated.
- Preserve Merged, Duplicate, Superseded, and Withdrawn; map other exact outcomes to Other and preserve the literal in `Detail`.
- Never infer resolution from inactivity or a closed thread; archive only when no current action remains.
- Keep open records under `issues/`; move archived records to `issues/archive/` and update the index in the same change.

## Implementation boundary

`Research-and-Reporting` permits research, drafting, issues, and comments, but no source implementation.
`Pull-Request-Implementation` authorizes only the exact source scope recorded in that finding.
Use a clean contribution branch or worktree based on the current project-defined upstream branch.
Exclude `AGENTS.md`, `FORMAT.md`, `ISSUES.md`, and `issues/` from upstream contribution diffs.
Resolve callers, compatibility, lifecycle, and failure modes before changing shared behavior.
Run the narrowest conclusive repository-owned checks and record their observed results.
Apply the repository commit convention to each coherent commit.

## Publication

Use current upstream forms and templates; preserve their required field order.
Store the exact title and body under `Publication-Draft` once Draft-Ready or PR-Ready.
Before publication, re-check source currentness, prior art, target ownership, external template, evidence, and exact draft.
For migrated submissions whose verbatim title or body was not retained, state that gap explicitly in `Publication-Draft`; never invent submitted text.

Show the complete exact draft and target to the user before every external write; publish only after the user approves that exact draft and target.
A changed draft or target requires presenting the complete current version again.
After publication, immediately record the exact URL, submitted text, and State.
Never cross-post one finding, publish speculative or duplicate material, or make an unsupported implementation commitment.

## Validation

Run the read-only validator bundled with `skill-fork-contribution-tracking` after every ledger mutation and before completion.
Pass this repository root as its positional argument.
The validator checks IDs, records, links, projections, and lifecycle structure; it does not verify factual evidence or contribution value.
