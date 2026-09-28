---
name: mini-prd
description: Create a traceable, testable PRD from an approved brief without inventing features or arbitrary requirements.
---

# MINI PRD v3

Read `00-engineering-guardrails/SKILL.md` first.

## Guard
`0-BRIEF.md` must exist and be read completely.

## Rules
- Do not invent user personas, features, metrics, or requirements merely to reach a fixed count.
- Every requirement must have a source: `[USER]`, `[PROJECT]`, or an explicitly approved assumption.
- Every functional requirement must have acceptance criteria.
- Separate `MUST`, `SHOULD`, `MAY`, and `OUT OF SCOPE`.
- Preserve unresolved questions; do not silently settle them.
- If a requirement is inferred, it is `[INFERRED]` and does not become a MUST without approval.

## Structure
1. Problem / Goal
2. Users (only known users; unknown demographics remain unknown)
3. User stories
4. Functional requirements
5. Non-functional requirements
6. Acceptance criteria
7. Scope / Out of Scope
8. Dependencies / Constraints
9. Decision & Assumption Log

### Traceability
Use:
`US-01 ← [USER] brief U-02`
`FR-01 ← US-01`
`AC-01 ← FR-01`

Do not create a requirement solely because a typical application would have it.

## Final gate
Before Tech Spec, list:
- open decisions,
- high-impact assumptions,
- requirements with weak evidence,
- acceptance criteria that cannot yet be tested.

Material unresolved decisions block architecture decisions that depend on them.
