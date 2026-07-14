# INDEX - resolve-docs

Every document in this repo, one line each. Read this before opening anything else.
Status meanings: **current** = live authority · **superseded** = replaced (see superseded_by) · **historical** = spent/point-in-time, kept for record.
Live project state (what is built, merged, in progress) lives in Notion, not here.

## ⭐ START HERE — the Alpha authority set

1. **`briefs/ALPHA-BUILD-TICKETS.md`** — **THE AUTHORITY — start here for any Alpha build work.** Every ticket, every element / action / motion contract per screen. Where a mockup or an older brief disagrees, this pack wins.
2. `mockups/ResolvePM_Implementation_Brief_dc.html` — the consolidated **design spec** the ticket pack was built from (full version).
3. `briefs/RESOLVE-AUDIT-REGISTER.md` — the **evidence base**: every finding from the 13–14 July audits (F1–F33).

**Phase 0 is COMPLETE (14 Jul 2026):** cap-pm `main` = `2de51b2` · resolve-docs `main` = `760eaf4`. ⚠️ The branch SHAs `b7b98ce` and `85812f2` are STALE — that work IS cap-pm `main` now; branch off `main`, not those.

Motion authority lives in the mockups dir: `mockups/Resolve_Flow_Spec.html` (Surfaces A–I, literal values) + `mockups/MOTION-SYSTEM-HANDOFF.md` (the motion handoff overview) + the five `*_Motion_dc.html` workbenches.

## Root

- `RESOLVE-ALPHA-MASTER-PLAN.md` [current] - The Alpha master plan v1 (9 Jul 2026): phases, parallelism map, standing rules, reference file index. Sequencing context; **superseded by `briefs/ALPHA-BUILD-TICKETS.md` for the binding per-screen build contract.**

## briefs/ - Alpha build tickets

- `ALPHA-BUILD-TICKETS.md` [current — **THE AUTHORITY**] - The Alpha build ticket pack: Phase 0 (done) → Phase 1 (backend spine) → Phase 2 (screens) → Phase 3 (audit + retest). Binding element/action/motion contract per screen. Start here.
- `RESOLVE-AUDIT-REGISTER.md` [current — **evidence base**] - Every finding (F1–F33) from the 13–14 July audits: pipeline/generation, data layer, frontend/routes, intake, repo/process. The evidence the ticket pack stands on.
- `00-MASTER-PLAN.md` [superseded → ALPHA-BUILD-TICKETS.md where they differ] - Frontend build-batch plan: batch definitions and ticket detail for the I/R/N brief set.
- `01-I1-intake-reskin.md` [superseded → ALPHA-BUILD-TICKETS.md where they differ] - I1 - Intake reskin build brief.
- `02-I2-intake-transition.md` [superseded → ALPHA-BUILD-TICKETS.md where they differ] - I2 - Intake-to-app transition build brief (Batch A).
- `03-R1-signin-otp-reskin.md` [superseded → ALPHA-BUILD-TICKETS.md where they differ] - R1 - Sign-in / OTP reskin build brief.
- `04-R2-roadmap-cards-reskin.md` [superseded → ALPHA-BUILD-TICKETS.md where they differ] - R2 - Roadmap theme cards reskin build brief.
- `05-R3-todo-list-reskin.md` [superseded → ALPHA-BUILD-TICKETS.md where they differ] - R3 - Todo list reskin build brief.
- `06-R4-roadmap-reveal-reskin.md` [superseded → ALPHA-BUILD-TICKETS.md where they differ] - R4 - Roadmap reveal reskin build brief.
- `07-N1-app-shell-nav.md` [superseded → ALPHA-BUILD-TICKETS.md where they differ] - N1 - App shell and navigation build brief.
- `08-N2-os-reveal.md` [superseded → ALPHA-BUILD-TICKETS.md where they differ] - N2 - OS Reveal build brief (Batch D; needs N3).
- `09-N3-dashboard.md` [superseded → ALPHA-BUILD-TICKETS.md where they differ] - N3 - Dashboard build brief.
- `10-N4-work-tab.md` [superseded → ALPHA-BUILD-TICKETS.md where they differ] - N4 - Work tab build brief (users edit derived dates here).
- `11-N5-team-tab-agent-chat.md` [superseded → ALPHA-BUILD-TICKETS.md where they differ] - N5 - Team tab build brief. Full N5 shipped in Alpha (roster + intros AND chat); see TICKET-N5-REVISED.md for the architecture.
- `12-N6-knowledge-base.md` [superseded → ALPHA-BUILD-TICKETS.md where they differ] - N6 - Knowledge Base build brief (hosts the refusals / cut record).
- `13-N7-settings.md` [superseded → ALPHA-BUILD-TICKETS.md where they differ] - N7 - Settings build brief.
- `14-N8-global-bar.md` [superseded → ALPHA-BUILD-TICKETS.md where they differ] - N8 - Global bar build brief. Uses the Contextual mockup variant; the non-Contextual mockup is the alternative option.
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

