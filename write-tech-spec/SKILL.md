---
name: write-tech-spec
description: Produce an evidence-based technical specification from approved requirements, preserving uncertainty and preventing architecture hallucinations.
---

# TECH SPEC v3

Read `00-engineering-guardrails/SKILL.md` first.

## Guard
Read the full PRD and decision/assumption log.

## Phase 0 — Repository reconnaissance
If an existing repository exists, inspect it before proposing architecture:
- tree,
- manifests,
- runtime versions,
- entry points,
- relevant code,
- tests,
- schema/migrations,
- config,
- git status/diff if available.

Mark observations `[CODE]`.

Do not replace an existing stack merely because another stack is fashionable.

## Phase 1 — Decisions
For every major technical decision record:
- Decision ID
- Choice
- Evidence/source
- Alternatives considered
- Reason
- Reversal cost/risk
- Status `APPROVED | PROPOSED | OPEN`

Major decisions include:
- framework/runtime,
- DB,
- auth,
- API style,
- state management,
- deployment,
- external services,
- schema changes.

## Phase 2 — Design
Include only components required by accepted FRs or necessary mechanical infrastructure.

For each entity/endpoint/component:
`Purpose → source FR → inputs → outputs → invariants → failure behavior`.

Do not add "standard" endpoints, entities, caching, queues, auth, logging, analytics, or admin features unless:
- required by an accepted requirement, or
- explicitly marked `[PROPOSED]` and kept outside the implementation baseline.

## Phase 3 — Risks
For each risky decision:
- failure mode,
- impact,
- mitigation,
- verification method.

## Phase 4 — Acceptance mapping
Every FR/AC must map to:
`FR/AC → component → implementation task → verification`.

If a requirement has no verification method, flag it.

## Output
`.agents/2-TECH-SPEC.md`

Never present an unverified architecture as "the correct architecture".
