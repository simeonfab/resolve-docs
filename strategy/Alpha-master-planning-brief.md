---
status: current
note: "The locked 'Alpha v2' scope definition that the root master plan's Phase 2 executes against."
---

# ResolvePM Alpha — Final Plan (post-review, v2)

## 0. Scope and product bar
**Alpha = one coherent, stitched end-to-end journey:** a tester brings a messy project into Resolve (Intake), signs in, confirms a roadmap, and lands in a **working cockpit** that helps them understand what matters, what to do next, and where the project now lives. Frontend, stitched well, real data. Not a platform build.

**The bar (from review, adopted):**
> The tester should feel they have brought a messy project into Resolve and landed in a working cockpit that helps them understand what matters, what to do next, and where the project now lives.

**Alpha centres on:** one user type (messy solo founder) · one coherent journey · full Dashboard per mockup · completed Roadmap (onboarding *and* steady-state tab) · credible Work · useful Knowledge · minimal Settings · shallow Global Bar · simple OS Reveal · Team as roster + suggestions only.

**Testers:** real tester sign-ups remain supported (the 5-tester Alpha goal stands) — Intake already generates each tester's populated project. A **canonical seeded test project** is added as the dev/verification fixture (review adopted), not a constraint to one account (review adjusted).

## 1. Explicit non-goals (do not build — Beta or later)
Real Team chat · multi-agent behaviour/orchestration · billing · metering · external integrations · production notifications · team workspaces/permissions · deep document management · PDF generation pipeline (in-app report view ships instead) · full ResolveOS runtime surface · personalisation framework · broad preference system. Defining Beta itself (production-ready for external users) is a separate task.

## 2. Current state
**Live:** Intake 1–6 (I1, I2, dark sign-in, entry seam, Surface D exit), N1 App Shell/nav, R1–R4 (sign-in, roadmap cards, to-do, roadmap reveal).
**Not built (old scaffolding in prod):** Dashboard, Roadmap tab, Work, Team, Knowledge, Settings, OS Reveal, Global Bar.
**Open tails (non-blocking):** seam pixel residual (deferred); `7dd13ee` branch deletion unconfirmed.

## 3. Persistence & dynamism model (review adopted)
**Persisted / stable (would feel broken if it changed on refresh):**
- Current selected project (persisted or deterministic — single project per user in Alpha, so deterministic is fine).
- Selected win / current focus — **persisted once chosen, refreshed only on meaningful change** (its work item completed/cancelled or explicitly superseded). Implemented via the N3 Option-A migration: `projects.win_work_item_id` + `win_set_at`.
- OS reveal completion — persisted flag.
- Team-tab first-visit (intro stagger) — persisted flag.
**Implementation:** one small **UI-state home** — additive nullable columns on `projects` (`win_work_item_id`, `win_set_at`, `os_reveal_seen_at`, `team_intro_seen_at`). No new table, no preference framework.
**Dynamic / live (computed per load):** Dashboard card counts, blocked items, work status, roadmap progress, week-progress bar, project summary, Knowledge content (from `project_sessions`), navigation routes.

## 4. The build — ordered steps (review order adopted, with N9 added and N5 rescoped)

### Step 1 — Alpha data spine + canonical test project (NEW, build first)
One shared data-access module all screens read through (instead of per-screen queries):
`activeItems` (todo/in_progress) · `blockedItems` · `dueToday` · `upcomingFromRoadmap` (source_type='roadmap', todo) · `itemsForTheme(initiative_id)` · `winSelection` (highest priority among confirmed active, tie-break earliest created_at) · `weekProgress` · `projectSummary` (from `project_sessions`).
Plus the UI-state columns migration (§3), and a **canonical seeded test project** (realistic themes/work items/blocked items/due-dates/session data) as the dev fixture every subsequent ticket verifies against.

### Step 2 — N3 Dashboard (anchors the cockpit)
Four fixed cards, tap-to-expand-in-place, per `ResolvePM Dashboard.dc.html`. Win = persisted per §3, whole card is tap target, opens its linked work item (agent/roadmap routing deferred). Blocked = real (`status='blocked'`; omit "waiting on [person]" — no field). Today = real to-dos due today; meetings omitted (no data source — do not invent). Pointer = static nudge → Roadmap ("N items moved" stubbed — no change history). Empty states pre-defined (win: caught-up state; blocked: hide card; today: one-line empty). Strip old scaffolding entirely. *(Standalone N3 brief has full detail.)*

### Step 3 — N9 Roadmap tab (NEW TICKET — gap found in review)
The steady-state Roadmap tab (`ResolvePM Roadmap Tab.dc.html`) had **no ticket** — R2–R4 are onboarding-only. Build: By-phase (Now/Next/Later stacked, Now emphasised) ↔ By-theme (theme cards with their own Now/Next/Later rows) toggle, from `initiatives` + `work_items` via the spine. **PD floating button = suggestions-only** (per N5-lite) — opens starter suggestions, not a live chat. Strip old scaffolding.

