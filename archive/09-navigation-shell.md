---
status: superseded
superseded_by: "briefs/00-MASTER-PLAN.md + /RESOLVE-ALPHA-MASTER-PLAN.md"
note: "Old chunk-plan generation, replaced by the I/R/N brief set."
---

# Chunk 09 — Navigation Shell

**Depends on:** `01-database-foundation` only (needs auth to exist to gate routes). Pure UI scaffolding otherwise — no data dependency on Roadmap/Dashboard/Work content.
**Can run in parallel with:** `06-team`, `08-knowledge-base`, and can start as early as `01` is done.

## Core principle
Navigation should be logical, contextual, and intentional — help the user move when movement adds value, not expose commands or make the user think about app structure. Do not build a pattern where the user has to type navigation commands ("go to Roadmap," "/dashboard," etc.) — the UI's primary navigation plus the global bar's intent-understanding should make that unnecessary.

## Two navigation layers to build

### 1. Primary navigation
A stable, simple, visible tab structure:
- Dashboard
- Roadmap
- Work
- Team
- Knowledge Base

Keep this simple — it is just the basic map of the product, not where the "intelligence" lives.

### 2. Contextual navigation (build the mechanism, even if only a few examples exist in Alpha)
Appears only when movement adds value, triggered by object relationships, not generic prompts. Example pattern: a Work item shows "Supports Roadmap theme: [X]" with an optional link to view that theme on Roadmap — the link only appears because there's a real relationship, not as a standing feature on every object.

## Route gating — important, ties to onboarding
- Dashboard, Work, Team, Knowledge Base must all be inaccessible until the Roadmap onboarding gate (`03-roadmap-onboarding-gate.md`) has been completed and confirmed.
- Roadmap is the only accessible tab immediately after sign-up.

## Nav order
**First-experience order (new user, before Roadmap gate is confirmed):**
1. Roadmap (the only one actually accessible — others should be visibly present but locked/disabled until the gate is confirmed)

**Steady-state order (after Roadmap gate confirmed, ongoing use):**
1. Dashboard
2. Work
3. Roadmap
4. Team
5. Knowledge Base

Reasoning: once the project is running, Work becomes more important than Roadmap for daily execution, so it moves up in steady-state ordering even though Roadmap led during onboarding.

## What to avoid
Do not build a command-based navigation pattern (typed commands to move between pages). Do not over-invest in contextual navigation for Alpha — a small number of clearly-justified links (e.g. Work item → Roadmap theme) is enough; this is not meant to be exhaustive in Alpha.

## Acceptance criteria
- Five-tab primary navigation exists: Dashboard, Roadmap, Work, Team, Knowledge Base
- All tabs except Roadmap are locked/inaccessible until the Roadmap onboarding gate is confirmed
- After confirmation, steady-state nav order is Dashboard, Work, Roadmap, Team, Knowledge Base
- At least one working example of contextual navigation exists (e.g. Work item linking to its Roadmap theme)
