# Durable Doc Emphasis Refactor Plan

- status: done
- owner: Orchestrator
- created: 2026-03-20
- completed: 2026-03-20
- objective: Refactor the durable personalization docs so they foreground the most important story, tier foundational versus supporting material, and reduce flat equal-weight presentation without changing the established doc roles.

## Scope

- In scope:
  - Refactor narrative ordering, section tiering, and emphasis structure in the durable personalization doc set.
  - Preserve the distinct document roles already established for philosophy, roadmap, proposal policy, metrics shortlist, LLM boundary contract, and shipped-truth reference.
  - Reduce repeated boundary-setting language when it obscures the main point.
- Out of scope:
  - Product strategy changes beyond presentation and emphasis.
  - API, schema, or UI behavior changes.
  - Broad documentation architecture changes or new top-level docs.

## Task Waves

- Wave 1: Task_1
- Wave 2: Task_2, Task_3, Task_4
- Wave 3: Task_5, Task_6

## Tasks

### Task_1
- type: design
- owns:
  - docs/personalization-philosophy.md
  - docs/personalization-roadmap.md
  - docs/personalization-proposal-policy.md
  - docs/llm-integration-readiness-contract.md
  - docs/personalization-metrics-shortlist.md
  - docs/feature-reference.md
- depends_on: []
- acceptance:
  - Freeze a clear emphasis model for the doc set: primary claim first, tiered supporting material, and reference detail later.
  - Identify which docs need structural refactors versus only ordering changes.
  - Preserve current document-role boundaries while improving rhetorical weighting.
- validation:
  - required: true
    owner: orchestrator
    kind: evidence-review
    detail: The refactor scope must be grounded in the completed research audit and must clearly distinguish structural changes from role changes.

### Task_2
- type: docs
- owns:
  - docs/personalization-roadmap.md
- depends_on:
  - Task_1
- acceptance:
  - Make the roadmap story visible within the first screen: improve shipped personalization first, strengthen evidence and context, then consider gated expansion.
  - Separate foundational product work from later-stage LLM boundary and expansion branches.
  - Tier dependency gates into foundational proof gates and later-stage LLM graduation gates.
- validation:
  - required: true
    owner: reviewer
    kind: clarity-review
    detail: The roadmap must foreground the main sequence and stop presenting product priorities, safety boundaries, and speculative branches as flat peers.

### Task_3
- type: docs
- owns:
  - docs/personalization-proposal-policy.md
- depends_on:
  - Task_1
- acceptance:
  - Promote the governing proposal-policy spine before supporting endpoint mappings and templates.
  - Keep the metric-to-action matrix as the dominant core section.
  - Demote templates and UI-specific support material into later or clearly secondary sections.
- validation:
  - required: true
    owner: reviewer
    kind: clarity-review
    detail: The proposal-policy doc must clearly communicate what qualifies a deterministic proposal before presenting supporting reference blocks.

### Task_4
- type: docs
- owns:
  - docs/personalization-philosophy.md
  - docs/personalization-metrics-shortlist.md
- depends_on:
  - Task_1
- acceptance:
  - Group philosophy principles into visible tiers so foundational beliefs are distinct from derived constraints and expansion limits.
  - Make the shortlist signal earlier which metrics are current primary drivers, which are enabling instrumentation, and which remain deferred.
  - Reduce flat same-weight bullet runs where tiering would clarify priorities.
- validation:
  - required: true
    owner: reviewer
    kind: clarity-review
    detail: Foundational versus supporting ideas must be visibly distinct in both docs.

### Task_5
- type: docs
- owns:
  - docs/llm-integration-readiness-contract.md
  - docs/feature-reference.md
- depends_on:
  - Task_2
  - Task_3
  - Task_4
- acceptance:
  - Apply only light ordering and boundary-tuning changes needed to keep the LLM contract and feature reference from competing with the main narrative docs.
  - Group hard safety gates versus readiness evidence in the LLM contract where useful.
  - Keep the feature reference terse and clearly in shipped-truth mode.
- validation:
  - required: true
    owner: reviewer
    kind: consistency-review
    detail: These docs must remain support anchors and must not absorb emphasis that belongs in philosophy, roadmap, or proposal policy.

### Task_6
- type: review
- owns:
  - docs/personalization-philosophy.md
  - docs/personalization-roadmap.md
  - docs/personalization-proposal-policy.md
  - docs/llm-integration-readiness-contract.md
  - docs/personalization-metrics-shortlist.md
  - docs/feature-reference.md
- depends_on:
  - Task_2
  - Task_3
  - Task_4
  - Task_5
- acceptance:
  - Verify that each doc shows its governing claim or main story within the first screen.
  - Verify that long peer lists have been replaced or clarified with tiers where appropriate.
  - Verify that repeated caveats and companion routing no longer compete with the main story.
- validation:
  - required: true
    owner: reviewer
    kind: review
    detail: Docs-only validation from docs/coding-agent/references/validation.md is satisfied through clarity, ordering, cross-link, and terminology review.

## Validation Summary

- Docs-only task.
- Required validation passed through reviewer clarity, ordering, cross-link, and terminology review.

## Progress Log

- 2026-03-20: Drafted plan after a research audit confirmed that the durable doc roles are mostly correct, but emphasis remains too flat, especially in the roadmap and proposal-policy docs.
- 2026-03-20: Worker wave refactored `docs/personalization-roadmap.md`, `docs/personalization-proposal-policy.md`, `docs/personalization-philosophy.md`, and `docs/personalization-metrics-shortlist.md` to foreground the main story and visibly tier foundational versus supporting material.
- 2026-03-20: Worker follow-on wave applied light support-doc tuning to `docs/llm-integration-readiness-contract.md` and `docs/feature-reference.md` so they stayed in support-anchor roles.
- 2026-03-20: Reviewer approved the six-document set with no required fixes.

## Decision Log

- 2026-03-20: Keep the existing doc architecture and refactor emphasis within it rather than reopening the philosophy/roadmap/boundary split.
- 2026-03-20: Prioritize rhetorical weighting and narrative ordering over adding more coverage or more caveats.