- `00-README.md` [current] - Pipeline module set overview (v0.1): prompt modules + enforcement spec, model-agnostic. Gate 1 is the only halt; Gate 2 degrades to provisional + refusals (T0.3).
- `01-elicit.md` [current] - Stage 1 - Elicit: challenge the brain dump, surface outcome + raw material.
- `02-structure.md` [current] - Stage 2 - Structure.
- `03-floor-gate.md` [current] - Stage 3 - Floor gate (Gate 1 outcome, Gate 2 lever). **Rewritten in T0.3: Gate 1 halts; Gate 2 NEVER halts — provisional + refusals. Output split into gate1/gate2.**
- `04-extract-themes.md` [current] - Stage 4 - Extract themes / classification.
- `05-referee.md` [current] - Stage 5 - Referee (validator clauses, AND rule). T0.3: last-theme fallback degrades to provisional, never escalates to a question. Parser fix pending: Ticket 0.1.
- `06-assemble.md` [current] - Stage 6 - Assemble (deterministic, no model call).
- `07-input-safety.md` [current] - Stage 7 - Input safety. Beta-gated.
- `08-integration-map.md` [current] - Integration map: new pipeline onto the live intake system + project_sessions schema. T0.3: Gate 1/Gate 2 consequences clarified.
- `FLOOR-GATE-DESIGN-FINDING.md` [open-design-finding] - Floor gate discriminator finding (12 Jul 2026): validates the isolated call architecture but flags that the "no natural endpoint" lever criterion is wrong. Blocks nothing in Alpha; must be resolved before the refusal behaviour is sold as a differentiator.
- `GATE-2-QUESTION-PROBLEM.md` [current] - The question we can't get right: problem statement for the floor gate Gate 2 lever discriminator.
- `floor-intakes-with-outcomes.json` [current] - Test data for the floor gate. NOTE: sibling test file synthetic_intakes.json (referenced by the master plan) is NOT yet in this repo.

## mockups/ - current pixel-target mockups (dc set) + motion authority