### Step 4 — N4 Work tab
Sections per mockup: Now / Waiting on / Captured inbox (empty shell + specified copy) / Upcoming from roadmap / Synced from tools (empty shell + copy). All real sections read through the spine; pre-populated from roadmap items — never blank. Checkbox → `status='done'` write. Week-progress bar from the spine's `weekProgress` (window decision: §6.2). Strip old scaffolding.

### Step 5 — N6 Knowledge Base
Read-only from `project_sessions`: the four understanding cards (no pencils), recommended action card, and the report row. **Report row opens an in-app report view** rendering `project_plan_report` (adopted middle path); PDF deferred to Beta — subtitle copy adjusted accordingly (flagged deviation). No search/ask, no decision tracking.

### Step 6 — N2 OS Reveal (stitches the transition)
First-run grand reveal (veil → header → tab bar → N3's cards populate in leverage order) vs calm recurring open, gated by the persisted `os_reveal_seen_at` flag (§3). Depends on N3. Pure sequencing/animation per mockup otherwise.

### Step 7 — N7 Settings (minimal)
Avatar menu (Profile · Settings · Sign out). Account/Workspace real reads. Preference toggles = **UI-only, honestly flagged** (no notification backend in Alpha). **Reset reconciliation stays in scope:** unify the Chunk-03 testing reset with the spec'd "reset onboarding flow" into ONE mechanism (two coexisting resets is a real bug risk). Profile edit = stub, flagged.

### Step 8 — N8 Global Bar (shallow)
One shared contextual pill per `ResolvePM Global Bar Contextual.dc.html`: placeholder + 2–3 suggestions change per tab; **navigate/find actions wired; capture, confirmed-create, deep intent-routing deferred to Beta.** Suggestions that would require deep behaviour either navigate or show "coming soon" — never fake. Not shown during the chrome-free onboarding sequence. Decision §6.4 on Team/KB presence. Remove the old "Search workflows · Ctrl K" element if present.

### Step 9 — N5-lite Team tab (roster + suggestions only — rescoped)
Build the Team tab roster per mockup: three roles, project-contextualised intro sentences (from `project_sessions` data, not hardcoded), staggered first-visit animation gated by `team_intro_seen_at`. **Tapping a lead opens suggestions / "coming soon" — no live chat, no stand-up room, no orchestration** (all Beta). `team_threads` stays unused until Beta. This preserves the locked Alpha scope (role setup + self-intro) while adopting the review's no-real-chat call.

## 5. What changed vs the pre-review plan
- **Adopted:** implementation order (spine first, Dashboard anchors, OS Reveal before Settings/Global Bar, Team last); persistence model; dynamic-vs-stable split; non-goals list; canonical test project; PDF-unless-trivial; product bar.
- **Adjusted (push-backs):** one-controlled-account → canonical project as dev fixture only, real testers stay supported (Intake self-populates per user); Team deferred → **N5-lite** roster kept (locked Alpha scope), chat/stand-up cut; PDF → in-app report view now, PDF Beta.
- **Added:** **N9 Roadmap tab** — genuine ticket-set gap surfaced by the review; without it the cockpit's Roadmap tab stays old scaffolding.
- **Removed from Alpha:** real agent chat, stand-up orchestration, roles-into-frontend mechanism (all Beta), Global Bar capture/create, notifications, PDF pipeline, metering.

## 6. Decisions — ALL RESOLVED (7 July 2026, Simeon)
1. **Win persistence:** persisted + refresh-on-meaningful-change = Option A migration (`projects.win_work_item_id` + `win_set_at`). LOCKED.
2. **Week-progress window (N4):** items **completed this calendar week**. LOCKED.
3. **Report-view copy (N6):** change the "PDF" subtitle to match the in-app report view. LOCKED.
4. **Global Bar on tabs (N8):** **omit on Team** (has its own surface), **show on Knowledge Base**. LOCKED.
5. **Reset reconciliation (N7):** the spec's behaviour is canonical — clears the session and replays intake, saved roadmap stays intact. Unified into ONE mechanism; the old Chunk-03 testing reset is removed/absorbed so only one reset exists. LOCKED.
No open decisions remain — the plan is fully executable.

## 7. Verified schema (unchanged appendix)
`projects`(17: id, user_id, roadmap_confirmed_at → + the four new UI-state columns) · `initiatives`(28, themes) · `work_items`(93: status/priority/type/source_type/confirmation_state/roadmap_trace/initiative_id/due_date) · `project_sessions`(17: understanding/recommended_action/top_priorities/project_plan_report) · `team_threads`(0, unused until Beta) · `knowledge_base_items`/`updates`/`notes`/`integration_accounts`/`sync_events`/`ai_usage_events`/`tester_feedback`(0) · `pending_project_handoffs`(26).
**Confirmed absent (named gaps, all handled above):** meetings/calendar; roadmap-change history; PDF serving; notifications; UI-state flags (being added).
