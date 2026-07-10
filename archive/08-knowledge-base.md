---
status: superseded
superseded_by: "briefs/00-MASTER-PLAN.md + /RESOLVE-ALPHA-MASTER-PLAN.md"
note: "Old chunk-plan generation, replaced by the I/R/N brief set."
---

# Chunk 08 — Knowledge Base (Alpha scope: minimal stub)

**Depends on:** `01-database-foundation` (`knowledge_base_items` table)
**Priority:** lowest. Not required for the Alpha critical path. Can be built anytime, including in parallel with anything else, or skipped entirely for the first Alpha release if time is short.

## Naming
This tab was previously called "Context." It has been renamed to **Knowledge Base** — a more precise name for what it actually is: ResolvePM's passive project memory layer, not an active work surface.

## Core definition (for reference — full build is Beta scope, not Alpha)
Knowledge Base stores the project's documents, sources, decisions, assumptions, and background so Resolve can retrieve and use them across Roadmap, Work, Dashboard, and Team without asking the user to repeat themselves. It does not own Roadmap, Work, Dashboard, or Team's responsibilities — it supports them.

## Alpha scope — what to actually build now
A minimal stub only:
- The tab exists in navigation
- It can display whatever is already in `project_sessions` (the Intake handoff data) as read-only background/context
- No search/ask interaction required yet
- No decisions/ADR tracking UI required yet
- No sources library UI required yet

## Explicitly NOT in Alpha scope (Beta or later)
- Search/ask as the primary interaction model
- Decisions & ADR tracking
- Assumptions & open questions tracking
- Background notes as a distinct editable structure
- Global bar querying Knowledge Base contextually

## Acceptance criteria
- Tab exists and is reachable from navigation
- Displays the Intake handoff data (project understanding, recommended action, report) in a simple read-only view
- Nothing more elaborate is required for Alpha
