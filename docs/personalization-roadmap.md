# Personalization roadmap

## Purpose

This document is a durable product strategy reference for SleepTracker personalization.

Use it to:

- understand the current evidence base behind personalization priorities
- align short-term and longer-horizon roadmap thinking
- evaluate whether future ideas are ready to move into design or implementation work

This document is not an execution plan, release note, or source of shipped-feature truth.

## How to use this document

- Treat this page as roadmap guidance, not as a statement that listed items are already shipped.
- Use [feature-reference.md](./feature-reference.md) for current implemented behavior.
- Use [personalization-metrics-shortlist.md](./personalization-metrics-shortlist.md) and [personalization-agent-action-map.md](./personalization-agent-action-map.md) for the current metric and action logic that informs this roadmap.
- Use [assistant-readiness-contract.md](./assistant-readiness-contract.md) as the companion document for read-first assistant boundaries and future write-readiness gates.

## Current shipped truth lives elsewhere

Current shipped behavior belongs in [feature-reference.md](./feature-reference.md), especially the Personalization section describing the existing rolling-window metrics, recommendation outputs, friction telemetry ingestion endpoint, and friction backlog endpoint.

This roadmap intentionally goes beyond current implementation, but future-facing items here should be read as directional and gated rather than approved commitments.

## Current evidence snapshot

The present roadmap is grounded in repo documentation plus recent planning analysis of the live database.

Durable current-product truth still lives in the linked source docs. Exact operational counts from planning-time snapshots should be treated as temporary planning evidence unless they are promoted into an upstream source document.

### Repo-backed product baseline

- Personalization already exists as an API-backed capability centered on rolling-window metrics, recommendation outputs, friction telemetry ingestion, and ranked backlog proposals.
- The current metric shortlist prioritizes personal duration and timing baselines, social jetlag, schedule variability, quality-aligned factor ranking, and friction cost metrics.
- The current action map emphasizes guarded, evidence-based actions with explicit triggers, confidence thresholds, and rollback conditions.

### Live-data findings informing roadmap priority

- Recent planning analysis reinforced that the dataset is now larger than the earlier documented snapshot and is consistent enough to support stronger baseline calibration work.
- Regularity and social-jetlag signals remain strong enough to justify near-term prioritization.
- Exercise and notes signals remain weak, so they should not drive primary personalization logic yet.
- Friction telemetry remains effectively unobserved in planning analysis, which means the instrumentation path exists but is not yet producing useful decision input.
- Recent logging consistency appears strong enough to support heavier use of recent-window patterns.

## Strategy principles

1. Keep current truth separate from roadmap intent.
2. Let personal evidence outrank generic heuristics when sample quality is sufficient.
3. Prioritize assistance-first experiences before autonomous write behavior.
4. Prefer regularity, timing stability, and schedule-shift guidance where evidence is already strong.
5. Add instrumentation before committing to UX automation where evidence is currently missing.
6. Use guarded rollout gates, clear provenance, and reversible decisions for any assistant-mediated action.
7. Defer domains with weak signal rather than overfitting thin data.

## Short-term roadmap

The short-term roadmap focuses on making current personalization more trustworthy, better instrumented, and more useful within already-shipped product surfaces.

### 1. Threshold recalibration

Goal: replace coarse static assumptions with stronger personal baselines where the current data volume supports it.

Direction:

- recalibrate unusual-duration and timing interpretation against personal baseline windows rather than relying only on fixed global heuristics
- emphasize regularity and social-jetlag interpretation because those are the strongest verified signals right now
- keep exercise-conditioned and notes-conditioned logic secondary until signal quality improves

Why now:

- the personalization shortlist already identifies personal baselines, social jetlag, and variability as the highest-value implemented-first metrics
- current logging consistency is high enough to support recent-window recalibration

### 2. Friction instrumentation

Goal: make the friction telemetry path decision-useful rather than nominally present.

Direction:

- verify that friction telemetry is emitted at the right points in the current user flows
- capture enough context to explain failed, delayed, or repaired interactions
- produce stable, reviewable evidence before using friction backlog ranking to drive product changes

Why now:

- the current roadmap cannot prioritize friction-reduction work confidently when observed friction events remain at 0
- the metrics shortlist and action map both treat friction cost metrics as high-ROI, but only after meaningful capture exists

### 3. Context capture

Goal: improve the usefulness of read-time interpretation by adding lightweight context that explains timing changes without forcing a heavy journaling workflow.

Direction:

- focus first on context that helps interpret schedule shifts, irregularity, and recovery patterns
- keep structured capture small and reviewable
- avoid overbuilding around notes or free text until repeated signal appears in the data

Why now:

- timing and regularity are strong signals, but their interpretation benefits from knowing whether shifts were intentional, exceptional, or recurring
- notes signal remains too weak to justify note-driven personalization as a primary path

### 4. Assistance-first UX in current surfaces

Goal: make existing surfaces more helpful without introducing a separate assistant product prematurely.

Direction:

