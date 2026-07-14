---
status: current
note: "Sequencing superseded by /RESOLVE-ALPHA-MASTER-PLAN.md (its own words: 'superseded by this doc for sequencing; still holds ticket detail'). Ticket-level detail remains authoritative."
---

> ⚠️ **SUPERSEDED by [`briefs/ALPHA-BUILD-TICKETS.md`](./ALPHA-BUILD-TICKETS.md) where they differ** — that pack is THE AUTHORITY for the Alpha build (binding element / action / motion contract per screen). This master plan is retained for context.


# ResolvePM Alpha — Full Engineering Handoff — Master Plan

## Purpose
This covers every screen needed for the complete Alpha end-to-end journey, from Intake's first screen through to Settings. Design is final and delivered as pixel-exact HTML mockups (`.dc.html` files) — these are the literal spec, not inspiration. Engineers must build to what is in the mockup exactly. Do not deviate, do not "improve," do not fill gaps with your own judgement — flag ambiguity back instead.

## Repo split (hard boundary, no shared context)
- **Screens 1–6 (Intake)** → `ResolveOS-intake-frontend` (static HTML/JS site)
- **Screens 7–17 + Global Bar (ResolvePM app)** → `cap-pm-cockpit-alaria` (Next.js, Supabase)

These are separate Codex/engineering sessions. Nothing here requires shared context between the two repos beyond the handoff API contract already built (Chunk 02).

## Copy correction — read before building
Earlier planning used "See the whole picture" as placeholder wording for the to-do→roadmap transition. **The delivered mockups override this.** Build to the actual copy in the files:
- To-do List's forward button: **"See it as a roadmap"**
- Roadmap Reveal's own headline: **"Here's the whole picture."** (a headline, not a button)
- Roadmap Reveal's forward button (into the app): **"Open my roadmap"**

## Standing rules (apply to every ticket below)
1. **Pixel-exact.** Every colour, spacing value, font, radius, and copy string in the referenced `.dc.html` file is the literal spec. Do not introduce new styles, even ones that seem like an improvement.
2. **Brand tokens, for reference only (already baked into every mockup):** Ink `#1A1A18`, Surface `#F2EDE4`, Card `#FFFFFF`, Border `#DDD8CE`, Amber `#C8A040`, Terracotta `#8B4A2A`. DM Serif Display for exactly one headline per screen; Inter everywhere else. 10px/6px radii. Sentence case only, never ALL CAPS.
3. **"Bizarro Superman" rule.** If an instruction in a ticket is not specific enough to build without guessing, that is a failure of the ticket, not permission to guess. Stop and ask.
4. **File-touch audit before parallelising.** Before starting two tickets marked parallel-safe at the same time, list which files/components each will touch. If they overlap (shared nav shell, shared layout, shared types), run them sequentially instead.
5. **Real production verification required, not just local tests.** A ticket is not done until it has been checked against the actual live deployment with real data — matching the standard already used for Chunks 01–03 (see Lessons Learned in Notion if you need the history).
6. **Never invent data.** Every dynamic value shown in the mockups (task names, agent replies, roadmap themes) is illustrative placeholder content from a fictional example project ("Nook & Thread rebrand"). Real data comes from the actual database (`initiatives`, `work_items`, `projects`, `project_sessions`) per the schema already built in Chunks 01–03. Do not hardcode the example content.
7. **Report deviations and ambiguities explicitly**, the same way previous chunks in this project have — do not silently resolve something you're unsure about.

## Sequencing

### Track 1 — Intake (`ResolveOS-intake-frontend`), fully independent
1. **Ticket I1: Intake reskin, screens 1–5** (Name entry → Welcome/problem → Bring it in → Confirm Understanding → Next Move)
2. **Ticket I2: Intake Transition + bridge animation** (screen 6) — depends on I1 being in place (reuses its shell/stepper)

### Track 2 — ResolvePM app (`cap-pm-cockpit-alaria`)

**Reskins — backend already exists, fully parallel with everything else:**
- Ticket R1: Sign-in (OTP)
- Ticket R2: Roadmap Cards (theme confirmation)
- Ticket R3: To-do List
- Ticket R4: Roadmap Reveal

**New-build — sequenced:**
- Ticket N1: App Shell / bottom tab bar + route gating — **build first**, everything else's mockups assume this shell exists
- Ticket N2: OS Reveal (first-run + recurring open) — depends on N1 and N3 both existing
- Tickets N3–N7 (Dashboard, Work, Team+Agent Chat, Knowledge Base, Settings) — parallel-safe once N1 exists, subject to the file-touch audit rule
- Ticket N8: Global Bar / Orchestrator — scaffold can start early, but final per-tab contextual wiring depends on N3–N6 existing

## Real gaps flagged, not solved in this plan — engineers should raise these, not invent answers
- **PDF generation/serving for Knowledge Base's "full report" download** — no backend exists for this yet.
- **Settings' "Reset onboarding flow"** — confirmed to be the same feature as the earlier testing-only reset button already built; if any divergent behaviour was built for the earlier version, reconcile against this ticket's spec, not the other way round.
- **First-run vs. recurring OS Reveal state** — needs a real persisted flag (e.g. on `projects` or `auth.users`) tracking whether a given user has seen the grand reveal before. Not yet designed at the data layer — flag back if unclear how to implement.
- **Gap questions on Confirm Understanding (required vs. skippable)** — still genuinely undecided. Build the UI as shown (skippable-looking, optional fields) but confirm with product before enforcing either way.
- **To-do list headline copy and agent intro/chat copy** — explicitly marked placeholder in the brief itself; build the structure, treat copy as swappable.
