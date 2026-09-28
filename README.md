# OpenCode Dev Skills v3

Engineering workflow focused on preventing AI coding agents from guessing, scope-creeping, and falsely claiming completion.

## Pipeline

`idea-intake → mini-prd → write-tech-spec → create-issues → implement-task → verify`

Supporting skills:
- `legacy-decoder` — evidence-based reverse engineering
- `learnit` — project-based learning with verification
- `00-engineering-guardrails` — mandatory cross-skill safety/quality contract

## Core improvements over v2

1. **Evidence hierarchy**: user request, project decisions, code, docs, inference are explicitly separated.
2. **No-guess rule**: architecture/data/API/security ambiguity can block implementation.
3. **Testable requirements**: requirements require observable acceptance criteria.
4. **No arbitrary feature counts**: no invented FR/US just to fill a template.
5. **Decision log**: major technical decisions record evidence, alternatives, risk, and status.
6. **Repository reconnaissance**: existing code is inspected before architecture is proposed.
7. **Change budget**: every implementation has an explicit scope boundary.
8. **Verification evidence**: `Done` requires actual checks; otherwise use `NEEDS_VERIFICATION`.
9. **Failure protocol**: failed checks cannot be hidden or bypassed.
10. **Diff-aware verification**: actual changes are compared against user requirements and tasks.
11. **Inference containment**: inferred legacy behavior cannot silently become refactor requirements.
12. **Acceptance traceability**: `USER → BRIEF → PRD → FR → SPEC → TASK → CODE → TEST`.

## Recommended installation

Copy the skill directories into the appropriate agent skill directory. Keep `00-engineering-guardrails` available to every project skill.
