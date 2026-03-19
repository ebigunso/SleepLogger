# SleepTracker Personalization Roadmap

## Metadata

- status: draft
- last_updated: 2026-03-18
- doc_role: phased personalization roadmap and dependency reference
- canonical_for: planned personalization milestones, sequencing, dependency gates, and explicit non-commitments
- not_canonical_for: shipped product behavior, durable product philosophy, metric definitions, action policy, or implementation status claims
- freshness_semantics: directional by design; treat shipped status and dated evidence as anchored to linked companion docs rather than as standalone truth here

## Purpose

This document is the roadmap for how SleepTracker personalization should mature from better evidence-backed guidance in shipped surfaces toward possible later-stage LLM-powered product surfaces and integrations.

Use it to understand milestone order, readiness gates, and what is intentionally deferred. Do not use it as proof that a capability is already shipped, approved, or scheduled for a specific release.

For durable product beliefs, aspirations, and non-goals beyond milestone interpretation, use [personalization-philosophy.md](./personalization-philosophy.md) when consulting the personalization document set.

## How to read this roadmap

- Treat every milestone here as directional and gated.
- Treat dated evidence as planning input, not floating current truth.
- Use [feature-reference.md](./feature-reference.md) for shipped behavior.
- Use [personalization-metrics-shortlist.md](./personalization-metrics-shortlist.md) for the current metric shortlist, thresholds, and dated planning snapshot basis.
- Use [personalization-agent-action-map.md](./personalization-agent-action-map.md) for trigger logic, guardrails, confidence thresholds, and rollback policy.
- Use [llm-integration-readiness-contract.md](./llm-integration-readiness-contract.md) for the governing boundary on later-stage LLM-powered product integrations and any progression toward drafts or writes.

In this roadmap, "LLM-powered" means product surfaces or integrations that use an LLM to explain, structure, draft, or mediate personalization guidance. It does not imply shipped autonomous behavior, write approval, or external tool access. Deterministic automation and first-party product flows remain separate categories from LLM-powered behavior.

## Baseline and source-of-truth pointers

Shipped personalization behavior is anchored to [feature-reference.md](./feature-reference.md), including the implemented rolling-window metrics, recommendation outputs, friction telemetry ingestion endpoint, and friction backlog endpoint.

Metric and action logic that informs milestone order is anchored to [personalization-metrics-shortlist.md](./personalization-metrics-shortlist.md) and [personalization-agent-action-map.md](./personalization-agent-action-map.md). This roadmap can summarize why those sources matter, but it does not replace them.

The most explicit dated planning evidence in the durable doc set is the 2026-02-17 backend snapshot captured in [personalization-metrics-shortlist.md](./personalization-metrics-shortlist.md). As of that dated snapshot:

- personal duration, timing, regularity, and social jetlag signals were strong enough to justify foundational roadmap priority
- exercise-conditioned and notes-conditioned signals were still weak and should remain secondary
- friction cost metrics were high value in principle but under-instrumented in practice

If subsequent planning analysis changes milestone order, that analysis should be referenced as a dated input or promoted into the relevant canonical companion doc rather than asserted here as undated current truth.

## Foundational milestones

This sequence focuses on making shipped personalization more trustworthy, more interpretable, and better instrumented inside existing product surfaces.

### 1. Recalibrate personal baselines

Primary outcome: move from coarse static interpretation toward stronger personal baseline windows where sample quality supports it.

Milestone intent:

- recalibrate unusual-duration and timing interpretation against personal baseline windows instead of relying only on fixed global heuristics
- emphasize regularity and social jetlag interpretation because they are the strongest dated signals in the current evidence base
- keep exercise-conditioned and notes-conditioned logic secondary until those signals improve

Why this milestone belongs in the foundational sequence:

- it builds directly on the current metric shortlist rather than requiring a new product category
- it improves trust in already shipped personalization outputs before expanding scope

### 2. Make friction telemetry decision-useful

Primary outcome: turn the existing friction telemetry path into planning evidence that can justify backlog ranking and UX changes.

Milestone intent:

- verify capture coverage across relevant user flows
- record enough context to explain failed, delayed, or repaired interactions
- require stable reviewable evidence before friction backlog ranking drives prioritization

Why this milestone belongs in the foundational sequence:

- friction cost metrics are high leverage only when they are observed, not assumed
- stronger instrumentation is a prerequisite for subsequent automation or LLM-mediated product decisions

### 3. Add lightweight context capture

Primary outcome: improve read-time interpretation by collecting small amounts of structured context that explain timing changes without creating a heavy journaling workflow.

Milestone intent:

- prioritize context that explains schedule shifts, irregularity, and recovery patterns
- keep structured capture explicit, minimal, and reviewable
- avoid treating free text or sparse notes as a primary personalization driver

Why this milestone belongs in the foundational sequence:

- strong timing signals are more useful when the product can distinguish intentional shifts from noise
- a minimal context model is needed before any later-stage LLM-powered drafting or mediation can be trustworthy

### 4. Improve guidance in shipped surfaces first

Primary outcome: make existing surfaces more helpful before introducing a separate LLM-powered surface or integration.

Milestone intent:

- improve guidance in shipped dashboard, trends, and sleep-entry experiences
- favor read-time insights, recommended defaults, and contextual warnings over autonomous action
- keep wording directional and evidence-based, consistent with the current action-map guardrails

