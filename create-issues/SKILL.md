---
name: create-issues
description: Break an approved technical specification into minimal, traceable, testable implementation tasks without scope creep.
---

# TASK GENERATOR v3

Read `00-engineering-guardrails/SKILL.md` first.

## Input
Prefer:
1. accepted Tech Spec,
2. accepted PRD,
3. direct user request for a small change.

If none is available, ask for the missing source.

## Task design
Each task must be:
- independently understandable,
- small enough to review,
- traceable to FR/AC or direct user input,
- explicit about files/areas,
- explicit about acceptance checks,
- explicit about dependencies.

Do not create arbitrary task counts.

### Required fields
- ID
- Title
- Source
- Scope
- Acceptance criteria
- Files/areas likely affected
- Dependencies
- Status
- Verification commands/checks
- Risk

### Extra work
Anything not required by the source is:
`[PROPOSED EXTRA]`

Extras never enter the executable queue until explicitly approved.

## Dependency rule
Order tasks by real dependency, not by document order.

## Completion rule
A task cannot be `Done` from planning alone.
Allowed statuses:
`TODO | IN_PROGRESS | BLOCKED | NEEDS_VERIFICATION | DONE | REJECTED`

`DONE` requires verification evidence.
