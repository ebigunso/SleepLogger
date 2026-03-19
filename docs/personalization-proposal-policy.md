# Personalization proposal policy

- status: draft
- last_updated: 2026-03-20
- doc_role: guarded deterministic proposal policy for personalization actions
- canonical_for: trigger logic, guardrails, confidence thresholds, rollback expectations, and proposal templates tied to personalization metrics
- not_canonical_for: shipped feature inventory, roadmap sequencing, durable philosophy, or blanket approval for LLM-powered actions

This is a policy companion to [personalization-metrics-shortlist.md](./personalization-metrics-shortlist.md).
It maps each high-priority metric to concrete deterministic proposal candidates, including trigger thresholds and guardrails.

This document does not grant generic automation or LLM permission to act. It defines when SleepTracker's deterministic product logic may surface or rank proposals, and it should be read alongside [feature-reference.md](./feature-reference.md) for shipped behavior, [personalization-philosophy.md](./personalization-philosophy.md) for durable product intent, [personalization-roadmap.md](./personalization-roadmap.md) for phased direction, and [llm-integration-readiness-contract.md](./llm-integration-readiness-contract.md) for the separate LLM integration boundary.

## Governing rule

A proposal qualifies as a deterministic personalization proposal only when all of the following are true:

- It starts from a named metric in the matrix below rather than from open-ended product ideation.
- It maps that metric to a concrete proposal candidate that SleepTracker can explain without speculative reasoning.
- It carries explicit trigger, confidence, guardrail, and rollback conditions before it is surfaced or ranked.
- It stays within shipped deterministic product logic and does not imply blanket automation, hidden writes, or LLM authority.

If any part of that chain is missing, the proposal is out of policy.

## Main policy spine

- Treat each matrix row as a guarded proposal rule, not as permission to mutate user data.
- Run rules on a rolling window (for example: 28 days), then compare with the prior window.
- Surface or rank a proposal only when trigger, confidence, and guardrails are all satisfied.
- Keep shipped read and write capabilities anchored to [feature-reference.md](./feature-reference.md); keep any future LLM-mediated behavior constrained by [llm-integration-readiness-contract.md](./llm-integration-readiness-contract.md).

### Evidence and backlog proposal requirements

When generating a feature or change proposal, include:

1. **Observed evidence** (counts, rates, or deltas in current and prior window)
2. **Expected benefit** (estimated time saved per week or reduced rework)
3. **Confidence level** (high, medium, or low based on sample and stability)
4. **Rollback condition** (what metric change invalidates the proposal)

Only auto-promote proposals when confidence is **medium or higher**.

### Confidence, guardrail, and rollback expectations

- Confidence must reflect both sample sufficiency and signal stability across adjacent windows.
- Guardrails must block a proposal when data quality, missing data, or recent behavior changes make the signal unreliable.
- Rollback expectations must be stated up front so a shipped proposal can be withdrawn when the supporting metric deteriorates or the expected benefit does not materialize.
- Directional insights may describe association, but never causation, unless a separate policy explicitly authorizes stronger claims.

## Metric-to-action matrix

| Metric | Primary analysis question | Proposal candidates | Trigger to act | Guardrail before applying |
|---|---|---|---|---|
| Personal duration baseline (p10/p50/p90, IQR) | Is current sleep duration outside personal norm? | 1) Replace static unusual-duration warning with personalized range. 2) Add contextual warning text based on personal tails. | >= 60 sessions in baseline window and out-of-range incidence >= 5% in recent window | Do not apply if baseline window includes major schedule disruption period (travel/shift changes) |
| Timing baseline by day type (weekday/weekend medians) | Are start/end times predictably different by day type? | 1) Prefill form defaults using day-type median bed/wake time. 2) Offer one-click "Use your usual weekday/weekend times." | >= 8 weekday and >= 4 weekend sessions in window, with stable medians across 2 windows | Do not auto-switch defaults if recent 14-day pattern diverges strongly from baseline |
| Social jetlag indicator (weekend-mid minus weekday-mid) | Is schedule phase shifting on weekends? | 1) Show schedule-shift insight card. 2) Suggest consistency-oriented trend view by default. | Absolute midpoint delta >= 30 min for 2 consecutive windows | Suppress if weekend sample is too small (< 4 sessions) |
| Schedule variability score (bed/wake dispersion) | Is irregular timing the main instability source? | 1) Prioritize regularity insight over duration-only insight. 2) Suggest adding quick rounding controls or consistent-time shortcuts in future UX backlog. | Variability >= 60 min and persists across 2 windows | Defer if data gaps are high (missing days > 30% in window) |
| Quality-aligned factor ranking | Which factors most align with higher quality nights? | 1) Rank top 2-3 actionable factors in dashboard insight text. 2) Shift default trend explanation toward quality-linked factors. | >= 40 sessions with quality and >= 3 distinct quality values; factor effect is stable across adjacent windows | Use directional language only ("associated with"), never causal language |
| Friction cost metrics (form time/errors/retries/immediate edits/partial follow-up failures) | Which workflow pain points waste the most time? | 1) Maintain auto-ranked UX backlog by estimated minutes saved/week. 2) Promote top item to implementation proposal when persistent. | >= 30 captured submit flows and at least one friction pattern persists for 2 windows | Require explicit evidence summary before proposing implementation changes |

