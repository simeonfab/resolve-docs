---
status: current
note: "Parked backlog item - not approved for implementation (self-declared)."
---

# ResolveOS Role Authority Gap — Diagnosis (Parked)

Status: parked backlog item, not approved for implementation
Created: 2026-06-26
Trigger: ResolveYGO Notion planning audit found scope creep at the planning layer; same pattern suspected inside ResolveOS role/governance files

## Why this is parked, not active

Current priority is finding and running external Intake testers, with a concrete short-term goal of £180/month in paid testing/service revenue (covers Claude + ChatGPT top-tier subscriptions, which currently bottleneck engineering throughput). This work does not move toward that goal — it's a framework refinement, not tester acquisition or revenue. Do not start implementation until Intake is validated and/or the £180/month goal is in motion. See `resolve-intake-focus-directive.md` for current active scope.

## The three goals (as originally framed)

1. **Built-in scope-creep resistance as default behaviour** — roles should resist scope expansion by default, not just record it cleanly after the fact.
2. **Real role-level epistemic authority** — roles should be able to evaluate and challenge, not just stay in their lane and escalate for permission:
   - Strategic Product Director should actively gatekeep the roadmap, not just maintain it.
   - Technical Strategy Lead should be able to say "I think this is wrong" unprompted, not just execute or seek delegation approval.
   - QA/evidence roles should intelligently curate and evaluate evidence quality — flag statistical relevance, double-counting across claims, over-weighted single sources — not just store and log feedback.
3. **Default posture flip** — default should be "push back if something looks wrong, unless explicitly told 'don't argue, just do it,'" rather than "defer unless told otherwise."

The mechanism for all three has to be: each role gets a written, concrete standard to challenge against (e.g. Product Director needs a definition of "on-roadmap," QA needs a definition of "statistically relevant evidence," Technical Strategy Lead needs defined sequencing/architecture soundness criteria) — challenge behaviour and criteria must ship together, or the pushback is hollow performance.

## Step 1 — Diagnosis against actual current files (with quotes)

Files reviewed in full: all six role files (`02-roles/product-manager.md`, `business-analyst.md`, `qa-tester.md`, `strategic-product-director.md`, `technical-strategy-lead.md`, `implementation-engineer.md`) and all eight governance files (`06-governance/source-of-truth-rules.md`, `duplication-control.md`, `codex-working-rules.md`, `architecture-decisions.md`, `project-readiness.md`, `decision-maker-reporting.md`, `extraction-migration-guardrails.md`, `update-process.md`).

### Goal 1 — Scope-creep resistance

What exists is reactive, not default-challenging. Product Manager: *"Do not expand scope silently. If useful extra work is noticed, suggest it as follow-up work instead of adding it to the current scope."* This stops the AI quietly adding extra work to a ticket — it does not authorize challenging the admin when the admin proposes the extra scope themselves.

Strategic Product Director has the only real teeth: *"Challenge roadmap assumptions strongly when evidence, strategy, user value, coherence, or trust is weak."* But this role only activates when specifically loaded. When working through Implementation Engineer, Product Manager, or any other role directly, no scope-creep lens is applied at all.

**Conclusion:** resistance exists but is role-gated, not a standing default across all roles.

### Goal 2 — Epistemic authority

- **Strategic Product Director**: already has strong language — *"tell the admin to stop, reorder, or rethink roadmap direction."* Gap: it can only gatekeep against a defined roadmap/strategy, and its own rule says it may *"define or refine strategic product direction when the project context and admin instruction support it"* — i.e. no existing roadmap criteria yet means no gatekeeping standard to check against. Chicken-and-egg problem, as already suspected.
- **Technical Strategy Lead**: can *"challenge implementation feasibility"* and *"stop unsafe implementation direction,"* but every instance is framed as something it *may* do during review. No line authorizes unprompted "I think this is wrong" before being asked.
- **QA Tester**: the clearest gap. Zero language anywhere about evidence quality, statistical relevance, double-counting, or over-weighting a single source. Its evidence model is binary — ran/didn't run, passed/failed, tied to acceptance criteria. The closest related concept (Assumption / Evidence / Confidence / Validation Status: proven / partially validated / unvalidated / disproven) lives in `06-governance/project-readiness.md` as generic governance — it is not assigned to QA as an active responsibility.

### Goal 3 — Default posture flip

Confirmed gap. Every role's challenge language is permission-based ("may challenge," "should challenge when X is weak"), never framed as the default stance. The actual default behaviour throughout the files is escalate-on-ambiguity (flag missing/contradictory information), not evaluate-by-default on everything proposed — including well-formed requests. No override phrase mechanism ("don't argue, just do it") exists anywhere in the current files; this would be a new concept, not a tweak to existing language.

### Bonus — role-set fit assessment

The six roles (Product Manager, Business Analyst, QA Tester, Implementation Engineer, Strategic Product Director, Technical Strategy Lead) are cleanly bounded with an unusually well-thought-through escalation matrix between them — little real overlap found. For the current problem (helping a non-technical person shape a messy project via Intake), no missing role was identified. The actual gap is that Intake doesn't invoke any of these roles yet — a sequencing/workflow issue, not a roster gap.

## Step 2 — Necessity assessment

**Conclusion: not the highest-leverage fix right now.** Reasoning:

1. Doesn't move toward the active £180/month goal — only paying testers/service engagements do that.
2. ResolveOS is at Sprint 001/beta with an existing blocker mechanism (stop, flag, escalate to admin) that already catches most real damage. What's missing here is proactive challenge — a refinement, not a structural hole. Refinements should wait until real usage data (from a paying user) shows which role's silence actually cost something.
3. One piece is smaller and lower-risk than the rest if ever picked up early: the **QA evidence-quality gap**. It's self-contained — extending one role file to reuse the existing Assumption/Evidence/Confidence/Validation-Status model from `project-readiness.md` — rather than a redesign of default behaviour across all six roles. Still not approved for now, but flagged as the cheapest entry point if this is ever revisited before the others.

## Step 3 — Implementation approach (not started, for future reference only)

Not discussed in detail — explicitly deferred until Step 1 and 2 are revisited with real usage evidence. Whoever picks this up next should re-confirm Step 2 against whatever has happened with testers/revenue in the meantime before proceeding to design.
