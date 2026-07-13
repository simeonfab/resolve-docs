# INDEX - resolve-docs

Every document in this repo, one line each. Read this before opening anything else.
Status meanings: **current** = live authority · **superseded** = replaced (see superseded_by) · **historical** = spent/point-in-time, kept for record.
Live project state (what is built, merged, in progress) lives in Notion, not here.

## Root

- `RESOLVE-ALPHA-MASTER-PLAN.md` [current] - The Alpha master plan v1 (9 Jul 2026): phases, parallelism map, standing rules, reference file index. Attach to any fresh working chat.

## briefs/ - Alpha build tickets

- `00-MASTER-PLAN.md` [current] - Frontend build-batch plan: batch definitions and ticket detail for the I/R/N brief set. Sequencing now governed by the root master plan.
- `01-I1-intake-reskin.md` [current] - I1 - Intake reskin build brief.
- `02-I2-intake-transition.md` [current] - I2 - Intake-to-app transition build brief (Batch A).
- `03-R1-signin-otp-reskin.md` [current] - R1 - Sign-in / OTP reskin build brief.
- `04-R2-roadmap-cards-reskin.md` [current] - R2 - Roadmap theme cards reskin build brief.
- `05-R3-todo-list-reskin.md` [current] - R3 - Todo list reskin build brief.
- `06-R4-roadmap-reveal-reskin.md` [current] - R4 - Roadmap reveal reskin build brief.
- `07-N1-app-shell-nav.md` [current] - N1 - App shell and navigation build brief.
- `08-N2-os-reveal.md` [current] - N2 - OS Reveal build brief (Batch D; needs N3).
- `09-N3-dashboard.md` [current] - N3 - Dashboard build brief.
- `10-N4-work-tab.md` [current] - N4 - Work tab build brief (users edit derived dates here).
- `11-N5-team-tab-agent-chat.md` [current] - N5 - Team tab build brief. Full N5 shipped in Alpha (roster + intros AND chat); see TICKET-N5-REVISED.md for the architecture.
- `12-N6-knowledge-base.md` [current] - N6 - Knowledge Base build brief (hosts the refusals / cut record).
- `13-N7-settings.md` [current] - N7 - Settings build brief.
- `14-N8-global-bar.md` [current] - N8 - Global bar build brief. Uses the Contextual mockup variant; the non-Contextual mockup is rejected and archived.
- `BETA-EPIC-SHARPEN-LOOP.md` [beta-epic] - Phase 1 sharpen loop (12 Jul 2026): Gate 2 no longer halts; it sets provisional=true on affected themes and logs to refusals as a diagnostic. Only Gate 1 halts. Deliberately out of Alpha scope.
- `PHASE-1-PLAN.md` [current] - Phase 1 pipeline build plan (grounded, 11 July 2026): every claim grounded in code read, live DB state, or pipeline modules.
- `TICKETS-1.1-1.2.md` [current] - Phase 1 tickets 1.1 (Referee) and 1.2 (Floor): server-side services with no UI, built and tested standalone; wiring happens in 1.4.
- `TICKET-1.4.md` [current] - Ticket 1.4 - Generation replacement + wiring: kills the fake pipeline; replaces string-concat roadmap generation with the validated pipeline.
- `1.3-REVISED-BRIEF.md` [current] - Ticket 1.3 revised - Gate 1 halt screen + provisional marking: revised scope replacing previous 1.3 brief, shrunk UI changes for Claude Design.
- `1.6-BLIND-GRADING-BRIEF.md` [current] - Ticket 1.6 blind grading brief: pre-registered criteria for the two-roadmap A/B blind grade (invention count, themes-are-themes, real DoDs, sequencing, honesty). The Alpha go/no-go grading harness.
- `TICKET-N5-REVISED.md` [current] - N5 revised - Team tab + agent chat architecture decisions (role prompts as system prompts, isolated stand-up router, shared context). Adds the decisions the original N5 brief was blocked on.
- `TICKET-N8-REVISED.md` [current] - N8 revised - Global bar / orchestrator architecture decisions (isolated intent router, confirmation-gated writes, capture → knowledge_base_items). Adds the engine spec the original N8 brief was blocked on.

## briefs/handoffs/ - session handoffs (point-in-time)

- `SESSION-HANDOFF.md` [historical] - Resolve Alpha session handoff (13 Jul 2026). Filing note prepended: §1 incident is CLOSED (no incident, main git-verified) and §2's "string-concat generator is now gone" is FALSE (1.4 never committed). Live state lives in Notion `Resolve Current Context`.