## Secondary reference material

This section supports implementation and review of the policy above. It is not the governing policy and should not be read as equal in weight to the matrix or the proposal qualification rules.

### Backend endpoint mapping

This page references shipped endpoints that support the current deterministic personalization policy. Endpoint inventory and product truth remain canonical in [feature-reference.md](./feature-reference.md).

Backend endpoints used by this map:

- `GET /api/trends/personalization`
- `POST /api/personalization/friction-telemetry`
- `GET /api/personalization/friction-backlog`

### Suggested proposal templates

#### Template A: Default tuning proposal

- **Problem:** Static defaults diverge from observed personal baseline.
- **Evidence:** Day-type medians stable across two windows.
- **Proposed change:** Update form defaults and warning thresholds to personal baseline.
- **Success metric:** Reduced immediate edit rate and reduced warning dismissals.

#### Template B: Friction reduction proposal

- **Problem:** Repeated flow failures or retries indicate avoidable input friction.
- **Evidence:** Error cluster persists (same error kind or repair pattern).
- **Proposed change:** Add targeted UX affordance (quick-adjust, better boundary handling, retry guidance).
- **Success metric:** Reduced retries and reduced median time-in-form.

#### Template C: Insight prioritization proposal

- **Problem:** Current insight emphasis does not match strongest personal drivers.
- **Evidence:** Quality-aligned factor ranking stable over adjacent windows.
- **Proposed change:** Reorder dashboard or Trends insights to emphasize top factors.
- **Success metric:** Improved consistency in quality-linked outcomes over time.

### Trends page support (secondary)

Purpose-first mapping for the existing Trends page metric toggle.

| Trends metric key | Primary user question | Action intent (what user should do next) | Interpretation cue style |
|---|---|---|---|
| `duration` | "Am I getting enough sleep time recently?" | Shift bedtime or waketime plan to recover or protect total sleep window. | Direction + magnitude (minutes/h:mm delta vs prior period) |
| `quality` | "Is perceived sleep quality moving in the right direction?" | Repeat routines linked to better nights; review low-score clusters. | Direction on 1..5 scale with stability emphasis |
| `bedtime` | "Is my sleep onset timing drifting?" | Tighten bedtime consistency around intended anchor. | Earlier or later shift + variability cue |
| `waketime` | "Is wake timing stable?" | Protect consistent wake anchor and reduce swings. | Earlier or later shift + variability cue |

#### Period comparison and interpretation policy for Trends-linked recommendations

- **Alignment:** use same-length prior period directly before current `from..to` range, aligned by wake date.
- **Minimum data gate:** require at least 3 usable points in both periods for the selected metric before issuing directional interpretation.
- **Missing-data rule:** ignore missing metric values; do not impute zeros or synthetic values.
- **Default behavior:** keep prior-period comparison opt-in or off by default to avoid clutter in default chart state.
- **Tone constraints:** keep recommendation wording non-causal and concise; one primary interpretation cue per selected metric.

#### Compatibility guardrails

- Restrict purpose and comparison logic to currently shipped trend metrics (`duration`, `quality`, `bedtime`, `waketime`).
- Use existing `/api/trends/sleep-bars` fields only; do not assume additional backend payload keys.
- Respect existing controls already on Trends page: presets (`7d/14d/30d`), custom date range, and chart or schedule view toggle.

## Non-goals

- No cohort comparisons or broad product analytics.
- No heavy event instrumentation beyond friction telemetry required for ranking.
- No automatic action when confidence is low or data quality guardrails fail.
- No implication that current first-party CRUD or telemetry endpoints are approved for LLM-mediated writes.
