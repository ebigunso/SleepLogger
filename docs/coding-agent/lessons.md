# Lessons Log (Append-Only)

This file captures durable lessons discovered during work in this repository.
It is optimized for:
- quick triage
- searchability
- promotion into rules / docs / global skills later

---

## Rules for writing lessons (mandatory)

1) Atomic entries:
- Write **one lesson per distinct failure category**.
- If an entry has more than one independent root cause, split it.

2) Promotion target:
- Each entry must name exactly one primary promotion target:
  - repo rules (common/worker/orchestrator),
  - repo reference docs (how-to-run/validation/ui-e2e/improvement-loop),
  - troubleshooting entry,
  - or a future global skill.

3) Capture deviations (not just user corrections):
Write a lesson entry whenever there is a deviation, including:
- user course correction
- re-plan / plan delta due to new insight
- subagent blocked/failed
- reviewer needs revision/failed
- required validation skip/waiver or unexpected validation failure
- environment/tooling recovery that should become repeatable knowledge

4) Subagent-inclusive:
- Subagents should surface “Lesson Candidates” when they encounter deviations.
- Orchestrator is the single writer of this file and should persist the relevant candidates.

---

## Deviation signal patterns (examples)

These phrases often imply “this should change going forward”:
- “This should be accounted for going forward”
- “Prefer Y as a default”
- “Don’t do X next time”
- “You missed validation / evidence”
- “This is too cautious / too risky”
- “You should have … first”

When in doubt, capture a lesson. Atomic entries keep cost low.

---

## Lesson template (copy/paste)

## YYYY-MM-DD — <short title>  [tags: <comma-separated>]

Context:
- Plan:
- Task/Wave:
- Roles involved:

Deviation:
- <what went wrong or what required course correction, 1–3 bullets>

Root cause:
- <why it happened, 1–3 bullets>

Fix applied:
- <what was done to resolve it>