## resolve-pipeline/ - Phase 1 pipeline modules

- `00-README.md` [current] - Pipeline module set overview (v0.1): prompt modules + enforcement spec, model-agnostic.
- `01-elicit.md` [current] - Stage 1 - Elicit: challenge the brain dump, surface outcome + raw material.
- `02-structure.md` [current] - Stage 2 - Structure.
- `03-floor-gate.md` [current] - Stage 3 - Floor gate (Gate 1 outcome, Gate 2 lever). Gate 2 wording corrected in root master plan.
- `04-extract-themes.md` [current] - Stage 4 - Extract themes / classification.
- `05-referee.md` [current] - Stage 5 - Referee (validator clauses, AND rule). Parser fix pending: Ticket 0.1.
- `06-assemble.md` [current] - Stage 6 - Assemble (deterministic, no model call).
- `07-input-safety.md` [current] - Stage 7 - Input safety. Beta-gated.
- `08-integration-map.md` [current] - Integration map: new pipeline onto the live intake system + project_sessions schema. [INFERRED] sections await Ticket 0.2.
- `FLOOR-GATE-DESIGN-FINDING.md` [open-design-finding] - Floor gate discriminator finding (12 Jul 2026): validates the isolated call architecture but flags that the "no natural endpoint" lever criterion is wrong. Blocks nothing in Alpha; must be resolved before the refusal behaviour is sold as a differentiator.
- `GATE-2-QUESTION-PROBLEM.md` [current] - The question we can't get right: problem statement for the floor gate Gate 2 lever discriminator.
- `floor-intakes-with-outcomes.json` [current] - Test data for the floor gate. NOTE: sibling test file synthetic_intakes.json (referenced by the master plan) is NOT yet in this repo.

## mockups/ - current pixel-target mockups (dc set)

- `ResolvePM_Agent_Chat_dc.html` [current] - Agent chat. BETA SCOPE - never attach for N5-lite builds (master plan Phase 2.2).
- `ResolvePM_App_Shell_dc.html` [current] - App shell / nav (N1 pixel target).
- `ResolvePM_Bring_It_In_dc.html` [current] - Bring It In (intake entry).
- `ResolvePM_Confirm_Understanding_dc.html` [current] - Confirm Understanding gate.
- `ResolvePM_Dashboard_dc.html` [current] - Dashboard (N3 pixel target).
- `ResolvePM_Global_Bar_Contextual_dc.html` [current] - Global bar - Contextual variant. THE chosen N8 target.
- `ResolvePM_Implementation_Brief_dc.html` [current] - Implementation brief surface.
- `ResolvePM_Intake_Transition_dc.html` [current] - Intake-to-app transition (I2 pixel target).
- `ResolvePM_Knowledge_Base_Tab_dc.html` [current] - Knowledge Base tab (N6 pixel target).
- `ResolvePM_Next_Move_dc.html` [current] - Next Move surface.
- `ResolvePM_OS_Reveal_dc.html` [current] - OS Reveal (N2 pixel target).
- `ResolvePM_Onboarding_dc.html` [current] - Onboarding flow.
- `ResolvePM_Roadmap_Cards_dc.html` [current] - Roadmap theme cards (R2 pixel target).
- `ResolvePM_Roadmap_Reveal_dc.html` [current] - Roadmap reveal (R4 pixel target).
- `ResolvePM_Roadmap_Tab_dc.html` [current] - Roadmap tab steady-state (N9 pixel target - N9 brief not yet written).
- `ResolvePM_Settings_dc.html` [current] - Settings (N7 pixel target).
- `ResolvePM_Sign-in_dc.html` [current] - Sign-in / OTP (R1 pixel target).
- `ResolvePM_Team_Tab_dc.html` [current] - Team tab (N5-lite pixel target - roster + intros scope only).
- `ResolvePM_Todo_List_dc.html` [current] - Todo list (R3 pixel target).
- `ResolvePM_Work_Tab_dc.html` [current] - Work tab (N4 pixel target).

## strategy/ - strategy & reference docs

