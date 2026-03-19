# SleepTracker LLM Integration Boundary Contract

## Metadata

- status: active
- last_updated: 2026-03-20
- doc_role: durable boundary and readiness contract for LLM integration in SleepTracker
- canonical_for: approved LLM read boundary, prohibited LLM actions, and prerequisites for LLM-generated drafts or LLM-mediated writes
- not_canonical_for: shipped feature inventory, roadmap sequencing, API behavior, or approval of speculative integrations
- freshness_semantics: durable by design; use companion docs for shipped status, staged sequencing, and dated planning evidence

## Purpose

This document defines the durable boundary for LLM integration in SleepTracker and the conditions that must be met before any LLM-generated draft or LLM-mediated write path becomes roadmap-ready.

It is a guardrail reference, not an implementation spec. It describes the approved LLM read posture, the actions that remain out of scope, and the prerequisites for any staged progression toward drafts or user-confirmed writes.

## Terminology And Scope

For this document, the following terms are used consistently:

- LLM-powered surface or LLM integration: any product surface where a large language model reads SleepTracker data or docs to generate summaries, recommendations, drafts, or mutation requests.
- Deterministic automation: rule-based or explicitly programmed system behavior that does not rely on LLM generation, such as existing product CRUD handlers, auth flows, or scheduled internal logic.
- First-party product flow: a shipped SleepTracker user flow where the user directly reads or mutates their own data through the product UI or API.

This contract governs LLM integration boundaries only. It does not redefine what first-party product flows already support, and it does not treat existing deterministic automation or user-driven CRUD as LLM-approved behavior.

## Allowed LLM Read Boundary

The approved LLM boundary is read-first and limited to shipped, first-party product reads plus durable companion docs that describe those reads.

### Product reads within boundary

An LLM-powered surface may analyze the following read-accessible product information when those reads are already available through authenticated first-party product flows:

- Session and auth state needed to determine whether protected reads are available.
- Sleep history reads, including range, day, and item detail views.
- Exercise intensity reads used as context in existing product flows.
- Trends reads for sleep bars and related chart or schedule interpretation.
- Settings reads that affect existing first-party behavior, such as timezone.
- Personalization analysis reads that expose metrics, recommendation outputs, and friction backlog proposals.

These approved reads do not extend to external systems, inferred conversations, speculative capture channels, or undocumented integrations.

### Documentation reads within boundary

An LLM-powered surface may also rely on durable companion docs that explain shipped behavior, metrics, and guardrails:

- [feature-reference.md](./feature-reference.md) for shipped product behavior and constraints.
- [personalization-metrics-shortlist.md](./personalization-metrics-shortlist.md) for the evidence-backed metric shortlist and gating thresholds.
- [personalization-agent-action-map.md](./personalization-agent-action-map.md) for proposal guardrails, confidence thresholds, and rollback policy.
- [personalization-roadmap.md](./personalization-roadmap.md) for phased direction without redefining shipped product truth.
- [personalization-philosophy.md](./personalization-philosophy.md) for durable personalization beliefs, non-goals, and trust boundaries.

## Explicitly Out Of Scope Actions

The following actions are out of scope for any LLM-powered surface unless a later shipped design explicitly changes this contract and its companion docs:

- Creating, editing, or deleting sleep sessions.
- Creating or updating exercise intensity entries.
- Creating notes or changing existing notes.
- Posting settings changes on the user's behalf.
- Triggering logout or any other mutating session action.
- Writing friction telemetry as an LLM side effect.
- Auto-applying personalization proposals or promoting backlog items into product changes.
- Generating product-visible drafts without a dedicated draft and provenance model.
- Writing to calendars, reminders, tasks, messaging systems, or any non-SleepTracker destination.

The existence of user-driven CRUD endpoints does not make those endpoints LLM-approved. Existing mutation support belongs to first-party product flows, not to LLM integration.

## Prerequisites For LLM-Generated Drafts Or LLM-Mediated Writes

