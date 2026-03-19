# Durable Personalization Doc Architecture Refresh Plan

- status: done
- owner: Orchestrator
- created: 2026-03-16
- completed: 2026-03-18
- objective: Refresh the durable personalization product docs so they clearly separate philosophy, roadmap sequencing, and LLM integration boundaries; replace ambiguous assistant terminology with explicit LLM language; and add durable freshness semantics.

## Scope

- In scope:
  - Revise top-level durable personalization docs.
  - Add or extract a philosophy/aspiration document if needed.
  - Restructure roadmap and boundary docs for clearer document roles.
  - Standardize LLM terminology and freshness metadata semantics.
  - Audit adjacent top-level product/domain docs for overlap and apply bounded follow-on polish where it materially improves the new document architecture.
- Out of scope:
  - Any product, API, schema, or UI behavior changes.
  - Changing execution plans beyond documenting the work.
  - Treating speculative integrations as current shipped capabilities.
  - Broad onboarding, runbook, API example, OpenAPI, or coding-agent harness documentation rewrites.

## Task Waves

- Wave 1: Task_1, Task_2
- Wave 2: Task_3, Task_4, Task_5
- Wave 3: Task_6, Task_7

## Tasks

### Task_1
- type: research
- owns:
  - docs/personalization-roadmap.md
  - docs/llm-integration-readiness-contract.md
  - docs/feature-reference.md
  - docs/personalization-metrics-shortlist.md
  - docs/personalization-agent-action-map.md
- depends_on: []
- acceptance:
  - Enumerate ambiguous uses of assistant terminology, mixed-role sections, and time-relative durability risks in the current docs.
  - Freeze one terminology policy and one freshness policy before drafting.
  - Ground the revision strategy in existing repo docs patterns.
  - Classify adjacent top-level docs by role: canonical current-truth, philosophy/strategy, dated evidence snapshot, policy/guardrail, or examples/how-to.
- validation:
  - required: true
    owner: orchestrator
    kind: evidence-review
    detail: The chosen structure, terminology, and freshness model must be justified against current repository docs rather than invented ad hoc.

### Task_2
- type: design
- owns:
  - docs/personalization-roadmap.md
  - docs/llm-integration-readiness-contract.md
  - docs/personalization-philosophy.md
  - docs/personalization-metrics-shortlist.md
  - docs/personalization-agent-action-map.md
  - docs/feature-reference.md
- depends_on:
  - Task_1
- acceptance:
  - Decide the durable doc architecture, including whether to add a separate philosophy doc.
  - Define metadata fields and section-level freshness semantics.
  - Define the canonical role of each durable doc.
  - Define a treatment matrix for adjacent docs: cross-link only, light polish, or explicitly out of scope.
- validation:
  - required: true
    owner: reviewer
    kind: clarity-review
    detail: The role of each doc is distinct and non-overlapping.

### Task_3
- type: docs
- owns:
  - docs/personalization-philosophy.md
- depends_on:
  - Task_2
- acceptance:
  - Create or extract a philosophy/aspiration doc for durable product beliefs, north-star goals, and non-goals.
  - Keep the content philosophy-level and avoid milestone sequencing or endpoint detail.
  - Cross-link it appropriately to roadmap and current-truth docs.
- validation:
  - required: true
    owner: reviewer
    kind: clarity-review
    detail: The resulting doc reads as durable philosophy rather than roadmap or implementation planning.

### Task_4
- type: docs
- owns:
  - docs/personalization-roadmap.md
- depends_on:
  - Task_2
  - Task_3
- acceptance:
  - Refactor the roadmap so it foregrounds aspiration, milestones, and dependency gates rather than mixed philosophy-plus-detail prose.
  - Anchor or reduce time-sensitive evidence language.
  - Keep current shipped truth delegated to docs/feature-reference.md and related canonical docs.
- validation:
  - required: true
    owner: reviewer
    kind: consistency-review
    detail: Roadmap language must not overstate shipped behavior and must stay distinct from philosophy and boundary detail.

### Task_5
- type: docs
- owns:
  - docs/llm-integration-readiness-contract.md
