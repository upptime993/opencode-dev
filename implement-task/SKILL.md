---
name: implement-task
description: Implement one approved task with strict scope control, repository inspection, minimal changes, and evidence-based completion.
---

# IMPLEMENT TASK v3

Read `00-engineering-guardrails/SKILL.md` first.

## Preconditions
1. Read `3-TASKS.md`.
2. Task must be approved and not `[PROPOSED EXTRA]`.
3. Task must have a valid source and acceptance criteria.
4. Check dependencies.
5. Inspect current git status/diff when available.
6. Read all relevant existing files before editing.

If a precondition fails, do not guess. Mark the task `BLOCKED` or ask for clarification.

## Before coding: change contract
Write internally or in the task:
- In scope
- Out of scope
- Expected files/areas
- Acceptance checks
- Risky/destructive operations prohibited

## Implementation rules
- Prefer the smallest change that satisfies the acceptance criteria.
- Preserve unrelated user changes.
- Do not rewrite working code merely for style.
- Do not add product behavior that is not in scope.
- Mechanical helpers are allowed only when they do not change product behavior.
- Any behavior, schema, API, auth, dependency, or deployment change outside scope requires approval.

## Verification before Done
Run the strongest available checks:
1. formatter/linter/typecheck,
2. build/compile,
3. relevant unit/integration tests,
4. focused smoke test,
5. inspect diff.

Use project-specific commands discovered from manifests/config. Never invent commands blindly.

If a check fails, diagnose and rerun after a justified fix.
If a required check cannot run, use `NEEDS_VERIFICATION`, not `DONE`.

## Final report
- Task ID/source
- Files changed
- Acceptance criteria + evidence
- Commands run + results
- Scope deviations
- Remaining risks
- Status
