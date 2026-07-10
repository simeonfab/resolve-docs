---
status: current
note: "Pointer doc, not the spec - the Notion Roles epic (393d8b53b7a8810ea8d1df77834670b6) is the working source of truth."
---

# Resolve — Roles Epic + Frontend-Incorporation: Handoff Brief

*Orientation for a new chat. This brief is a POINTER, not the spec — the Notion page linked below is the working source of truth. Read it in full before doing anything; do not work from this brief alone.*

---

## Source of truth — read this FIRST
**"🎯 Epic: Redefine Core Resolve Roles (Product Director, Growth Lead, Tech Lead, Implementation Engineer)"**
→ https://app.notion.com/p/393d8b53b7a8810ea8d1df77834670b6

This page is authoritative and self-maintaining across chats. Read it fully, and update its task list as you work. This brief only orients you and flags what's outstanding.

Program-level state (overall Alpha build + priorities):
→ Active Priorities: https://app.notion.com/p/390d8b53b7a881dda5e4daedbc76d54c

---

## What the epic is
Define the four core Resolve roles sharply, then rewrite the ResolveOS role files to match — cannibalising good existing content, rewriting where weak, defining each role's skills alongside it. Scoped first for solo / non-technical founders building **software** with Claude/Codex-style tools.

## Status: role definitions are essentially COMPLETE
- All four roles have all 9 questions answered (identity, goal, **scope/boundaries**, methodology, tone, skills, escalation, anti-patterns, uncertainty) — the Q3 cross-role scope pass is **done**.
- Global layer **extracted** (5 global behaviours + 4 shared skills).
- **QA resolved:** folds into **Implementation Engineer** as a distinct mode (shift-left pattern) — **not** Tech Lead.
- Launch/productisation lifecycle mapped (phases, per-phase drivers, deliverables, expanded artifact list).

→ Definitions are complete enough that the **file rewrite can proceed**, sequenced with the remaining tasks below.

## Genuinely outstanding (per the page's own task list)
1. Confirm **Business Analyst** folds into Product Director (QA already resolved).
2. Full **voice/structure consistency** pass across all four (the Q3 scope pass is done; a broader consistency review is not).
3. **Task 8** — revalidate the Orchestrator + the four role files against the global layer so they *reference/inherit* it rather than duplicate it inline.
4. **Task 8b** — design the **"master records"** cross-role awareness system (master role record, master discussion record, maybe a third); resolve whether it extends Knowledge Base / `team_threads` or is a new layer.
5. **Per-role push-back criteria** — concrete written challenge triggers per role, shipped *inside* each role definition (the push-back *methodology* is global and done; the specific per-role triggers are the deliverable still owed).
6. **Compile the launch/productisation** body of work into its standalone global markdown ("launch/productisation principles").
7. **Decide where the files live** in the repo (replace in place vs. new files) — undecided.
8. **The actual file rewrite** (Codex) — the deliverable, run in tandem with the above.

## ⚠️ Confirm before Codex touches any files
The page names the source role files — `strategic-product-director.md`, `technical-strategy-lead.md`, `product-manager.md`, `implementation-engineer.md` — and the entrypoint `00-system/resolveos-entrypoint.md`, **but does NOT state which repo/path they live in**, and "where the files live / replace-in-place vs. new files" is itself open task #7. So **confirm the real repo + current file inventory first — do not guess.** (Plausible: a ResolveOS file set, possibly inside `ResolveOS-intake-frontend` since the Intake app injects these markdown files into its API calls — but verify.)

## Repos
- `ResolveOS-intake-frontend` — Intake app; injects ResolveOS role/governance markdown into its API calls.
- `cap-pm-cockpit-alaria` — ResolvePM app (Next.js 14 / Supabase / Vercel).

---

## SEPARATE FUTURE EPIC — Incorporating ResolveOS roles into the Resolve frontend

This is the connection between **ResolveOS (the backend/framework: roles, skills, governance, the `00-system/resolveos-entrypoint.md` routing/entrypoint layer)** and **Resolve the product frontend (Intake + ResolvePM)** — i.e. how the defined roles actually get **assigned to a user** and **executed in-product**.

**Explicitly NOT part of the roles-definition epic.** That page states: *"Mapping which role/skill attaches to which specific user-journey moment… is a distinct, later epic. Do not conflate the two."* This frontend-incorporation work is that separate epic (or closely adjacent — decide the boundary vs. the journey-mapping framing).

**FIRST TASK for whoever picks this up: create its own Notion source-of-truth page.** It has none yet. Do not fold it into the roles epic page.

**Known / partial (starting material, not decided):**
- Two open questions: **(a) role-assignment mechanism** — all roles by default / user chooses / attached to specific pages/contexts; **(b) role-execution mechanism** — leading hypothesis is the same user input routed through different role prompts (a custom role prompt attached to the user's message), but this needs real research + a decision.
- Partial pattern already in the ResolvePM design: **contextual pinning** (Product Director on Roadmap; agents on Team tab), the **Orchestrator** global input bar (maps to `00-system/resolveos-entrypoint.md`, v1 = read + navigate), **Team tab v1** (role setup/onboarding + stand-up room).
- **Dependency:** feeds/blocks **N5 (Team + Agent Chat)** — the highest-complexity Alpha ticket (Batch C), not yet built. Decide the mechanism before N5 is built.
- Related backlog: **"Define user journeys per build phase"** (MVP / Alpha / Beta / Monetizable).

---

## Links summary
- **Roles epic (source of truth):** https://app.notion.com/p/393d8b53b7a8810ea8d1df77834670b6
- **Active Priorities (program state):** https://app.notion.com/p/390d8b53b7a881dda5e4daedbc76d54c
- **Repos:** `ResolveOS-intake-frontend`, `cap-pm-cockpit-alaria`
- **Role file references:** `strategic-product-director.md`, `technical-strategy-lead.md`, `product-manager.md`, `implementation-engineer.md`, `00-system/resolveos-entrypoint.md`

*Supersedes any earlier roles brief — the Notion epic page is the truth, this only points at it.*
