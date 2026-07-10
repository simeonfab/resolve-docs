---
status: superseded
superseded_by: "briefs/00-MASTER-PLAN.md + /RESOLVE-ALPHA-MASTER-PLAN.md"
note: "Old chunk-plan generation, replaced by the I/R/N brief set."
---

# Chunk 03 — Roadmap Onboarding Gate

**Depends on:** `02-signup-handoff` (needs `project_sessions` populated)
**Blocks:** `04-dashboard`, `05-work` (both read from the confirmed Roadmap/to-do list this chunk produces)

## What this chunk does
This is the first screen the user sees after sign-up. It must be completed before Dashboard (or any other tab) becomes accessible. Its job is to confirm the user's goals and produce a near-term to-do list, using data already captured during Intake wherever possible — never re-asking what's already known.

## Roadmap's dual role (context, not new work in this chunk)
Roadmap is both (a) a presentable/exportable surface for stakeholders, and (b) the source of truth Work/Dashboard derive from. This chunk builds (b) — the onboarding/generation flow. The presentable/export view is not required for this chunk's acceptance criteria but the underlying data model should not preclude it later.

## Roadmap structure — what gets built
Two layers:
1. **Short-term to-do list** — next ~10 activities, actionable, undated (duration is unpredictable for solo founders, so deliberately no fixed timeline attached to these items).
2. **Theme-based medium/long-term layer** — NOT department/workstream-based (frontend/marketing/sales was explicitly rejected as the default categorization). A theme is a goal (e.g. "improve customer retention"), not a feature list or team lane. Each theme carries a measurable definition of done (e.g. a target percentage), not just a description.

Screen structure: the user should be able to **toggle between** the to-do list and the theme layer — not see both at once on one screen.

## Generation flow — build exactly this sequence

1. **Pull existing data.** Read `project_sessions` for this user — specifically the Call 2 recommended action / top priorities.

2. **Confirm the top goal directly — do not blank-re-ask it.** Since this data already exists from Intake, present it as a direct, stated confirmation:
   > "We think your goal is [X], based on what you said."
   This is neither a blank question (pretending not to know) nor silent reuse (hiding that it's carried forward) — it is an explicit confirmation statement. Do not phrase this as "what is your goal?" if the data already exists.

3. **Suggest additional goals — do not invent, do not blank-elicit either.** The Product Director role (see `06-team.md`) suggests candidate additional goals grounded in what's already known from the Intake session data. The user confirms or adjusts these. Do not silently generate goals with no basis in the actual project data, and do not present a completely blank "what are your other goals?" form as the only option — lead with grounded suggestions.
   - **Flag, not decided:** the exact number of additional goals to suggest is not specified. Use a reasonable small default (e.g. 2–3) — this is a minor tunable, not a blocker, but should be easy to change later.

4. **Definition of done per goal.** For each confirmed goal/theme, ask the user what "done" looks like — capture a measurable target (e.g. a percentage, a count, a binary milestone).

5. **Generate the near-term to-do list.** Produce the first ~10 actions that plausibly move the confirmed themes forward. These items become Work items with `source_type = roadmap` and `roadmap_trace` pointing to the relevant theme (see `01-database-foundation.md` provenance fields).

6. **User review before confirming.** The user must be able to review and directly edit the resulting themes and to-do list as **real, editable UI elements** — drag-and-drop, type-in-place. Do NOT render this as a static AI-generated image or an uneditable summary; it must be genuinely manipulable by the user, not just describable by an agent.

7. **Confirmation unlocks Dashboard.** Once the user confirms, the Roadmap/to-do list state is persisted, and Dashboard (and the rest of the app) becomes accessible.

## Editability — ongoing, not just onboarding
After onboarding, the Roadmap continues to be editable two ways:
- **Agent-led** — the Product Director role is the primary way the roadmap gets manipulated in normal use (e.g. via its pinned contextual presence on the Roadmap screen).
- **Direct manipulation** — the user can also directly edit the real underlying elements without going through the agent. Both paths must work; this is not an either/or.

## What this chunk explicitly does NOT include (Alpha scope)
- No "cold start" flow (no prior Intake data at all) — flagged as needing its own decision later, not handled here.
- No workstream/department split — theme-based only.
- No fixed dates on the to-do list items.
- No stakeholder-export/presentation view required for this chunk (that's part of Roadmap's other role, not onboarding).

## Acceptance criteria
- New user lands on this screen immediately after `02-signup-handoff` completes
- Top goal is presented as a direct confirmation statement using real data from `project_sessions`, never as a blank question
- At least one additional goal is suggested, grounded in the Intake session data, not invented from nothing
- User can set a definition-of-done value for each confirmed goal
- A near-term to-do list of roughly 10 items is generated and traceable to the confirmed themes
- User can directly edit (not just view) the themes and to-do list before confirming
- Confirming this screen persists the state and unlocks Dashboard access
