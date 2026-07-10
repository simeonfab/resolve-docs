---
status: superseded
superseded_by: "briefs/00-MASTER-PLAN.md + /RESOLVE-ALPHA-MASTER-PLAN.md"
note: "Old chunk-plan generation, replaced by the I/R/N brief set."
---

# Chunk 07 — Global Bar / Orchestrator Input (Alpha scope)

**Depends on:** `09-navigation-shell` (needs pages to route to). Final wiring depends on `04-dashboard`, `05-work`, `06-team` existing.
**Note:** the scaffold/UI shell for this can be started early in parallel with other chunks, but full routing logic should be wired last.

## Naming / framing
Do not think of this as just "the top bar." The underlying system is an **Orchestrator Input** — the global bar is its default/first visible placement, but the architecture should not bind the input to one visual component, since other placements (contextual, mobile floating input, etc.) are expected later. Build with this separation in mind: an orchestrator engine (intent understanding + routing decisions), input surfaces (where the user types), and destination handlers (where results land) should be reasonably separable, not hard-coded into one component.

## Alpha scope — what to build
Alpha should support:
1. **Ask** — answer a project question
2. **Find** — retrieve an item, source, decision, work item, or context
3. **Navigate** — route the user to the correct page
4. **Capture** — turn a brain-dump into possible structured objects
5. **Confirmed internal actions** — create/update internal ResolvePM objects, but only after user confirmation

**Important — this scope was revised from an earlier read-only decision.** The global bar is NOT read/navigate-only in Alpha. Confirmed-write is required because the Work page's "global text capture" input source depends on it. Confirmation-gating is what keeps this safe — do not skip the confirmation step to save build time.

## Explicitly NOT in Alpha scope
- Sending emails
- Creating external Jira/GitHub issues without confirmation (and no integrations exist yet in Alpha anyway)
- Updating external tools automatically
- Scheduling meetings
- Multi-step automations
- Silently changing project state when confidence is low

## Confirmation rule
If the action changes project state, confirm unless confidence is high and the user explicitly commanded it. Safe to do inline without confirmation: finding a decision, answering why something is current, opening a page, showing a related work item. Needs confirmation: creating work items from a brain-dump, changing a roadmap goal, deleting/archiving something.

## Behaviour loop
1. Understand the user's intent
2. Decide where the thing belongs (Roadmap, Work, Team, Dashboard, Knowledge Base)
3. Decide whether it's safe to act now
4. Ask for confirmation when needed
5. Perform the action
6. Place the result in the correct product area
7. Show or navigate the user to the result if useful

Example pattern:
> "This belongs on Roadmap. Can I put it there?"
> "Done — I've added it to Roadmap. Opening it now."

Do not just say "go to Roadmap" if the bar can safely perform the action itself.

## Team boundary — do not make the Team tab pointless
Quick/lightweight questions can be answered directly in the bar (e.g. "what would Tech Lead think about using GitHub Issues first?" gets a brief inline answer with an offer to continue in Team). Deeper, extended expert discussion should route to the Team tab, not happen entirely inside the bar.

## Acceptance criteria
- Bar is present on every screen
- Supports ask/find/navigate/capture/confirmed-create as specified above
- Any action that changes project state requires explicit confirmation before executing, unless the user gave an explicit high-confidence command
- No external actions (email, external tool writes) are performed in Alpha
- Architecture keeps intent-understanding, input surface, and destination logic reasonably separated rather than hard-coded to the top bar component specifically
