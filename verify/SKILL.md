---
name: verify
description: Verify implemented changes against requirements, tests, runtime behavior, regression risk, security, and scope using actual evidence.
---

# VERIFY v3

Read `00-engineering-guardrails/SKILL.md` first.

## Inputs
Read:
- `3-TASKS.md`
- relevant Tech Spec/PRD
- actual changed files/diff
- tests/config needed for the feature.

Do not verify only from the task description.

## Verification matrix
For each acceptance criterion:
- Expected behavior
- Test/check performed
- Evidence/result
- Status `PASS | FAIL | NOT VERIFIED`

## Required checks
Choose checks appropriate to the project:
- format/lint,
- typecheck,
- build/compile,
- unit tests,
- integration tests,
- smoke/manual test,
- migration/schema validation,
- security checks for sensitive behavior,
- regression checks.

Do not claim a check passed if it was not actually run.

## Scope fidelity
Compare:
`actual diff → task → FR → original user requirement`.

Classify unexpected changes:
- mechanical/no behavior,
- required dependency change,
- scope creep,
- suspicious/unrelated.

Scope creep is not "good extra work"; it is a review item.

## Completion
A task is `DONE` only when required acceptance criteria are verified and no blocking failures remain.

Otherwise:
- `NEEDS_VERIFICATION`, or
- `BLOCKED`, or
- `FAIL`.

Save `.agents/REVIEW.md` with evidence, not just conclusions.