- depends_on:
  - Task_2
- acceptance:
  - Refactor or retitle the contract so its scope is explicitly about LLM integration rather than generic automation.
  - Make allowed reads, prohibited actions, and readiness prerequisites explicit.
  - Add durable metadata and freshness semantics.
- validation:
  - required: true
    owner: reviewer
    kind: boundary-review
    detail: The contract must not imply unsupported current LLM capabilities or integrations.

### Task_6
- type: docs
- owns:
  - docs/personalization-philosophy.md
  - docs/personalization-roadmap.md
  - docs/llm-integration-readiness-contract.md
- depends_on:
  - Task_3
  - Task_4
  - Task_5
- acceptance:
  - Add cross-links and metadata consistently across the revised durable docs.
  - Remove or anchor unqualified uses of time-relative language.
  - Ensure terminology is precise for LLM versus deterministic automation.
- validation:
  - required: true
    owner: reviewer
    kind: review
    detail: Docs-only validation from docs/coding-agent/references/validation.md is satisfied via clarity, consistency, and terminology/freshness sweep.

### Task_7
- type: docs
- owns:
  - docs/personalization-metrics-shortlist.md
  - docs/personalization-agent-action-map.md
  - docs/feature-reference.md
- depends_on:
  - Task_2
  - Task_3
  - Task_4
  - Task_5
- acceptance:
  - Apply only the bounded adjacent-doc touches selected by the treatment matrix.
  - Keep `docs/personalization-metrics-shortlist.md` clearly separated between dated snapshot basis and durable shortlist guidance.
  - Tighten `docs/personalization-agent-action-map.md` so its role is guarded proposal policy, not implied LLM capability approval.
  - Limit `docs/feature-reference.md` changes to targeted cross-links or brief companion notes, preserving it as canonical shipped truth.
- validation:
  - required: true
    owner: reviewer
    kind: clarity-review
    detail: Every touched adjacent document must have one clear role and must not duplicate another document's primary job.
  - required: true
    owner: reviewer
    kind: freshness-review
    detail: Dated snapshot material must remain explicitly time-scoped and must not be presented as durable current truth.
  - required: true
    owner: reviewer
    kind: terminology-review
    detail: Touched docs must distinguish shipped product behavior, strategy, and future LLM boundaries consistently.

## Validation Summary

- Docs-only task.
- Required validation passed through reviewer clarity, consistency, boundary, terminology, freshness, and cross-link review.

## Progress Log

- 2026-03-16: Drafted plan after a user correction identified ambiguous assistant terminology, weak philosophy-versus-detail separation, and missing freshness semantics in the durable product docs.
- 2026-03-16: Expanded the draft scope after an adjacent-doc audit identified bounded follow-on work in `docs/personalization-metrics-shortlist.md`, `docs/personalization-agent-action-map.md`, and cross-link-only touches in `docs/feature-reference.md`.
- 2026-03-18: Worker wave created `docs/personalization-philosophy.md`, refactored `docs/personalization-roadmap.md`, and reframed the boundary document into an explicit LLM integration contract, later canonicalized at `docs/llm-integration-readiness-contract.md`.
- 2026-03-18: Worker follow-on wave applied bounded adjacent-doc polish to `docs/personalization-metrics-shortlist.md`, `docs/personalization-agent-action-map.md`, and `docs/feature-reference.md`.
- 2026-03-18: Reviewer approved the six-document set with no remaining required fixes.

## Decision Log

- 2026-03-16: Treat document-role architecture as a prerequisite to rewriting content so philosophy, roadmap sequencing, and LLM integration boundaries can evolve independently.
- 2026-03-16: Treat explicit LLM terminology and freshness metadata as durable defaults for future product-doc work rather than one-off fixes to these files.
- 2026-03-16: Keep adjacent-doc polish intentionally narrow; leave `docs/api_examples.md`, `README.md`, and other non-overlapping docs alone unless link fallout requires a minimal fix.
- 2026-03-18: Prefer a canonical filename that matches the LLM-specific document role; preserve the old path only as a historical compatibility stub when needed for older references.
