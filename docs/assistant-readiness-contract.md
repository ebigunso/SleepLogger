# Assistant Readiness Contract

## Purpose

This document defines the durable boundary for assistant behavior in SleepTracker and the conditions that must be met before any future assistant write path becomes roadmap-ready.

It is a guardrail reference, not an implementation spec. It describes current product truth, the approved read posture for assistance, and the prerequisites for any later progression toward drafts or user-confirmed writes.

## Intended consumers

- Product and roadmap authors deciding what assistant capabilities are in or out of scope.
- Engineers and reviewers checking whether proposed assistant features match current product truth.
- Future planning work that needs a stable boundary reference separate from execution plans.

## Current assistant boundary

SleepTracker is assistance-first and read-first today.

The current product supports authenticated direct user CRUD flows plus authenticated read endpoints. Users can create, edit, and delete their own records through first-party product flows, but there is no assistant-controlled write surface, no draft object model, no provenance layer for assistant-authored changes, and no confirmation workflow that would make assistant writes safe or reviewable.

As a result, the assistant boundary today is:

- Analyze current first-party product data through approved read surfaces.
- Summarize, interpret, and prioritize information already available in the product and its durable reference docs.
- Do not create, edit, delete, submit, or synchronize user data.
- Do not imply support for external tool invocation, conversational capture, calendar ingestion, or task-system integration.

## Approved read surfaces today

Approved assistant reads must stay inside current product capabilities and current durable source documents.

### Product read capabilities

The safe read surface today is limited to authenticated first-party product reads and their existing derived outputs:

- Session and auth state needed to determine whether protected reads are available.
- Sleep history reads, including range, day, and item detail views.
- Exercise intensity reads used as context in existing product flows.
- Trends reads for sleep bars and related chart or schedule interpretation.
- Settings reads that affect current first-party behavior, such as timezone.
- Personalization analysis reads that expose metrics, recommendation outputs, and friction backlog proposals.

These reads are grounded in current shipped behavior described in the feature reference and current personalization docs. They do not extend to external systems, inferred conversations, or speculative integrations.

### Durable source documents

The assistant may also rely on durable documentation that explains current behavior, metrics, and guardrails:

- `docs/feature-reference.md` for current implemented product behavior and constraints.
- `docs/personalization-metrics-shortlist.md` for the current metric shortlist, go or no-go gates, and rollout order.
- `docs/personalization-agent-action-map.md` for current trigger, confidence, guardrail, and rollback policy around personalization proposals.
- `docs/personalization-roadmap.md` as the companion roadmap reference for phased future direction, once present.

## Explicitly out of scope today

The following write behaviors are explicitly out of scope for the assistant today:

- Creating, editing, or deleting sleep sessions.
- Creating or updating exercise intensity entries.
- Creating notes or changing existing notes.
- Posting settings changes on the user's behalf.
- Triggering logout or any other mutating session action.
- Writing friction telemetry as an assistant side effect.
- Auto-applying personalization proposals or promoting backlog items into product changes.
- Generating product-visible drafts without a dedicated draft and provenance model.
- Writing to external calendars, reminders, tasks, messaging systems, or any non-SleepTracker destination.

The presence of user-driven CRUD endpoints does not make them assistant-approved. Current mutation support exists for direct product use, not for assistant execution.

## Prerequisites for future assistant write readiness

No assistant draft generation or write path should be considered roadmap-ready until all of the following foundations exist and are credible in production:

### 1. Friction telemetry strong enough to justify intervention

Assistant write proposals should be informed by observed user friction, not by assumption. The system needs sustained friction telemetry that can identify repeated workflow pain, estimate likely benefit, and distinguish persistent problems from one-off noise.

### 2. Richer context capture

Current product truth does not include a robust context layer for intent capture beyond existing first-party records and derived metrics. Before assistant-generated drafts or writes are considered, the product needs richer structured context about what the user is trying to do, why the suggestion is relevant, and which current signals support it.

### 3. Drafts and provenance

Any future assistant-generated change must first exist as a draft with clear provenance. The system needs a model that records what was proposed, which inputs informed it, when it was generated, and how it differs from current saved data.

Without drafts and provenance, there is no durable review boundary and no safe authorship trail.

### 4. Explicit confirmation

The user must have a clear confirmation step before any assistant-originated mutation is committed. Confirmation must make the proposed change, its basis, and its target object obvious enough to prevent silent or ambiguous writes.

### 5. Reversible flows

Every assistant-originated change path needs a straightforward rollback or undo path. Reversibility is required both for user trust and for operational safety when suggestions are wrong, stale, or based on incomplete context.

### 6. Stronger outcome feedback

The product needs outcome feedback that can show whether assistant suggestions or write flows actually help. This includes evidence about acceptance, abandonment, corrections after acceptance, and whether the proposed action improved the intended user outcome.

Without stronger feedback, the system cannot distinguish useful assistant behavior from plausible but ineffective automation.

## Graduation path

Future assistant capability should graduate in stages. Each stage depends on the guardrails from the earlier stage remaining intact.

### Stage 1. Read-only analysis

The assistant reads current first-party product data and durable docs, then produces explanations, summaries, prioritization, and recommendations. No drafts are persisted. No writes are attempted.

### Stage 2. Draft with provenance

The assistant may generate a non-applied draft artifact tied to explicit provenance, source inputs, and target records. The draft is reviewable, attributable, and separate from saved user data.

### Stage 3. User-confirmed writes

The assistant may submit a mutation only after the user reviews and explicitly confirms a draft through a reversible flow with clear authorship and outcome tracking.

### Stage 4. Broader assistant surfaces

Only after draft safety, confirmation quality, rollback behavior, and outcome feedback are all proven should broader assistant surfaces be considered. Broader surfaces still need to remain constrained by first-party trust boundaries and current product evidence.

## Guardrails and rollback expectations

- Default to the most conservative stage when evidence is incomplete.
- If provenance, confirmation, or reversibility becomes unclear, fall back to read-only analysis.
- If an assistant proposal cannot explain its basis in current first-party data or durable docs, it should not progress.
- If telemetry shows low trust, high correction rates, or weak outcome improvement, stop graduation and roll back the affected surface.
- Any future assistant write path must be narrower than general user CRUD until its safeguards are proven over time.
- Roadmap discussion does not count as approval to ship assistant writes.

## Source references

This contract should be read alongside the following durable references:

- `docs/feature-reference.md` for current implemented features, authenticated reads, and user-driven mutations.
- `docs/personalization-metrics-shortlist.md` for the current evidence-backed personalization metrics and gating thresholds.
- `docs/personalization-agent-action-map.md` for guardrails, confidence thresholds, and rollback conditions around personalization proposals.
- `docs/personalization-roadmap.md` for the companion phased roadmap covering short-term and longer-horizon direction without redefining current product truth.

If any future proposal conflicts with current product truth in those documents, this contract should be interpreted conservatively and the proposal should remain read-only until the missing prerequisites are explicitly satisfied.