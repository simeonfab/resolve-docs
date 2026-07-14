---
status: current
---

> ⚠️ **SUPERSEDED by [`briefs/ALPHA-BUILD-TICKETS.md`](./ALPHA-BUILD-TICKETS.md) where they differ** — that pack (THE AUTHORITY for the Alpha build) carries the binding element / action / motion contract per screen. Use this brief for background only.


# Ticket R3 — To-do List Reveal Reskin

**Repo:** `cap-pm-cockpit-alaria`
**Type:** Reskin. Backend (to-do generation from confirmed themes into `work_items`) already exists from Chunk 03.
**Reference file (pixel-exact):** `ResolvePM Todo List.dc.html`
**Parallel-safe** with every other ticket in this repo.

## Layout
- Same chrome-free shell as R2 (brand mark, 3-segment progress bar — this screen is segment 2 of 3).
- Eyebrow: "Your first week." Headline (serif): "Here's where to begin." **— confirm exact copy against the file; the underlying brief document explicitly flags this headline as placeholder, so treat the copy as swappable, but the structure/animation below is final.**
- Subhead: "Built straight from your roadmap — the first concrete moves, in the order they matter."

## The "sheet" animation — build exactly this sequence, do not simplify
This must auto-play once on load, then stop (does not loop in production — the reference file loops for preview purposes only).

1. Row 1 types/appears and its text reads "Set up your Resolve workspace" — a checkmark pops in (slightly oversized, settles to normal size) — then the row strikes through.
2. Row 2 appears the same way: "Confirm your roadmap" — checkmark pops — strikes through.
3. A **dark "Up next" card** appears, containing the actual first real task (from the confirmed to-do list, real data).
4. Below that, upcoming tasks appear one by one, lighter-weight rows with a small outline dot — **count is flexible (2–5 items), based on what's actually in the confirmed to-do list, not a fixed number.**
5. A completion counter appears: "X of Y complete" (real numbers from the actual to-do list state, not hardcoded "2 of 12").
6. Footer button fades in last: **"See it as a roadmap"** — leads to Ticket R4 (Roadmap Reveal).

**Timing:** should feel deliberate and smooth, not rushed — refer to the reference file's actual animation-delay values as the timing spec (do not compress them).

## Real bug to avoid, from an earlier build pass of similar work
A previous version of this pattern briefly showed the literal text "undefined" because a reset step cleared text before it was read for typing. If implementing a similar type-in-effect for any of these rows, make sure the source text is captured before any reset/clear step runs, and guard against typing an empty/undefined string.

## Acceptance criteria
- Matches the reference file's visual treatment and animation sequence exactly.
- Auto-plays once, does not loop, in production.
- All task/counter content is real data from the confirmed roadmap/to-do list — never hardcoded example content.
- "See it as a roadmap" button only appears/becomes active after the animation sequence completes — do not let the user skip ahead mid-animation unless that's a deliberate, explicitly confirmed decision (flag if you think it should be skippable).