- `Alpha-master-planning-brief.md` [current] - Alpha final plan (post-review, v2): scope, product bar, per-surface depth. The locked plan the master plan references.
- `RESOLVEOS-SELF-HOSTING-GOVERNANCE.md` [current] - Self-hosting governance checklist for framework changes.
- `build-phases-and-commercial-model.md` [current] - Build phases (MVP/Alpha/Beta/Monetizable) + commercial model incl. guided sessions layer. Current phase authority.
- `resolve-brand-reference.md` [current] - Brand reference: visual system tokens and rules.
- `resolve-gamification-brief.md` [current] - Emotional weight & gamification brief. Future-phase direction.
- `resolve-output-templates.md` [current] - Output template master definitions (v1) for the intake report.
- `resolveos-competitive-matrix.md` [current] - Competitive matrix (incl. Odysseus / open-source analysis).
- `resolveos-role-authority-diagnosis.md` [current] - ResolveOS role authority gap diagnosis. Parked.
- `roles-epic-handoff-brief.md` [current] - Roles epic + frontend-incorporation handoff brief (pointer to the Notion epic).
- `tester-recruitment-copy-paste.md` [current] - Tester recruitment outreach material (Phase 4 / Phase S).
- `work-mode-activity-system-brief.md` [current] - Work-mode activity system: mode-aware orchestrator prompts (PC / phone / voice / minimal).

## prompts/ - executed agent prompts (historical record)

- `chatgpt-notion-migration-prompt.md` [historical] - Executed Resolve-Planning-to-Notion migration prompt.
- `chatgpt-surface-audit-prompt.md` [historical] - Executed ChatGPT surface-audit prompt (pre-migration inventory).
- `claude-code-esmodule-fix-prompt.md` [historical] - Executed Claude Code ES-module fix prompt.
- `claude-code-push-prompt.md` [historical] - Executed Claude Code push prompt.
- `codex-audit-resolvePM.md` [historical] - Executed Codex audit prompt for ResolvePM.
- `codex-bugfix-4items.md` [historical] - Executed Codex bugfix prompt (4 items).
- `codex-bugfix-7items.md` [historical] - Executed Codex bugfix prompt (7 items).
- `codex-bugfix-esmodule.md` [historical] - Executed Codex ES-module bugfix prompt.
- `codex-bugfix-prompt.md` [historical] - Executed Codex bugfix prompt.
- `codex-build-prompt-v2.md` [historical] - Executed Codex build prompt v2.
- `codex-build-prompt-v3.md` [historical] - Executed Codex build prompt v3.
- `codex-build-prompt-v4.md` [historical] - Executed Codex build prompt v4.
- `codex-build-prompt-v5.md` [historical] - Executed Codex build prompt v5.
- `codex-build-prompt-v6-report-architecture.md` [historical] - Executed Codex build prompt v6 (report architecture).
- `codex-build-prompt.md` [historical] - Executed Codex build prompt v1.
- `codex-output-templates-prompt.md` [historical] - Executed Codex build prompt for the output templates.
- `codex-push-prompt.md` [historical] - Executed Codex push prompt.

## snapshots/ - point-in-time code/doc copies (canonical in product repos)

- `CLAUDE-cap-pm-cockpit-alaria.snapshot.md` [historical] - Snapshot of the ResolvePM repo's CLAUDE.md (renamed to prevent agent auto-load).
- `DEPLOY.md` [historical] - Deployment / guardrails doc for the live Intake app (snapshot).
- `doc-page.js` [historical] - Point-in-time code copy (origin: project knowledge; canonical home in the product repo).
- `index.html` [current] - Point-in-time copy of the Intake frontend. Canonical: ResolveOS-intake-frontend.
- `package.json` [current] - Point-in-time copy of the Intake package.json. Canonical: ResolveOS-intake-frontend.
- `resolve.js` [current] - Point-in-time copy of the /api/resolve.js serverless function. Canonical: ResolveOS-intake-frontend.
- `support.js` [historical] - Point-in-time code copy (origin: project knowledge; canonical home in the product repo).

## archive/ - superseded and historical documents