No LLM-generated draft flow or LLM-mediated write path should be considered roadmap-ready until all of the following foundations exist and are credible in production:

### 1. Friction telemetry strong enough to justify intervention

LLM-generated proposals should be informed by observed user friction, not by assumption. The product needs sustained friction telemetry that can identify repeated workflow pain, estimate likely benefit, and distinguish persistent problems from one-off noise.

### 2. Richer context capture

Shipped product truth does not yet provide a robust context layer for intent capture beyond existing first-party records and derived metrics. Before LLM-generated drafts or writes are considered, the product needs richer structured context about what the user is trying to do, why the suggestion is relevant, and which existing signals support it.

### 3. Drafts and provenance

Any later-stage LLM-generated change must first exist as a draft with clear provenance. The product needs a model that records what was proposed, which inputs informed it, when it was generated, and how it differs from saved data.

Without drafts and provenance, there is no durable review boundary and no safe authorship trail.

### 4. Explicit confirmation

The user must have a clear confirmation step before any LLM-originated mutation is committed. Confirmation must make the proposed change, its basis, and its target object obvious enough to prevent silent or ambiguous writes.

### 5. Reversible flows

Every LLM-originated change path needs a straightforward rollback or undo path. Reversibility is required for user trust and for operational safety when suggestions are wrong, stale, or based on incomplete context.

### 6. Stronger outcome feedback

The product needs outcome feedback that can show whether LLM suggestions or LLM-mediated write flows actually help. This includes evidence about acceptance, abandonment, corrections after acceptance, and whether the proposed action improved the intended user outcome.

Without stronger feedback, the product cannot distinguish useful LLM behavior from plausible but ineffective automation.

## Staged Graduation Path

LLM integration should graduate in stages. Each stage depends on the guardrails from the earlier stage remaining intact.

### Stage 1. Read-only analysis

The LLM reads first-party product data and durable docs, then produces explanations, summaries, prioritization, and recommendations. No drafts are persisted. No writes are attempted.

### Stage 2. Draft with provenance

The LLM may generate a non-applied draft artifact tied to explicit provenance, source inputs, and target records. The draft is reviewable, attributable, and separate from saved user data.

### Stage 3. User-confirmed writes

The LLM may submit a mutation only after the user reviews and explicitly confirms a draft through a reversible flow with clear authorship and outcome tracking.

### Stage 4. Broader LLM-powered surfaces

Only after draft safety, confirmation quality, rollback behavior, and outcome feedback are all proven should broader LLM-powered surfaces be considered. Even then, they must remain constrained by first-party trust boundaries and shipped product evidence.

## Guardrails

- Default to the most conservative stage when evidence is incomplete.
- If provenance, confirmation, or reversibility becomes unclear, fall back to read-only analysis.
- If an LLM-generated proposal cannot explain its basis in first-party product data or durable docs, it must not progress.
- If telemetry shows low trust, high correction rates, or weak outcome improvement, stop graduation and roll back the affected surface.
- Any later-stage LLM-mediated write path must stay narrower than general user CRUD until its safeguards are proven over time.
- Roadmap discussion does not count as approval to ship LLM-mediated writes.

## Companion Docs

Read this contract alongside the following durable references:

- [feature-reference.md](./feature-reference.md) for shipped features, authenticated reads, and user-driven mutations.
- [personalization-metrics-shortlist.md](./personalization-metrics-shortlist.md) for evidence-backed personalization metrics and gating thresholds.
- [personalization-agent-action-map.md](./personalization-agent-action-map.md) for proposal guardrails, confidence thresholds, and rollback conditions.
- [personalization-roadmap.md](./personalization-roadmap.md) for the companion phased roadmap covering sequenced direction without redefining shipped product truth.
- [personalization-philosophy.md](./personalization-philosophy.md) for the durable product beliefs and non-goals that constrain any LLM expansion.

If any later-stage proposal conflicts with shipped product truth in those documents, interpret this contract conservatively and keep the LLM surface read-only until the missing prerequisites are explicitly satisfied.
