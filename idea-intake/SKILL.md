---
name: idea-intake
description: Convert an ambiguous project idea into an evidence-traceable brief. Prevents silent assumptions and premature architecture decisions.
---

# IDEA INTAKE v3

## Mandatory guard
Read `00-engineering-guardrails/SKILL.md` first.

## Goal
Produce a brief that distinguishes what the user actually requested from unresolved decisions. Do not manufacture requirements for completeness.

## Process

### 1. Capture the request
Store the user's original wording verbatim in `0-BRIEF.md`.

### 2. Extract only material unknowns
Identify ambiguities that could change:
- platform/runtime,
- data model,
- authentication,
- online/offline behavior,
- integrations,
- major user-visible behavior,
- security/privacy,
- deployment.

Do not ask cosmetic questions unless the user needs them now.

Use closed choices when they genuinely reduce ambiguity, but allow:
- `other`,
- `not decided`,
- `choose for me`.

Never treat silence as confirmation.

### 3. Hard constraints
Record only constraints actually stated or explicitly confirmed:
- deadline,
- platform,
- required stack,
- existing repository,
- required integrations,
- budget/resource constraints.

Unknown remains `UNKNOWN`.

### 4. Acceptance seed
For each confirmed goal, write at least one observable success condition.

Example:
`US-01`: user can add an expense.
`AC-01`: an expense with valid amount/date/category appears in the expense list.

### 5. Save
`.agents/0-BRIEF.md`

Required sections:
- Original Request
- Confirmed Requirements `[USER]`
- Open Questions
- Explicit Constraints
- Initial Acceptance Criteria
- Out of Scope / Not Yet Decided
- Assumptions (only if user explicitly allows defaults)

## Hard stops
Do not create a PRD if a critical ambiguity changes the product boundary and remains unresolved, unless the user explicitly authorizes a documented assumption.

Do not call a default "confirmed".