- improve guidance inside currently shipped surfaces such as dashboard, trends, and sleep-entry flows
- favor read-time insights, recommended defaults, and contextual warnings over autonomous action
- keep messaging directional and non-causal, consistent with the current action-map guardrails

Why now:

- the current codebase already has personalization and trends surfaces that can host better guidance
- the strongest current opportunity is better interpretation, not a new interface layer

### 5. External read contract before assistant writes

Goal: make any future assistant integration read-first and evidence-first before it can draft or write anything.

Direction:

- require a stable external read contract before any external assistant can act on personalization state
- separate read readiness from write readiness
- treat the companion [assistant-readiness-contract.md](./assistant-readiness-contract.md) as the governing boundary for future assistant behavior

Why now:

- the roadmap includes future assistant-oriented concepts, but current repo truth does not justify write-capable behavior
- durable roadmap guidance should make this boundary explicit early so later work does not skip it

## Longer-horizon roadmap

These items are conditional future directions. They are not current commitments, and they should advance only after the dependency gates below are satisfied.

### Drafts and provenance

Future direction:

- assistant-supported changes should start as drafts with visible provenance, evidence, and user review steps
- recommendations should cite the metrics or context that produced them

Why it belongs later:

- this depends on a trustworthy read contract, stronger context capture, and a clear confirmation model before any write path exists

### Conversational structured capture

Future direction:

- allow conversational input to populate structured sleep-related context rather than relying only on forms
- keep captured output normalized into explicit fields, not opaque assistant memory

Why it belongs later:

- structured capture should follow, not replace, a proven minimal context model
- current weak notes signal does not yet justify free-form conversational capture as the primary near-term path

### Assistant interface or MCP

Future direction:

- expose personalization insights and read-safe operations through a dedicated assistant interface or MCP-style contract
- treat this as an interface decision layered on top of durable read semantics, not as the first step

Why it belongs later:

- interface expansion without a stable read boundary would create capability ambiguity faster than product value

### Calendar awareness

Future direction:

- use calendar-aware context to distinguish deliberate schedule shifts from unplanned irregularity
- improve interpretation of social jetlag, consistency, and recovery needs using known schedule anchors

Why it belongs later:

- external context should only be added after the core sleep-data and lightweight internal-context path is trustworthy

### Daytime outcome labels

Future direction:

- add simple daytime outcome labels that help connect nighttime patterns with next-day function
- use them to improve prioritization beyond duration and timing alone

Why it belongs later:

- this introduces a new data-collection burden and should wait until current sleep-focused guidance is demonstrably useful

### Recovery coach evolving toward energy planner

Future direction:

- start from recovery-oriented interpretation and eventually support broader energy-planning guidance across sleep timing, consistency, and upcoming demands

Why it belongs later:

- this requires stronger context, better outcome labels, and tighter assistant-readiness controls than the product has today

## Dependency gates

Short-term roadmap items can move at different speeds, but the overall sequence should respect these gates.

### Gate 1. Evidence sufficiency

- personal baselines and recent-window patterns remain stable enough to support recalibration
- strong regularity and social-jetlag signals continue to outweigh weaker exercise and notes signals

### Gate 2. Friction observability

- friction telemetry captures enough real events to support ranking and prioritization
- backlog proposals can be justified with observed evidence rather than inferred pain

### Gate 3. Minimal context model

- the product has a lightweight, durable context layer that improves interpretation without creating heavy input burden
- context fields are explicit enough to support provenance and later assistant use

### Gate 4. Assistance-first product fit

- read-time recommendations inside current surfaces produce clear value before a separate assistant surface is pursued
- recommendation wording and ranking remain guarded, evidence-based, and reversible

### Gate 5. External read contract

- external consumers can read the relevant personalization state through a stable contract
- the read contract is documented and accepted before any draft or write path is considered

### Gate 6. Draft and confirmation model

- future assistant-mediated changes can be represented as drafts with provenance, user review, and rollback semantics
- no write-capable behavior should proceed without this gate

## Defer and non-goals

The following are explicitly deferred or out of scope for this roadmap phase.

- Do not treat speculative assistant features as currently shipped behavior.
- Do not prioritize exercise-conditioned or notes-conditioned personalization as a primary roadmap driver while those signals remain weak.
- Do not introduce autonomous writes, silent preference mutation, or opaque assistant memory.
- Do not create a separate assistant interface before read-time guidance inside existing surfaces has proven value.
- Do not expand into broad wellness or cohort analytics under the personalization banner.
- Do not replace current source-of-truth docs with roadmap language.

## Companion documents

- Current shipped capabilities: [feature-reference.md](./feature-reference.md)
- Current metric priorities and go or no-go thresholds: [personalization-metrics-shortlist.md](./personalization-metrics-shortlist.md)
- Current guarded action logic and proposal policy: [personalization-agent-action-map.md](./personalization-agent-action-map.md)
- Assistant boundary companion: [assistant-readiness-contract.md](./assistant-readiness-contract.md)
