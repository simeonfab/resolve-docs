---
status: superseded
superseded_by: "briefs/00-MASTER-PLAN.md + /RESOLVE-ALPHA-MASTER-PLAN.md"
note: "Old chunk-plan generation, replaced by the I/R/N brief set."
---

# Chunk 05 — Work (Alpha scope)

**Depends on:** `03-roadmap-onboarding-gate` (reads the confirmed to-do list)
**Can run in parallel with:** `04-dashboard`

## Core principle
Work must not become another PM tool the user has to maintain. It is not a manual task manager — it is the structured execution layer that collects work from Roadmap, and (in Alpha) manual entry, turning it into confirmed, traceable work items.

**Working principle:** Work input should feel like forwarding context to a capable operator, not maintaining another task system.

## Alpha scope — what to build
Only these two sources for Alpha:
1. **Roadmap-generated work** — near-term work items generated from the confirmed Roadmap themes/goals (produced by `03-roadmap-onboarding-gate`). The Work page must NOT start empty for a new user — it should already show the generated near-term items.
2. **Manual add/edit fallback** — supported, but explicitly the fallback, not the primary/default model.

## Explicitly NOT in Alpha scope (do not build these now)
- Integration-imported work (GitHub, Jira, etc.) — Beta scope.
- Global capture bar text parsing into work items — Beta scope (depends on the global bar's confirmed-write capability, see `07-global-bar.md`).
- Voice/chat capture — Beta scope.
- Agent-created work items from Team conversations — Beta scope.

## First-experience copy direction
The first time a user sees Work, it should read as:
> "Here's the work Resolve created from your roadmap. Review, edit, approve, or remove."

NOT:
> "Add your first task."

## Work item creation rule — enforce this
No work item should enter the active work list unless it has at least one of:
1. A roadmap trace
2. Explicit user confirmation
3. (Beta+ only) a trusted integration source
4. (Beta+ only) direct agent/action context

This is enforced via the provenance fields already added to the Work Items table in `01-database-foundation.md` (`source_type`, `source_ref`, `created_by`, `confidence`, `confirmation_state`, `roadmap_trace`, `external_sync_state`). For Alpha, `source_type` will only ever be `roadmap` or `manual`.

## Work page structure — v1 candidate sections
1. **Now** — current active work items, small number only.
2. **Waiting / blocked** — external dependencies and items needing replies.
3. **Captured inbox** — Alpha note: this will be empty/unused in Alpha since capture sources are Beta-scope. Build the section but it's expected to be empty for now.
4. **Upcoming from Roadmap** — next items generated from the agreed roadmap but not yet active.
5. **Synced from tools** — Alpha note: empty/unused in Alpha, no integrations yet. Build the section (so the layout doesn't need rework later) but no data will populate it.

## Acceptance criteria
- A new user's Work page is pre-populated with the near-term to-do list items generated during Roadmap onboarding — never starts empty
- Manual add/edit works as a fallback method
- Every work item carries the provenance fields, correctly set (`source_type = roadmap` for generated items, `source_type = manual` for user-added ones)
- No work item can exist without a roadmap trace or explicit user confirmation
- Section layout (Now / Waiting / Captured inbox / Upcoming from Roadmap / Synced from tools) is present even though two sections will be empty in Alpha
