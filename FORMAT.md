# GitHub Issue and Comment Reporting Format

## Authority

This file is the single source of truth for reporting rules for findings sent to the rclone project.
`ISSUES.md` is the sole source of truth for finding IDs, lifecycle status, and published locations.

- The goal is to report findings only.
- Never create, draft, offer, or propose a pull request.
- Never implement a reported fix unless a later instruction explicitly changes this policy.
- Default to proposal-only mode and show every draft to the user before publication.
- Publish externally only after the user explicitly approves the exact draft and target.
- Write GitHub issues and comments in friendly, concise English.
- Communicate with the user in the language of the surrounding conversation.

## Required Research

Before drafting anything:

1. Read `ISSUES.md` and use its current finding ID, lifecycle status, target, and published location.
2. Read the current upstream contribution guide and applicable issue template.
3. Search open and closed issues for the same symptom, root cause, backend, and relevant symbols.
4. Search open and merged pull requests for changes that own or introduced the relevant code.
5. Search rclone forum discussions for the same symptom, root cause, backend, and relevant symbols.
6. Read every potentially relevant issue, comment, pull request, review, forum thread, and current diff.
7. Verify all source claims against the current upstream implementation and pinned dependency contracts.
8. Record which effects are observed, source-proven, assumed, or not yet measured.

A search result is only a candidate target.
A matching word or symptom does not prove that an existing thread owns the same root cause.

## Issue or Comment Decision

Use this decision order for every finding.

### Comment on an existing open issue

Comment when all of the following are true:

- The issue describes the same observable problem or the same root cause.
- The new information materially advances diagnosis, evidence, reproduction, or resolution.
- The comment will not redirect the issue to an unrelated performance or architecture topic.

Do not comment merely because the same backend, error string, or API appears.
Do not hijack an issue whose actual cause differs.

### Comment on an existing open pull request

Comment when all of the following are true:

- The pull request currently changes the exact lifecycle, function, or invariant involved.
- The finding identifies a correctness, ownership, resource, or concurrency gap in the current diff.
- The comment is actionable within the current scope or clearly marked as an optional follow-up.

Do not ask the author to absorb unrelated work.
State explicitly when no scope expansion is requested.
Never create or offer a competing pull request.

### Open a new issue

Open a new issue when any of the following is true:

- No open issue or pull request owns the same root cause.
- Existing matches are closed, historical, tangential, or based on a different cause.
- The finding needs durable tracking beyond a temporary pull request discussion.
- A merged pull request provides useful history but no active discussion target.

Link relevant historical issues and pull requests without reopening or hijacking them.
Use one issue per independent root cause.

### Hold the finding without reporting it

Do not publish when any of the following is true:

- Reachability, root cause, or currentness is unverified.
- The claim is only a static pattern without realistic cost or behavior evidence.
- The proposed target is merely similar rather than directly relevant.
- The report would duplicate information already present in the target thread.
- The only support is an unmeasured severity or speculative production impact.

Keep a proposal draft and state exactly which evidence is missing.

### Ask the user

Use the interactive ask mechanism before proceeding when:

- Two targets are materially plausible and choosing one risks thread hijacking.
- The choice between a new issue and a comment has meaningful visibility or scope trade-offs.
- Required reproduction data, disclosure text, or publication scope is missing.
- The requested action would publish externally and the exact draft has not been approved.

Do not ask when research makes the correct target clear.
Recommend the safest target when presenting a choice.

## Evidence Contract

Every report must label its evidence honestly.

- Observed: reproduced behavior with command, version, environment, and output.
- Source-proven: current control flow or API ownership proves the invariant.
- Assumed: a necessary premise that has not been verified.
- Not measured: latency, throughput, resource growth, or request-count impact lacks measurement.

Rules:

- Never convert a source-proven invariant into an observed user impact.
- Never call something a leak without deterministic lifetime evidence or a resource-growth measurement.
- Never claim a network request occurs when it only may occur for some cache or protocol states.
- Never use internal severity labels such as Critical, High, Medium, or Low in upstream communication.
- Use exact `path:line`, function, method, field, error, and API names where they disambiguate the claim.
- Link the relevant issue or pull request when it establishes design intent or historical ownership.

## Duplicate-Search Statement

Every proposed report must include the applicable exact statement after its question and before its involvement text.

For a new issue:

```text
I checked all relevant issues, comments, pull requests, and forum threads; this report is not a duplicate.
```

For an existing issue or pull request comment:

```text
I checked all relevant issues, comments, pull requests, and forum threads; this evidence is not already reported.
```

Make this confirmation only after completing the required research and fully reading every plausible prior-art candidate.
If any plausible candidate is unavailable or unread, hold the finding instead.

## Tone Contract

