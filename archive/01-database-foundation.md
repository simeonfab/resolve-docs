---
status: superseded
superseded_by: "briefs/00-MASTER-PLAN.md + /RESOLVE-ALPHA-MASTER-PLAN.md"
note: "Old chunk-plan generation, replaced by the I/R/N brief set."
---

# Chunk 01 — Database Foundation

**Depends on:** nothing. This is the first chunk.
**Blocks:** everything else.
**Repo:** `simeonfab/cap-pm-cockpit-alaria` (existing ResolvePM backend — Next.js 14 App Router, Supabase, Vercel)

## What already exists (keep as-is, do not modify)
- **Initiatives** — full CRUD, priority, RAG status, dates, Jira key. Maps to Projects.
- **Work Items** — full CRUD, typed (task/action/issue/milestone/risk/decision/dependency), priority, status, due dates, linked to initiatives
- **Roadmap** — query layer grouping initiatives by quarter, linking milestones/risks/dependencies
- **Notes** — linked to initiatives or work items, typed, source-tracked
- **Updates** — status updates, decisions, risks, blockers, AI draft generation already wired
- **Integration accounts** — Jira, GitHub, Todoist, Vercel, Supabase already in schema
- **Sync events** — integration sync tracking
- **AI usage tracking** — token counting, model logging, lifetime limits
- Supabase auth — session-based, server-side, fully working

## What needs adding

### 1. `project_sessions` table
Stores the handoff data passed from Intake at sign-up. Fields needed:
- `id`
- `user_id` (FK to auth user)
- `project_understanding` (Call 1 output, full text/JSON)
- `recommended_action` (Call 2 output)
- `top_priorities` (Call 2 output)
- `what_not_to_do` (Call 2 output)
- `confirmed_tools`
- `project_plan_report` (Call 4 output, full report)
- `gap_answers`
- `output_templates_used`
- `created_at`

### 2. `team_threads` table
Stores Civ-advisor-style AI conversations per role. Fields needed:
- `id`
- `project_id` (FK)
- `role` (enum: `product_director`, `growth_lead`, `tech_lead` — see `06-team.md` for why only these three in Alpha)
- `messages` (JSON array or separate messages table, whichever matches existing patterns in this codebase for similar structures)
- `created_at`, `updated_at`

### 3. `knowledge_base_items` table
Documents and context. Alpha only needs this to exist as a minimal stub (see `08-knowledge-base.md` — full spec is Beta-scope). Minimal fields:
- `id`
- `project_id` (FK)
- `type` (source / decision / assumption / background_note)
- `content`
- `created_at`

### 4. `project_id` column
Add to existing tables (Initiatives, Work Items, Notes, Updates, etc.) to support multiple projects per user. Currently the schema assumes a single project — this needs to change to support one user having more than one project over time.

## Provenance fields on Work Items (needed for chunk 05, add now while touching schema)
Per the locked Work item creation rule: no work item should enter the active list without a clear source. Add these fields to the existing Work Items table if not already present:
- `source_type` — enum: `roadmap`, `integration`, `global_capture`, `voice`, `agent`, `manual`
- `source_ref` — reference to the originating object (roadmap_theme_id, github_issue_id, jira_issue_key, thread_id, voice_note_id, etc.)
- `created_by` — enum: `user`, `orchestrator`, `product_director`, `growth_lead`, `tech_lead`, `integration_sync`
- `confidence` — enum: `high`, `medium`, `low`
- `confirmation_state` — enum: `draft`, `suggested`, `confirmed`, `rejected`
- `roadmap_trace` — which goal/theme this supports
- `external_sync_state` — enum: `not_synced`, `imported`, `exported`, `two_way_sync`, `conflict` (Alpha won't use most of these values yet, but the field should exist)

## Roadmap goals/themes — new table or extension needed
Not explicitly speced as a table yet in the existing schema notes — Codex should check whether the existing "Roadmap" query layer (which groups initiatives by quarter) is sufficient, or whether a new `roadmap_themes` table is needed to represent:
- theme name
- definition of done / target metric
- linked near-term to-do items
- status

**Flag, not invented:** if the existing Roadmap/Initiatives schema doesn't cleanly support the theme-based model (short-term to-do list + theme-based medium/long-term, see `03-roadmap-onboarding-gate.md`), a new table will be needed. Do not force themes into the existing quarter-grouping structure if it doesn't fit — check first.

## Acceptance criteria
- All new tables/columns exist in Supabase and migrations run cleanly
- Existing functionality (Initiatives, Work Items, Notes, Updates, integrations) is untouched and still works
- `project_id` successfully supports a user having more than one project without breaking existing single-project assumptions