### Screen mockups (pixel targets)
- `ResolvePM_Implementation_Brief_dc.html` [current] - **The consolidated design spec the ticket pack was built from.** (Full version filed 14 Jul; replaced an earlier partial.)
- `ResolvePM_Onboarding_dc.html` [current] - Onboarding flow (§1–2 name + welcome/problem).
- `ResolvePM_Bring_It_In_dc.html` [current] - Bring It In (§3 intake entry).
- `ResolvePM_Confirm_Understanding_dc.html` [current] - Confirm Understanding (§4 gate).
- `ResolvePM_Next_Move_dc.html` [current] - Your Next Move (§5).
- `ResolvePM_Intake_Transition_dc.html` [current] - Intake-to-app transition + bridge (§6 / I2 pixel target). (Full bridge-motion version filed 14 Jul.)
- `ResolvePM_Sign-in_dc.html` [current] - Sign-in / OTP (§7 / R1). **Dark shell with the 6-box code states (filled / active / err).** (Full version filed 14 Jul.)
- `ResolvePM_Floor_Refusal_dc.html` [current] - §7.5 Gate 1 halt screen (the one deliberate stop). **Newly filed.**
- `ResolvePM_Roadmap_Cards_dc.html` [current] - Roadmap theme cards (§8 / R2 pixel target).
- `ResolvePM_Todo_List_dc.html` [current] - Todo list (§9 / R3 pixel target).
- `ResolvePM_Roadmap_Reveal_dc.html` [current] - Roadmap reveal (§10 / R4). Includes the provisional marker + tooltip. (Updated version filed 14 Jul.)
- `ResolvePM_OS_Reveal_dc.html` [current] - OS Reveal (§11 / N2 pixel target).
- `ResolvePM_Dashboard_dc.html` [current] - Dashboard (§12 / N3 pixel target).
- `ResolvePM_Roadmap_Tab_dc.html` [current] - Roadmap tab steady-state (§13). Includes the provisional marker + tooltip on by-theme headers. (Updated version filed 14 Jul.)
- `ResolvePM_Work_Tab_dc.html` [current] - Work tab (§14 / N4 pixel target).
- `ResolvePM_Team_Tab_dc.html` [current] - Team tab (§15). Header reads "Your team" (the "Coming soon" header was dropped per T2.12).
- `ResolvePM_Agent_Chat_dc.html` [current] - Agent chat (§15).
- `ResolvePM_Knowledge_Base_Tab_dc.html` [current] - Knowledge Base tab (§16 / N6 pixel target).
- `ResolvePM_Settings_dc.html` [current] - Settings (§17 / N7 pixel target).
- `ResolvePM_App_Shell_dc.html` [current] - App shell / nav (N1 pixel target).
- `ResolvePM_Global_Bar_Contextual_dc.html` [current] - Global bar - Contextual variant. **THE chosen §T2.15 target.**
- `ResolvePM_Global_Bar_dc.html` [current] - Global bar - non-Contextual variant. The alternative layout option referenced by T2.15 (Contextual is chosen). **Newly filed to mockups/** (an older copy also sits in archive/).
- `ResolvePM_Portfolio_dc.html` [current] - Portfolio view mockup. **Newly filed.**

### Motion authority (Surfaces A–I)
- `Resolve_Flow_Spec.html` [current] - **THE motion spec.** Every animation as literal copy-pasteable values (duration, easing, delay, stagger, keyframes), organized Surfaces A–I. **Newly filed.**
- `MOTION-SYSTEM-HANDOFF.md` [current] - Motion system overview + easing tokens + key interaction rules; index to the Flow Spec and the workbenches. **Newly filed.**
- `ResolvePM_Dashboard_Motion_dc.html` [current] - Dashboard motion workbench (Surface E). **Newly filed.**
- `ResolvePM_Roadmap_Motion_dc.html` [current] - Roadmap motion workbench (Surface F). **Newly filed.**
- `ResolvePM_Work_Motion_dc.html` [current] - Work motion workbench (Surface G). **Newly filed.**
- `ResolvePM_Knowledge_Motion_dc.html` [current] - Knowledge/Library motion workbench (Surface H). **Newly filed.**
- `ResolvePM_Feedback_Motion_dc.html` [current] - Toast + undo motion workbench (Surface I). **Newly filed.**

### mockups/screenshots/ - build/audit verification captures
- `screenshots/*.png` [current] - 38 verification screenshots (roadmap cards/reflow/toggle/expand/matrix, to-do, work check-off/capture/completed, feedback toasts, knowledge grid, OS reveal, reveal, roadmap-tab, handoff, brief). **Newly filed.**

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
- `doc-page.js` [historical] - Renderer for the `.dc.html` doc pages (origin: project knowledge; canonical home in the product repo). Identical to the copy bundled with the mockups.
- `index.html` [current] - Point-in-time copy of the Intake frontend. Canonical: ResolveOS-intake-frontend.
- `package.json` [current] - Point-in-time copy of the Intake package.json. Canonical: ResolveOS-intake-frontend.
- `resolve.js` [current] - Point-in-time copy of the /api/resolve.js serverless function. Canonical: ResolveOS-intake-frontend.
- `support.js` [historical] - Runtime for the `.dc.html` workbench files (origin: project knowledge; canonical home in the product repo). Identical to the copy bundled with the mockups.

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
- `ResolvePM_Global_Bar_dc.html` [superseded] -> mockups/ResolvePM_Global_Bar_dc.html - Older copy of the non-Contextual global-bar exploration; the current copy now lives in mockups/.
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
- N9 (Roadmap tab) brief - the designed page lives on branch `claude/roadmap-tab-step-3-cwrt6p` (cherry-pick 2 page files only; NEVER merge the branch); mockup is `mockups/ResolvePM_Roadmap_Tab_dc.html`.
- Async Manual Discovery Session Protocol v3 - lives in Notion, intentionally not mirrored here.
