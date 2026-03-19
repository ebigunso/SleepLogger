# SleepTracker Personalization Philosophy

## Metadata

- status: draft
- last_updated: 2026-03-18
- doc_role: durable product philosophy and aspiration reference for personalization
- canonical_for: core personalization beliefs, north-star aspirations, durable non-goals, and trust boundaries
- not_canonical_for: shipped feature inventory, roadmap sequencing, implementation details, endpoint behavior, or readiness approval for LLM integrations
- freshness_semantics: durable by design; use companion docs for shipped status, dated evidence snapshots, and staged sequencing

## Purpose

This document defines the durable product philosophy for SleepTracker personalization.

It explains what personalization is meant to optimize for, which trust boundaries should remain stable as the product evolves, and which kinds of expansion only make sense after stronger evidence exists.

This page is intentionally not a roadmap and not a record of shipped behavior. It should remain useful even as implementation details, milestones, and specific interfaces change.

## How to use this doc

- Use this page when deciding whether a personalization idea fits the product's long-term direction.
- Use [personalization-roadmap.md](./personalization-roadmap.md) for sequencing, dependency gates, and phased direction.
- Use [feature-reference.md](./feature-reference.md) for implemented behavior and first-party product truth.
- Use [llm-integration-readiness-contract.md](./llm-integration-readiness-contract.md) for explicit boundaries and prerequisites around LLM-powered integrations.
- Use [personalization-metrics-shortlist.md](./personalization-metrics-shortlist.md) for the evidence-backed metric families that are strongest enough to prioritize.
- Use [personalization-proposal-policy.md](./personalization-proposal-policy.md) for guarded proposal logic, triggers, and rollback expectations.

If this document and a companion document appear to disagree, defer to the companion doc for its own domain: shipped truth belongs in the feature reference, sequencing belongs in the roadmap, and LLM integration boundaries belong in the readiness contract.

## Core product beliefs

### Personalization should be evidence-first

SleepTracker personalization should be grounded in user-specific evidence before it becomes more adaptive, more assertive, or more automated. Strong recurring signal matters more than plausible theory. The product should prefer patterns that can be traced to repeated user data over rules that merely sound helpful.

### User-trusted data outranks convenience

The product should favor trustworthy, reviewable inputs over shortcuts that create ambiguity. If a behavior would be easier to ship but would weaken trust in where a recommendation came from, the trust-preserving path is the right default.

### Real signal should outrank weak correlation

Not every captured field deserves equal influence. Personalization should emphasize timing regularity, schedule shift, duration baselines, and other signals that show stable value. Weak or sparse signals should remain secondary until they become strong enough to justify product weight.

### Helpfulness should start inside first-party product flows

The product should first become more useful inside its own trusted surfaces: logging, review, dashboard, and trends experiences. Personalization should improve interpretation, defaults, and guidance inside those flows before expanding into separate LLM-powered surfaces.

### Deterministic automation and LLM-powered behavior are different categories

SleepTracker should treat three behavior categories as distinct:

- First-party product flows are the core user-controlled experiences for viewing and editing records.
- Deterministic automation includes explicit product logic such as baseline-aware warnings, defaults, ranking, and guarded proposal generation driven by known rules and thresholds.
- LLM-powered integrations or surfaces are optional interpretation layers that can summarize, explain, or draft based on approved reads, but they introduce a different trust and provenance problem than deterministic product logic.

These categories should not be collapsed into one generic interaction category. Deterministic product automation can be valuable without implying LLM involvement, and possible LLM surfaces should remain constrained by stricter evidence, provenance, and user-control requirements.

### LLM assistance should remain user-controlled

If SleepTracker adds LLM-powered experiences, they should assist the user rather than take over authorship. The system should prefer explanation, drafting, and recommendation over silent action. LLM help should be reviewable, attributable, and easy for the user to reject.

### Provenance and reversibility come before autonomy

Any move from read-only help toward drafted or applied changes should require clear provenance and easy reversal before broader autonomy is considered. The product should know what evidence informed a suggestion, how that suggestion differs from saved data, and how a user can undo it.

### Recovery help should earn broader usefulness

The most credible foundational value is helping users recover from poor sleep, irregular timing, and schedule disruption using strong personal evidence. Broader usefulness, such as richer planning or expanded context-aware guidance, should follow only when recovery-focused help proves reliable and worth extending.

## North-star aspirations

### A product that understands the user's own sleep patterns better than generic heuristics do

Personalization should increasingly reflect the user's observed baselines, recurring shifts, and meaningful variability instead of relying on one-size-fits-all thresholds.

### A product that explains itself

Users should be able to understand why a recommendation appears, which inputs influenced it, and how confident the system is. SleepTracker should become more interpretable as personalization becomes more sophisticated.

### A product that earns permission before it expands scope

New personalization surfaces should be justified by demonstrated value, trustworthy signals, and a clear reduction in user effort or confusion. Expansion should be earned, not assumed.

### A product that stays conservative about action

SleepTracker should be comfortable offering strong read-time guidance while remaining cautious about writes, side effects, and cross-system actions. More action should require more proof.

### A product that can support LLM-powered help without surrendering control

If LLM-powered integrations become worthwhile, they should operate as constrained layers on top of trusted product data and explicit user review. They should not replace first-party product flows or blur responsibility for saved changes.

## Durable non-goals

- Personalization is not meant to chase broad novelty without evidence that it improves user outcomes.
- Personalization is not meant to treat sparse signals, weak correlations, or anecdotal patterns as primary decision inputs.
- Personalization is not meant to turn existing user CRUD capability into implicit approval for LLM-mediated writes.
- Personalization is not meant to hide recommendation basis, confidence, or authorship behind opaque automation.
- Personalization is not meant to depend on external integrations, calendars, tasks, or ambient data before core internal signals justify that expansion.
- Personalization is not meant to introduce LLM-powered surfaces merely because summarization is technically possible.
- Personalization is not meant to optimize for autonomy before provenance, confirmation, and reversibility are credible.
- Personalization is not meant to become a substitute for first-party product clarity inside the main SleepTracker flows.

## Relationship to companion docs

This document is the philosophy layer in the personalization doc set.

- [personalization-roadmap.md](./personalization-roadmap.md) translates these beliefs into phased direction, dependency gates, and expansion sequencing.
- [llm-integration-readiness-contract.md](./llm-integration-readiness-contract.md) defines the boundary for LLM-powered integrations and the prerequisites that must exist before any drafted or write-capable behavior is considered safe.
- [feature-reference.md](./feature-reference.md) remains the canonical source for implemented product behavior, including first-party flows and shipped personalization capabilities.
- [personalization-metrics-shortlist.md](./personalization-metrics-shortlist.md) identifies which signal families are strong enough to deserve attention and which ones should remain deferred.
- [personalization-proposal-policy.md](./personalization-proposal-policy.md) shows how evidence-backed metrics can map to guarded deterministic proposals without implying blanket approval for LLM action.

Together, these docs should keep philosophy, shipped truth, proposal logic, and staged LLM boundary questions separate enough that each can evolve without distorting the others.