- Start with appreciation when commenting on another contributor's work.
- Use neutral phrases such as `I noticed`, `I may be missing context`, and `Would it make sense`.
- Describe code behavior, not author intent or competence.
- Ask one concrete question when maintainer input is needed.
- Avoid blame, demands, alarmism, sarcasm, and rhetorical severity.
- Keep one root cause and one requested decision per report.
- Do not tag maintainers or previous authors unless they are already participating or the user explicitly approves it.
- State that no pull request is planned when implementation is not being offered.

## New Issue Format

Use the official GitHub issue form when it requires named fields.
Map the content below into the closest fields instead of fighting the form.

### Title

```text
<area>: <specific observed or source-proven problem>
```

Title rules:

- Name the affected area, such as `smb`.
- State the concrete problem, not the proposed implementation.
- Avoid severity words, speculation, and generic titles such as `performance issue`.

### Body

```markdown
## Summary

<One concise paragraph describing the observed or source-proven problem.>

## Evidence

- `<path:line>`: <specific control-flow, lifecycle, or ownership evidence>.
- <Relevant command, output, API contract, issue, or pull request link>.

## Impact

<Observed impact with measurement, or an explicit statement that the impact has not yet been measured.>

## Question

<One concrete question about ownership, expected behavior, or the preferred direction.>

I checked all relevant issues, comments, pull requests, and forum threads; this report is not a duplicate.

## Involvement

I am reporting this finding only and am not currently proposing a pull request.

Investigated extensively with GPT-5.6 Sol (xhigh reasoning effort), using [Oh My Pi](https://github.com/can1357/oh-my-pi) as the agent framework.
```

### Bug form additions

When using the bug form, include every required field:

- latest tested rclone version
- operating system
- backend
- `rclone config redacted`
- exact command
- `-vv` log
- deterministic reproduction steps

Do not use the bug form for a source-only performance hypothesis without a reproducible user-visible problem.

### Enhancement form additions

Use the enhancement form when the invariant is source-proven but user-visible harm is not reproduced.
State the current problem, the desired invariant, and what remains unmeasured.

## Existing Issue Comment Format

```markdown
Hi, thanks for documenting this.

I noticed one detail that may be relevant to the same root cause:

- `<path:line>`: <new evidence>.
- <Why this evidence changes or strengthens the current diagnosis>.

<One concise question or proposed next diagnostic step.>

I checked all relevant issues, comments, pull requests, and forum threads; this evidence is not already reported.

I am only reporting the finding and am not currently proposing a pull request.

Investigated extensively with GPT-5.6 Sol (xhigh reasoning effort), using [Oh My Pi](https://github.com/can1357/oh-my-pi) as the agent framework.
```

Only use this format when the existing issue owns the same root cause.
Otherwise open a new issue or hold the finding.

## Existing Pull Request Comment Format

```markdown
Hi, thanks for working on this.

While reading the current diff, I noticed one possible <lifecycle, ownership, resource, or concurrency> gap:

- `<changed path:line>`: <specific behavior in the current diff>.
- `<related path or API contract>`: <why the current behavior may be incomplete>.

Would it make sense to <one focused question or suggestion>?
I may be missing ownership handled elsewhere.
I checked all relevant issues, comments, pull requests, and forum threads; this evidence is not already reported.
I am not suggesting a broader scope change or a separate pull request.

Investigated extensively with GPT-5.6 Sol (xhigh reasoning effort), using [Oh My Pi](https://github.com/can1357/oh-my-pi) as the agent framework.
```

Keep review comments scoped to the active diff.
Move independent follow-up ideas to a new issue only after user approval.

## Condensed Output Contract

When proposing reports to the user, output each candidate in this stable order:

```text
target: <new issue | issue #N | PR #N>
action: <open issue | comment | hold>
reason: <one sentence>
title: <new issue title, otherwise omit>
draft: <complete proposed text>
status: proposal only; not published
```

For multiple findings, preserve their existing IDs and never combine independent root causes.

## Publication Gate

Before publishing, verify every item:

- The target still exists and its state has not changed.
- The finding's ID, lifecycle status, target, and published location match `ISSUES.md`.
- The draft matches the latest source or current pull request diff.
- The report adds information not already present.
- Every material claim has evidence or an explicit uncertainty label.
- The tone is friendly and non-accusatory.
- The report contains no pull request offer or implementation commitment.
- The exact disclosure footer is the final paragraph.
- The applicable duplicate-search statement is present and every plausible prior-art candidate was fully read.
- The user approved the exact target and final text.

After publication, record the exact target, published status, and URL in `ISSUES.md` before returning only the
created issue or comment URLs and a concise status.

## Prohibited Actions

- Never create, draft, propose, or offer a pull request.
- Never implement code as part of this reporting workflow.
- Never publish without explicit approval of the exact draft.
- Never cross-post the same finding to multiple threads.
- Never revive a closed issue with an unrelated new root cause.
- Never use an active issue as a generic backend discussion.
- Never report unverified static patterns as production bugs.
- Never hide uncertainty or fabricate measurements, commands, logs, or maintainer intent.
- Never alter the required disclosure footer.