- `00-BUILD-PLAN.md` [superseded] -> briefs/00-MASTER-PLAN.md + /RESOLVE-ALPHA-MASTER-PLAN.md - Old top-level chunk build plan (chunks 01-09 below).
- `01-database-foundation.md` [superseded] -> briefs/00-MASTER-PLAN.md + /RESOLVE-ALPHA-MASTER-PLAN.md - Old chunk 01 - database foundation.
- `02-I2-intake-transition-draft-v0.md` [superseded] -> briefs/02-I2-intake-transition.md - Earlier draft of the I2 brief.
- `02-signup-handoff.md` [superseded] -> briefs/00-MASTER-PLAN.md + /RESOLVE-ALPHA-MASTER-PLAN.md - Old chunk 02 - signup handoff.
- `03-R1-signin-otp-reskin-draft-v0.md` [superseded] -> briefs/03-R1-signin-otp-reskin.md - Earlier draft of the R1 brief.
- `03-roadmap-onboarding-gate.md` [superseded] -> briefs/00-MASTER-PLAN.md + /RESOLVE-ALPHA-MASTER-PLAN.md - Old chunk 03 - roadmap onboarding gate.
- `03b-roadmap-onboarding-visual-spec.md` [superseded] -> mockups/ (ResolvePM_*_dc set) + strategy/resolve-brand-reference.md - Visual spec patch for old chunk 03; records light-Surface-first decision.
- `04-dashboard.md` [superseded] -> briefs/00-MASTER-PLAN.md + /RESOLVE-ALPHA-MASTER-PLAN.md - Old chunk 04 - dashboard.
- `05-work.md` [superseded] -> briefs/00-MASTER-PLAN.md + /RESOLVE-ALPHA-MASTER-PLAN.md - Old chunk 05 - work.
- `06-team.md` [superseded] -> briefs/00-MASTER-PLAN.md + /RESOLVE-ALPHA-MASTER-PLAN.md - Old chunk 06 - team.
- `07-global-bar.md` [superseded] -> briefs/00-MASTER-PLAN.md + /RESOLVE-ALPHA-MASTER-PLAN.md - Old chunk 07 - global bar.
- `08-knowledge-base.md` [superseded] -> briefs/00-MASTER-PLAN.md + /RESOLVE-ALPHA-MASTER-PLAN.md - Old chunk 08 - knowledge base.
- `09-navigation-shell.md` [superseded] -> briefs/00-MASTER-PLAN.md + /RESOLVE-ALPHA-MASTER-PLAN.md - Old chunk 09 - navigation shell.
- `N3-dashboard-plan-brief.md` [historical] - N3 Dashboard planning brief - N3 since built; kept for decision rationale.
- `ResolvePM_Global_Bar_dc.html` [rejected] - Global bar - non-Contextual variant. REJECTED exploration (master plan Phase 2.3); N8 uses the Contextual variant.
- `resolve-chatgpt-handoff-brief.md` [historical] - ChatGPT orchestrator session handoff (30 Jun).
- `resolve-claude-design-brief.md` [historical] - Front page & visual system redesign brief (executed).
- `resolve-convergence-evaluation.md` [historical] - Convergence evaluation of the old Resolve-Planning repo.
- `resolve-intake-focus-directive.md` [superseded] -> strategy/build-phases-and-commercial-model.md - Intake-only focus directive (pre-Notion era).
- `resolve-intake-mockup-5.html` [current] - Old intake mockup (pre-reskin; suffix kept - ambiguous whether version or download counter).
- `resolve-report-redesign-handoff.md` [historical] - Report redesign mid-process handoff (thread continued into pipeline work).
- `resolve-roadmap-clarity.md` [superseded] -> strategy/build-phases-and-commercial-model.md - Long-term direction doc paired with the intake-focus directive (pre-Notion era).
- `roadmap-onboarding-conversational-mockup.html` [current] - Old conversational roadmap onboarding mockup.
- `roadmap-onboarding-mockup-v2.html` [current] - Old roadmap onboarding mockup v2.
- `roadmap-onboarding-mockup-v3.html` [current] - Old roadmap onboarding mockup v3.
- `roadmap-onboarding-mockup.html` [current] - Old roadmap onboarding mockup (v1).
- `roadmap-visualization-animated.html` [current] - Old animated roadmap visualization mockup.
- `roadmap-visualization-mockup-v2.html` [current] - Old roadmap visualization mockup v2.
- `roadmap-visualization-mockup-v3.html` [current] - Old roadmap visualization mockup v3.
- `roadmap-visualization-mockup-v4.html` [current] - Old roadmap visualization mockup v4.
- `roadmap-visualization-mockup-v5.html` [current] - Old roadmap visualization mockup v5.
- `roadmap-visualization-mockup.html` [current] - Old roadmap visualization mockup (v1).
- `theme-confirmation-animated.html` [current] - Old animated theme confirmation mockup.
- `todo-list-animated-mockup.html` [current] - Old animated todo list mockup.
- `todo-list-presentation-mockup.html` [current] - Old todo list presentation mockup.

## Known gaps

- `synthetic_intakes.json` - referenced by the master plan as Phase 1 test data; not yet in this repo.
- N9 (Roadmap tab) brief - not yet written (master plan section 5 area); mockup already in mockups/.
- Async Manual Discovery Session Protocol v3 - lives in Notion, intentionally not mirrored here.
