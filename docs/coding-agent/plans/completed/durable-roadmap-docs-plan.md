# Durable Roadmap Docs Plan

- status: done
- owner: Orchestrator
- created: 2026-03-13
- completed: 2026-03-13
- objective: Create durable, reusable roadmap documentation that captures the short-term and long-horizon path toward a more helpful app, while keeping current product truth and assistant-readiness boundaries explicit.

## Scope

- In scope:
  - Author durable top-level docs for product roadmap and assistant-readiness boundaries.
  - Capture both short-term and long-horizon roadmap phases.
  - Cross-link the new docs to existing current-truth and guardrail docs.
- Out of scope:
  - Implementing roadmap items.
  - Changing product behavior or schemas.
  - Treating speculative assistant/integration concepts as current shipped features.

## Task Waves

- Wave 1: Task_1
- Wave 2: Task_2, Task_3
- Wave 3: Task_4

## Tasks

### Task_1
- type: design
- owns:
  - docs/personalization-roadmap.md
  - docs/llm-integration-readiness-contract.md
- depends_on: []
- acceptance:
  - Confirm the durable home is top-level `docs/`, not `docs/coding-agent/plans/`.
  - Confirm the two-doc split and define each document's role.
  - Freeze the source set: feature reference, personalization docs, cross-check findings, and completed-plan provenance as input only.
- validation:
  - required: true
    owner: orchestrator
    kind: evidence-review
    detail: The chosen document structure must be justified against existing docs patterns and the distinction between durable references and execution plans.

### Task_2
- type: docs
- owns:
  - docs/personalization-roadmap.md
- depends_on:
  - Task_1
- acceptance:
  - Create a durable roadmap organized around short-term and longer-horizon phases rather than implementation tasks.
  - Include current evidence snapshot, strategy principles, dependency gates, and explicit defer/non-goal sections.
  - Keep current product truth anchored in existing source docs instead of restating speculative features as shipped behavior.
- validation:
  - required: true
    owner: reviewer
    kind: clarity-review
    detail: The roadmap reads as a reusable strategy reference, not an execution plan or chat transcript.
  - required: true
    owner: reviewer
    kind: consistency-review
    detail: Claims remain aligned with docs/feature-reference.md and the existing personalization docs.

### Task_3
- type: docs
- owns:
  - docs/llm-integration-readiness-contract.md
- depends_on:
  - Task_1
- acceptance:
  - Define the current assistance-first, read-first assistant boundary.
  - List prerequisites before any future write-capable assistant surface becomes roadmap-ready.
  - Provide a staged graduation path from read-only analysis to draft-based and later user-confirmed write flows.
- validation:
  - required: true
    owner: reviewer
    kind: boundary-review
    detail: The contract must not imply unsupported current write capabilities or integrations.
  - required: true
    owner: reviewer
    kind: consistency-review
    detail: Terminology must match existing feature-reference and personalization docs.

### Task_4
- type: docs
- owns:
  - docs/personalization-roadmap.md
  - docs/llm-integration-readiness-contract.md
- depends_on:
  - Task_2
  - Task_3
- acceptance:
  - Add purposeful cross-links between the new docs and existing reference docs.
  - Ensure the roadmap points to current-truth docs rather than duplicating them.
  - Ensure completed plans are cited only as provenance, not as the long-term canonical home.
- validation:
  - required: true
    owner: reviewer
    kind: review
    detail: Docs-only validation from docs/coding-agent/references/validation.md is satisfied through reviewer clarity and terminology consistency checks.

## Validation Summary

- Docs-only task.
- Required validation passed through reviewer clarity, boundary, and consistency review plus terminology consistency against current durable docs.

## Progress Log

- 2026-03-13: Drafted plan after repo design-doc review and Researcher recommendation to split the durable output into roadmap and assistant-readiness companion docs under top-level docs/.
- 2026-03-13: Worker parallel wave created `docs/personalization-roadmap.md` and the LLM boundary companion doc, later canonicalized at `docs/llm-integration-readiness-contract.md`.
- 2026-03-13: Reviewer found two wording issues: drift-prone exact planning counts in the roadmap and an overstated read-boundary phrase in the assistant contract.
- 2026-03-13: Applied targeted wording fixes, re-ran review, and received approval with no remaining material findings.

## Decision Log

- 2026-03-13: Chose top-level docs/ instead of docs/coding-agent/ because the requested output is a durable product reference, not an execution artifact or harness-operation document.
- 2026-03-13: Chose a two-doc split so roadmap sequencing and assistant-readiness constraints can evolve independently without turning into one oversized strategy memo.
- 2026-03-13: Kept recent live-data analysis in the roadmap only as planning evidence, not as durable product truth, to avoid drift against upstream source docs.