Why this milestone belongs in the foundational sequence:

- it tests product value where the product already has user attention
- it establishes whether assistance-first behavior is useful before any broader interface expansion stage

### 5. Establish a read-safe LLM integration boundary

Primary outcome: keep later-stage LLM-powered product surfaces and integrations read-first until stronger safeguards exist.

Milestone intent:

- require a stable read contract before any LLM-powered integration can mediate personalization state
- keep read readiness separate from draft readiness and write readiness
- use [llm-integration-readiness-contract.md](./llm-integration-readiness-contract.md) as the governing boundary for this progression

Why this milestone belongs in the foundational sequence:

- roadmap language should make the boundary explicit before LLM-related ideas expand
- subsequent milestones should inherit this gate rather than reinterpret it ad hoc

## Conditional expansion milestones

These are conditional expansion directions, not current commitments. Any LLM-related item below refers to LLM-powered product surfaces or integrations and remains gated until the dependency gates in this roadmap are met.

### 1. Drafts with provenance for LLM-powered suggestions

Target outcome: LLM-powered suggestions can become reviewable drafts with visible provenance, evidence, and target objects before any user-confirmed write path exists.

This remains sequenced after the foundational gates because it depends on a trustworthy read boundary, stronger context capture, and a clear confirmation model.

### 2. Conversational structured capture

Target outcome: an LLM-powered surface can help turn user conversation into explicit structured context fields rather than opaque memory.

This remains sequenced after the foundational gates because structured capture should extend a proven minimal context model, not replace it, and the current sparse-note evidence base does not justify it as a default milestone.

### 3. Dedicated LLM-powered interface or MCP-style integration

Target outcome: personalization insights and read-safe operations can be exposed through a dedicated LLM-powered interface or MCP-style contract.

This remains sequenced after the foundational gates because interface expansion without stable read semantics would create capability ambiguity faster than product value.

### 4. Calendar-aware interpretation

Target outcome: planned schedule anchors can help distinguish deliberate shifts from irregularity and improve interpretation of consistency and recovery needs.

This remains sequenced after the foundational gates because external context should follow a trustworthy internal data and lightweight context foundation.

### 5. Daytime outcome labels

Target outcome: simple daytime outcome labels can connect nighttime patterns with next-day function and improve prioritization beyond duration and timing alone.

This remains sequenced after the foundational gates because it introduces new collection burden and should follow proof that the sleep-focused guidance loop is already useful.

### 6. Recovery coach evolving toward broader planning

Target outcome: SleepTracker can eventually move from recovery-oriented interpretation toward broader planning guidance across timing, consistency, and upcoming demands.

This remains sequenced after the foundational gates because it requires stronger context, better outcome feedback, and tighter LLM-readiness controls than the product's documented baseline currently allows.

## Dependency gates

Milestones can move at different speeds, but the sequence should respect these gates.

### Gate 1. Evidence sufficiency for recalibration

- personal baselines and recent-window patterns are stable enough to support recalibration
- regularity and social jetlag signals continue to outweigh weaker exercise and notes signals

### Gate 2. Friction observability

- friction telemetry captures enough real events to support ranking and prioritization
- backlog proposals are justified by observed evidence rather than inferred pain

### Gate 3. Minimal context model

- the product has a lightweight context layer that improves interpretation without imposing heavy user input cost
- context fields are explicit enough to support provenance and subsequent LLM-powered mediation

### Gate 4. Assistance-first product fit in shipped surfaces

- read-time recommendations inside shipped surfaces show clear product value
- recommendation wording and ranking remain guarded, evidence-based, and reversible

### Gate 5. Read-safe LLM integration contract

- external or cross-surface LLM-powered consumers can read relevant personalization state through a stable contract
- the read boundary is documented and accepted before any draft or write path is considered

### Gate 6. Draft, confirmation, and rollback model

- later-stage LLM-mediated changes can be represented as drafts with provenance, user review, and rollback semantics
- no write-capable LLM path should proceed without this gate

### Gate 7. Outcome feedback

- the product can measure whether suggestions, drafts, or subsequent mediated actions actually help
- low-trust or high-correction behavior can be detected early enough to stop graduation

## Deferred items and non-commitments

- This roadmap does not make any milestone a shipped truth claim.
- This roadmap does not approve autonomous writes, silent preference mutation, or opaque LLM memory.
- Exercise-conditioned and notes-conditioned personalization should not become primary roadmap drivers while their signals remain weak.
- A dedicated LLM-powered surface or integration should not outrank value delivery in shipped surfaces before the earlier gates are met.
- Broad wellness, cohort analytics, or unrelated automation should not be folded into personalization just because the product may add additional LLM-powered features in a later stage.
- Undated planning observations should not replace the canonical companion docs.

## Companion docs

- Shipped capabilities and constraints: [feature-reference.md](./feature-reference.md)
- Current metric priorities, thresholds, and dated snapshot basis: [personalization-metrics-shortlist.md](./personalization-metrics-shortlist.md)
- Current action logic, guardrails, and rollback policy: [personalization-agent-action-map.md](./personalization-agent-action-map.md)
- Current LLM integration boundary and staged write-readiness gates: [llm-integration-readiness-contract.md](./llm-integration-readiness-contract.md)
- Durable personalization beliefs and north-star framing: [personalization-philosophy.md](./personalization-philosophy.md)
