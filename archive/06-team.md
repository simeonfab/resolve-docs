---
status: superseded
superseded_by: "briefs/00-MASTER-PLAN.md + /RESOLVE-ALPHA-MASTER-PLAN.md"
note: "Old chunk-plan generation, replaced by the I/R/N brief set."
---

# Chunk 06 — Team (Alpha scope)

**Depends on:** `01-database-foundation` only (needs `team_threads` table). Does NOT depend on Roadmap/Dashboard/Work.
**Can run in parallel with:** `03`, `04`, `05`, `09` — this chunk has no data dependency on any of them.

## Two functions this tab covers
1. **Role setup/onboarding** — first-visit screen suggesting roles, each with a short brief on how it helps this specific project. User selects/confirms their team.
2. **Stand-up room** — ongoing group chat surface to talk to multiple/all agents at once. Alpha scope: this can be a simplified single-thread version rather than a fully separate multi-agent UI — do not over-build this for Alpha.

## Core roles for Alpha — exactly these three, do not add or substitute others
- **Product Director** — strategy, product-market fit, user journeys, prioritisation. Functional framing: closes the *direction* gap (where the company is going).
- **Growth Lead** — acquisition, positioning, pricing, traction. Functional framing: closes the *communication* gap (how to reach people). Deliberately not named "Sales Lead" (reads as a salesperson-for-hire) or "Commercial Lead" (reads as finance).
- **Tech Lead** — technical feasibility, architecture, "can this be built, how." Functional framing: closes the *expertise* gap (whatever specialist knowledge the founder personally lacks).

**Do not add a Compliance/HR role.** This was explicitly considered and rejected — out of scope for the target solo-founder user, and there's a standing product-wide principle that anything compliance/legal-adjacent should redirect the user to consult an actual lawyer, never advise directly. This principle should be respected in how any role responds if a user asks something compliance/legal-adjacent, regardless of which of the three roles they're talking to.

## Self-introduction mechanic — build this exactly
During Team tab onboarding, each recommended role introduces itself in plain English, contextualised to the specific project. Example pattern:
> "I'm Product — I help with strategy, product-market fit, user journeys. Here's what you could ask me about this project."

This is important specifically for Growth Lead, since "Growth Lead" is less immediately self-explanatory as a title than the other two — the self-introduction is what resolves that ambiguity, not a name change.

## Relationship to the six ResolveOS role files
The existing six ResolveOS role files (Business Analyst, Strategic Product Director, Technical Strategy Lead, Product Manager, QA Tester, Implementation Engineer) are NOT what gets exposed to the ResolvePM user in Alpha. The three roles above (Product Director, Growth Lead, Tech Lead) were decided from first principles ("what are the first three hires a solo founder needs"), not by directly relabeling the six existing files.
- Implementation Engineer is explicitly NOT one of the founder-facing advisor roles — it's a capability/tool the system operates (e.g. Codex), not a persona the founder consults. Do not expose it as a chattable role in this tab.
- There is a known content gap: none of the six existing ResolveOS role files actually cover commercial/growth. If Growth Lead's underlying prompt needs to draw on ResolveOS role content, flag this rather than forcing a mismatch — new role content may be needed, this is not resolved yet.

## Contextual agent pinning (separate mechanism, not built in this chunk but worth knowing)
Individual role agents can also be pinned to relevant screens elsewhere (e.g. Product Director pinned to Roadmap). That pinning is part of the respective page's own chunk (see `03-roadmap-onboarding-gate.md`), not this one — this chunk only builds the Team tab itself and the underlying role definitions/threads.

## Acceptance criteria
- Team tab shows exactly three roles: Product Director, Growth Lead, Tech Lead — no Compliance/HR, no direct exposure of the six ResolveOS files as chattable personas
- Each role introduces itself in plain English, contextualised to the user's actual project data (from `project_sessions`), on first visit
- A basic stand-up/group chat surface exists, even if simplified for Alpha
- `team_threads` correctly stores conversations per role per project
