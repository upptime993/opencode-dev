---
name: engineering-guardrails
description: Cross-skill engineering safety protocol for AI coding agents. Must be consulted before planning, changing, or verifying code. Prevents hallucinated requirements, unsupported assumptions, unverified completion, scope creep, and destructive changes.
---

# ENGINEERING GUARDRAILS v3

This is the mandatory operating contract for all project skills.

## 1. Evidence hierarchy

When deciding what the project requires, use this order:

1. Explicit current user instruction.
2. Existing accepted project documents and approved decisions.
3. Existing code/config/tests that can be directly inspected.
4. External official documentation, only when needed and available.
5. AI inference.

Never silently promote level 5 into level 1-4.

Use explicit labels:
- `[USER]` — directly requested by the user.
- `[PROJECT]` — accepted project decision/document.
- `[CODE]` — directly observed in the repository.
- `[DOC]` — verified external documentation.
- `[INFERRED]` — AI conclusion not directly proven.
- `[PROPOSED]` — recommendation, not a requirement.

## 2. The no-guess rule

If a missing fact can materially change:
- architecture,
- data model,
- public API,
- authentication/authorization,
- persistence,
- dependency choice,
- deployment,
- destructive behavior,
- user-visible behavior,

STOP and ask, or explicitly record a bounded assumption and obtain approval before implementation.

Do not resolve important ambiguity merely because a choice is "common", "best practice", or "probably what the user wants".

## 3. Requirements must be testable

Every implementation requirement needs:
- unique ID,
- source,
- observable acceptance criteria,
- out-of-scope boundary.

Bad:
> "Make login secure."

Good:
> `FR-03` source `[USER]`: user can sign in with email/password.
> Acceptance: valid credentials create a session; invalid credentials return an error; password is never stored in plaintext.
> Out of scope: social login.

Do not manufacture arbitrary counts such as "15-20 FRs" just to fill a template.

## 4. Assumptions are not requirements

An assumption may be used temporarily for analysis, but it must never become a requirement by propagation.

Every assumption has:
- ID,
- reason,
- impact if wrong,
- status: `OPEN | APPROVED | REJECTED`,
- source decision.

`OPEN` assumptions that materially affect implementation block implementation.

## 5. Change budget

Before editing code, define:
- requested behavior,
- allowed files/areas,
- forbidden/unrequested behavior,
- acceptance checks.

If implementation discovers a necessary change outside that boundary:
- tiny mechanical change with no behavior impact: may proceed and record it;
- behavior/architecture/data/API change: STOP and ask for approval;
- security or destructive change: STOP and ask explicitly.

## 6. Existing repository rule

For an existing project, inspect before designing:
- tree,
- package/dependency manifest,
- runtime/tool versions,
- entry points,
- relevant modules,
- configuration,
- tests,
- database/schema/migrations,
- current git diff/status if available.

Do not rewrite an existing architecture from a template without evidence.

## 7. Implementation is not complete until verified

Never mark a task `Done` merely because code was written.

Minimum evidence:
- relevant formatter/linter/typecheck/build command when available;
- relevant automated tests when available;
- focused smoke/manual check when automated coverage is absent;
- changed-file review/diff;
- acceptance criteria mapped to evidence.

If a check cannot run, status is `NEEDS_VERIFICATION`, not `Done`.

## 8. Verification must test the whole change

Verification must include:
- happy path,
- relevant failure/edge cases,
- regression risk,
- security implications where applicable,
- scope fidelity,
- actual changed files/diff.

Do not verify only by rereading the task text.

## 9. Failure protocol

If a command fails:
1. preserve the real error;
2. identify whether failure is environment, dependency, code, or test;
3. make the smallest justified fix;
4. rerun the check;
5. never hide failures by weakening/removing tests or ignoring errors.

If verification cannot establish correctness, report uncertainty explicitly.

## 10. Traceability chain

The preferred chain is:

`USER → BRIEF → PRD → FR → TECH SPEC → TASK → CODE → TEST EVIDENCE`

Every implementation task must have a valid upstream source. Every acceptance criterion must have downstream verification evidence.

## 11. No fake certainty

Never say:
- "100% correct",
- "production-ready",
- "best architecture",
- "secure",
- "done"

unless the evidence supports that claim and its limits are stated.

Prefer:
> "Implemented. Build passes. Tests X/Y pass. Acceptance AC-01..AC-03 verified. AC-04 not verified because no integration environment is available."

## 12. Git safety

Before destructive or broad changes:
- inspect `git status` and diff if available;
- do not overwrite unrelated user changes;
- do not reset/revert user work without explicit approval;
- keep changes minimal and reviewable.

## 13. Output contract

At the end of every implementation/verification action report:
- what changed,
- source,
- files,
- checks run + exact result,
- unresolved risks,
- scope deviations,
- next blocked/unblocked step.
