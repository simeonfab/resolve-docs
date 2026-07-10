---
status: superseded
superseded_by: "briefs/00-MASTER-PLAN.md + /RESOLVE-ALPHA-MASTER-PLAN.md"
note: "Old chunk-plan generation, replaced by the I/R/N brief set."
---

# ResolvePM — Alpha Build Plan (Top Level)

**Purpose of this document:** single top-level reference for Codex covering the full Alpha build, its chunk breakdown, dependencies, and what can run in parallel. Each chunk has its own detailed markdown file (`01-...md` through `09-...md`) with full spec, acceptance criteria, and explicit "do not invent" flags for anything still undecided.

**Standing principles that apply to every chunk (do not violate these):**
- Never invent data, copy, or behaviour not specified in a chunk file. If something is ambiguous, flag it rather than guessing.
- Prefer proven, modular, already-built tools/libraries over custom-built solutions (e.g. use Supabase's built-in magic-link auth rather than building custom auth).
- Never re-ask the user something already established elsewhere in the product — carry it forward and state it as a direct confirmation instead.
- Nothing enters an "active"/committed state (a Roadmap goal, a Work item) without a clear source: user confirmation, a trusted integration, or a direct trace back to the agreed Roadmap. Never invent work items or goals out of nothing.
- "Intake" is not a user-facing term. Do not expose the word "Intake" in any ResolvePM-facing copy.
- Design must be consistent with the Resolve Brand Reference Sheet — attached separately as `resolve-brand-reference.md`, give this file to Codex alongside these build docs. Key tokens (full detail in that file):
  - Colours: Ink `#1A1A18` (primary text, CTAs, win banner bg), Surface `#F2EDE4` (page bg), Card `#FFFFFF`, Border `#DDD8CE`, Amber `#C8A040` (active states, accents), Terracotta `#8B4A2A` (eyebrow labels, section headings), Nav Dark `#1E1E1A`
  - Type: DM Serif Display for H1/H2 only — one dominant serif element per screen, never two. Inter for everything else.
  - Radius 10px default / 6px small. Card padding 16px. Desktop content padding 40px/48px, mobile 24px/20px.
  - Mobile-first, max width 960px desktop, crossfade transitions ~200-300ms, never harsh cuts.
  - Do not introduce new visual styles outside this system.
- **Every chunk must include an explicit visual/layout spec, not just data/logic (added 4 July 2026, learned the hard way).** Chunk 03 was built with data flow and interaction logic only, no visual treatment specified — the result inherited the old pre-existing app's dark theme instead of the actual brand system, since nothing told Codex otherwise. Applies retroactively too: any already-built chunk that lacks a real visual spec should get one before further visual complaints are treated as "just needs polish."

**Alpha scope reminder:** Alpha is the testable MVP — hand-picked testers, functional not polished. No animation/transition polish, no integrations, no payment/billing, no voice capture. Testers use Simeon's own API setup.

---

## Chunk list and dependency graph

```
01-database-foundation
   |
   +--> 02-signup-handoff
   |        |
   |        v
   |    03-roadmap-onboarding-gate
   |        |
   |        +----------------+----------------+
   |        v                v                |
   |   04-dashboard      05-work              |
   |                                           |
   +--> 06-team (parallel-safe, needs only 01) |
   |                                           |
   +--> 09-navigation-shell (parallel-safe, needs only 01, can start immediately)
   |
   +--> 08-knowledge-base (parallel-safe, minimal stub, lowest priority, can start anytime)

07-global-bar (navigate-only for Alpha) — needs 09 (nav shell) to exist to route to;
   can be scaffolded in parallel but final wiring should happen after 04/05/06 pages exist.
```

## Build order recommendation

**Sequential spine (must happen in this order):**
1. `01-database-foundation`
2. `02-signup-handoff`
3. `03-roadmap-onboarding-gate`

**Parallel-safe once 01 is done (do not need to wait for each other):**
- `06-team` — only needs the database foundation, no dependency on Roadmap/Dashboard/Work
- `09-navigation-shell` — pure UI scaffolding/routing, no data dependency
- `08-knowledge-base` — minimal stub, lowest priority, no hard dependency

**Parallel-safe once 03 is done:**
- `04-dashboard` and `05-work` can be built at the same time — both read from the confirmed Roadmap/to-do list, but do not depend on each other's implementation.

**Last:**
- `07-global-bar` — scaffold can start early, but final routing wiring depends on the pages it navigates to (`04`, `05`, `06`) existing.

**Important caveat, not guessed away:** this dependency graph is based on product logic and the data model already documented, not on inspection of the actual current codebase structure. This document cannot guarantee file-level safety without seeing the live repo.

**Required step before running any chunks in parallel:** before starting two or more chunks marked "parallel-safe" at the same time, list which files/components each chunk will create or modify. If two parallel chunks touch the same file (e.g. a shared layout component, shared types, a shared nav component), flag it and run those two chunks sequentially instead, even though this document marks them parallel-safe. This audit is quick (a few minutes) and takes priority over the parallelization suggested above.

## Per-chunk files
- `01-database-foundation.md`
- `02-signup-handoff.md`
- `03-roadmap-onboarding-gate.md`
- `04-dashboard.md`
- `05-work.md`
- `06-team.md`
- `07-global-bar.md`
- `08-knowledge-base.md`
- `09-navigation-shell.md`