Prevention:
- Primary promotion target: <one of: rules/* | references/* | troubleshooting/* | global-skill>
- Candidate prevention rule (optional):
  - audience: common | worker | orchestrator
  - proposed rule: <one sentence>
- Optional guardrail:
  - <micro-checklist or dispatch/plan guardrail>

Evidence:
- <what confirmed this is a real recurring pattern>

---

## Entries

### Promotion Traceability

The following lessons were promoted into durable docs/skills and removed from active lesson entries:

- 2026-02-22 — Complete plan lifecycle before task close → `%APPDATA%/Code/User/prompts/Orchestrator.agent.md` (plan lifecycle closeout gate)
- 2026-02-22 — Reviewer gate must include required UI evidence artifacts → `%USERPROFILE%/.agents/skills/playwright-e2e-evidence/SKILL.md`, `docs/coding-agent/references/ui-e2e.md`
- 2026-02-22 — Normalize cwd in persistent terminal validation runs → `%USERPROFILE%/.agents/skills/workspace-troubleshooting/SKILL.md`
- 2026-02-22 — Split mixed-abstraction doc planning and enforce harmonization pass → `%APPDATA%/Code/User/prompts/Orchestrator.agent.md`, `%USERPROFILE%/.agents/skills/plan-format/SKILL.md`
- 2026-02-22 — Enforce task owns/objective alignment in plan quality → `%APPDATA%/Code/User/prompts/Orchestrator.agent.md`, `%USERPROFILE%/.agents/skills/plan-format/SKILL.md`
- 2026-02-22 — Resolve validation precedence conflicts during doc harmonization → `docs/coding-agent/rules/common.md`, `docs/coding-agent/references/validation.md`
- 2026-02-22 — Expand skill plans with explicit language/tech depth on request → `%USERPROFILE%/.agents/skills/plan-format/SKILL.md`
- 2026-02-24 — Skill triggerability requires frontmatter-first precision → `%USERPROFILE%/.agents/skills/skills-maintenance/SKILL.md`
- 2026-02-25 — Favor high-signal lesson capture over routine iteration logging → `%APPDATA%/Code/User/prompts/Orchestrator.agent.md`, `%USERPROFILE%/.agents/skills/improvement-loop/SKILL.md`, `docs/coding-agent/references/improvement-loop.md`
- 2026-02-25 — Ambiguity reduction requires taxonomy and evidence-field alignment → `%USERPROFILE%/.agents/skills/skills-maintenance/SKILL.md`
- 2026-02-25 — Treat third-party skills as read-only unless explicitly approved → `%APPDATA%/Code/User/prompts/Orchestrator.agent.md`, `%USERPROFILE%/.agents/skills/skills-maintenance/SKILL.md`
- 2026-02-25 — Keep SKILL.md as routing, move procedural runbooks to references → `%USERPROFILE%/.agents/skills/skills-maintenance/SKILL.md`
- 2026-02-25 — Default commit strategy should enforce logical chunking → `%USERPROFILE%/.agents/skills/git-workflow/SKILL.md`
- 2026-02-25 — Persist new workflow defaults in lessons during the same turn → `%USERPROFILE%/.agents/skills/improvement-loop/SKILL.md`
- 2026-02-25 — Commit work on a branch, not directly on main → `%USERPROFILE%/.agents/skills/git-workflow/SKILL.md`

### Lesson Entries
<!-- Append new lessons below this line. Keep entries atomic. -->

## 2026-03-11 — Do not infer plan approval from follow-up requirements  [tags: workflow, approval-gate, assumptions]

Context:
- Plan: `docs/coding-agent/plans/completed/login-password-visibility-icons-plan.md`
- Task/Wave: Pre-Wave 1 / Task_1 dispatch
- Roles involved: Orchestrator, User, Worker

Deviation:
- I treated a follow-up requirement clarification (theme-correct icon coloring) as implicit approval to execute a non-trivial plan.
- I dispatched Task_1 before the user gave an explicit approval signal.

Root cause:
- I collapsed “scope clarification” and “approval” into the same signal instead of treating approval as a separate gate.
- I did not require an explicit yes/approve-style acknowledgment before moving from planning to execution.

Fix applied:
- Paused further execution after the correction instead of continuing into reviewer work.
- Recorded the deviation and updated the active plan to reflect the pause and the need for explicit approval to continue beyond already-dispatched work.

Prevention:
- Primary promotion target: global-skill
- Candidate prevention rule (optional):
  - audience: orchestrator
  - proposed rule: Treat follow-up requirements, clarifications, and refinements as non-approval unless the user explicitly approves the plan or directly instructs execution.
- Optional guardrail:
  - Before dispatching any non-trivial Worker task after a plan, confirm the latest user message contains an explicit approval or direct execution instruction; otherwise stop and ask.

Evidence:
- User correction on 2026-03-11: "you should have not assumed I gave approval when that is not the obvious intention of the message."

## 2026-03-12 — Verify worktree with diff before claiming branch is clean  [tags: workflow, verification, git]

Context:
- Plan: Follow-up polish on `chore/login-password-toggle-icons`
- Task/Wave: Post-commit / PR creation handoff
- Roles involved: Orchestrator, User

Deviation:
- I reported that the latest visual polish had already been committed and that the branch was clean.
- A subsequent user check revealed the most recent `app.css` change was still uncommitted.

Root cause:
- I relied on one git cleanliness check and did not cross-check the worktree with an explicit file diff before declaring the branch clean.
- I proceeded to PR creation after a stale assumption about commit coverage instead of re-verifying the exact latest requested file change.

Fix applied:
- Verified the pending `sleep-ui/src/app.css` diff directly, then committed and pushed the missing change.
- Recorded the verification miss in the lessons log before closing the correction loop.

Prevention:
- Primary promotion target: global-skill
- Candidate prevention rule (optional):
  - audience: orchestrator
  - proposed rule: Before claiming a branch is clean or opening a PR after follow-up edits, verify both `git status` and a targeted `git diff` for the last-touched files.
- Optional guardrail:
  - When the user asks to commit recent edits, confirm the intended file diff is empty after the commit and push sequence before reporting completion.

Evidence:
- User correction on 2026-03-12: "The latest change is still uncomitted!"
- `git diff -- sleep-ui/src/app.css` showed the pending color change from `var(--color-primary)` to `var(--color-text-muted)`.

## 2026-03-12 — Never commit user-identifying local paths into repo artifacts  [tags: privacy, docs, git, validation]

Context:

Deviation:

Root cause:

Fix applied:

Prevention:
  - audience: orchestrator
  - proposed rule: Before committing plans, lessons, or validation notes, replace machine-specific absolute paths and user-profile references with repo-relative or environment-agnostic forms.
  - Run a targeted search for `C:/Users`, `/c/Users`, `%USERPROFILE%`, `%APPDATA%`, `AppData`, and similar local-path markers before pushing documentation-heavy changes.

Evidence:

## 2026-03-16 — Durable product docs used "assistant" ambiguously for LLM-specific behavior  [tags: docs, communication, terminology, llm]

Context:
- Plan: Follow-up revision to durable personalization roadmap docs
- Task/Wave: Pre-plan refresh / document architecture correction
- Roles involved: Orchestrator, User

Deviation:
- I wrote durable product docs that used "assistant" ambiguously enough to blur whether the docs were talking about LLM-powered integrations or more mechanical forms of automation.

Root cause:
- I used broad shorthand for a capability class that needed to be distinguished from adjacent concepts such as deterministic automation and repo-internal agent workflow.
- I did not freeze a terminology policy before drafting the durable docs.

Fix applied:
- Paused the doc revision flow and re-scoped the work around explicit capability-class terminology.
- Added a plan requirement to standardize terminology for model-driven and automation-adjacent concepts before revising the docs.

Prevention:
- Primary promotion target: global-skill
- Candidate prevention rule (optional):
  - audience: orchestrator
  - proposed rule: Before drafting durable product docs about behavior classes or system roles, define the capability-class terminology explicitly and avoid ambiguous umbrella terms unless they are defined near the top.
- Optional guardrail:
  - For durable product docs, distinguish among the nearby capability classes in scope before drafting content, and make the intended boundary explicit.

Evidence:
- User correction on 2026-03-16 requiring explicit LLM terminology because "assistant" could be interpreted as mechanical rather than LLM-driven.

## 2026-03-20 — Fix misleading durable doc names instead of apologizing for them  [tags: docs, naming, communication, durability]

Context:
- Plan: Follow-up cleanup of durable personalization docs
- Task/Wave: Post-architecture polish / naming correction
- Roles involved: Orchestrator, User

Deviation:
- I left a durable policy document under a misleading filename and compensated with explanatory prose about why the name should not be taken literally.
- That kept the underlying naming mismatch in place instead of remedying it.

Root cause:
- I optimized for minimal disruption and link preservation instead of treating the misleading filename itself as the defect.
- I accepted disclaimer text as sufficient even though the file name continued to advertise the wrong conceptual role.

Fix applied:
- Renamed the document to match its actual role as proposal policy.
- Updated live and historical references to the canonical path and removed the defensive filename disclaimer.

Prevention:
- Primary promotion target: global-skill
- Candidate prevention rule (optional):
  - audience: orchestrator
  - proposed rule: When a durable doc needs a sentence explaining why its filename or title should not be taken literally, treat that as a signal to fix the name or structure instead of preserving the mismatch with disclaimer prose.
- Optional guardrail:
  - For durable docs, compare title, filename, metadata role, and opening paragraph before finalizing; if they disagree, resolve the naming mismatch rather than documenting around it.

Evidence:
- User correction on 2026-03-20: "You shouldn't just dance around potential issues like this, but rather work to remedy them."

## 2026-03-16 — Durable product docs must separate philosophy from milestone and boundary detail  [tags: docs, communication, roadmap, abstraction]

Context:
- Plan: Follow-up revision to durable personalization roadmap docs
- Task/Wave: Pre-plan refresh / document architecture correction
- Roles involved: Orchestrator, User

Deviation:
- I mixed product aspirations, design philosophy, milestone sequencing, and implementation-adjacent boundary detail too closely in the durable roadmap set.

Root cause:
- I optimized for a complete immediate write-up instead of assigning clear roles to philosophy, roadmap, and boundary documents first.
- I allowed large abstraction jumps inside the same documents without a strong document-role architecture.

Fix applied:
- Re-scoped the work around a document-architecture refresh rather than a wording-only revision.
- Added a plan requirement to define separate durable document roles before rewriting the roadmap set.

Prevention:
- Primary promotion target: global-skill
- Candidate prevention rule (optional):
  - audience: orchestrator
  - proposed rule: Before drafting durable product documentation, define which document owns philosophy, which owns milestone sequencing, and which owns capability boundaries so mixed abstraction levels do not collapse into one memo.
- Optional guardrail:
  - If aspirations, milestones, and boundary rules all need to be expressed, decide the document split before drafting the prose.

Evidence:
- User correction on 2026-03-16 requiring the roadmap to convey aspirations and milestones before details, and suggesting a split if abstraction levels are too far apart.

## 2026-03-16 — Durable product docs need explicit freshness semantics for time-relative language  [tags: docs, communication, durability, freshness]

Context:
- Plan: Follow-up revision to durable personalization roadmap docs
- Task/Wave: Pre-plan refresh / document architecture correction
- Roles involved: Orchestrator, User

Deviation:
- I used time-relative language like "today" and "current" in durable product docs without a clear freshness contract or update context.

Root cause:
- I treated time-relative language as acceptable shorthand instead of requiring metadata or dated snapshot framing.
- I did not define how readers should distinguish durable principles from dated state observations.

Fix applied:
- Added a plan requirement to define metadata fields and freshness semantics before revising the docs.
- Shifted the revision approach toward explicit last-updated context and dated snapshot language where needed.

Prevention:
- Primary promotion target: global-skill
- Candidate prevention rule (optional):
  - audience: orchestrator
  - proposed rule: Durable product docs that use time-relative language must carry explicit freshness metadata or dated snapshot framing, and unqualified uses of terms like "today" or "current" should be removed or anchored.
- Optional guardrail:
  - Before finalizing durable docs, run a terminology sweep for time-relative words and confirm each one is either durable by role or explicitly date-anchored.

Evidence:
- User correction on 2026-03-16 requiring a durable way to distinguish outdated documents from up-to-date ones and to anchor uses of "today" in context.

## 2026-03-16 — Keep lesson incidents concrete and generalize the prevention rule  [tags: lessons, communication, scope, durability]

Context:
- Plan: Follow-up revision to durable personalization roadmap docs
- Task/Wave: Lesson refinement after document-architecture correction
- Roles involved: Orchestrator, User

Deviation:
- I generalized the entire lesson instead of keeping the triggering incident concrete and concentrating the generalization work in the prevention section.

Root cause:
- I treated the lesson title and incident record as the main place to maximize reuse, rather than treating the prevention rule as the main place to generalize future guidance.
- I did not separate "what happened" from "what rule should change going forward" clearly enough.

Fix applied:
- Reverted the incident lesson back to the concrete LLM-terminology problem that triggered it.
- Updated the meta-lesson so it makes the lesson-writing standard explicit for future captures.

Prevention:
- Primary promotion target: global-skill
- Candidate prevention rule (optional):
  - audience: orchestrator
  - proposed rule: When capturing a lesson, keep the incident record concrete and specific to what happened, then generalize the prevention rule to the highest-value confirmed scope that still directly addresses the issue; if a broader prevention scope is plausible but unconfirmed, ask instead of assuming.
- Optional guardrail:
  - Before finalizing a lesson, ask three questions: "Is the incident record concrete enough to preserve what happened?" "Can the prevention rule be generalized further without losing the original issue?" and "Would broadening the prevention rule further require assumptions about user intent or adjacent domains?"

Evidence:
- User correction on 2026-03-16 stating that lessons should be generalized as much as possible for future steering value, but not prematurely expanded beyond confirmed intent.
