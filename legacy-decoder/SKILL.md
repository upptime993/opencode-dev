---
name: legacy-decoder
description: Reverse-engineer an existing codebase while strictly separating observed evidence from inference and preserving uncertainty.
---

# LEGACY DECODER v3

Read `00-engineering-guardrails/SKILL.md` first.

## Goal
Explain what the code actually does, not what the AI imagines it was intended to do.

## Evidence classes
- `[CODE]` directly observed behavior/structure.
- `[DOC]` documented behavior found in project docs/config.
- `[INFERRED]` interpretation that is not proven.

Never write an inference as a fact.

## Discovery
Inspect:
- repository tree,
- manifests and lockfiles,
- entry points,
- routes/controllers/services,
- data models/schema/migrations,
- configuration,
- tests,
- scripts/build/deploy files,
- external integrations visible in code.

## Behavior mapping
For each important flow:
`entry → validation → transformation → side effects → persistence/external call → response/error`.

Quote exact file/function references when possible.

## Database
Derive relationships from actual schema/foreign keys where available.
Mark convention-based relationships as `[INFERRED]`.

## Unknowns
Explicitly list:
- unreachable code paths,
- missing tests,
- undocumented behavior,
- ambiguous business rules,
- dependencies whose behavior was not verified.

## Output
`.agents/4-LEGACY-DECODER.md`

Do not automatically create refactor tasks from inferred intent. Any such task is `[PROPOSED EXTRA]` until confirmed.
