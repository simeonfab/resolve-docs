---
status: current
---

# Ticket N4 — Work Tab

**Repo:** `cap-pm-cockpit-alaria`
**Type:** New build.
**Reference file (pixel-exact):** `ResolvePM Work Tab.dc.html`
**Depends on:** Ticket N1 (nav shell). Parallel-safe with N3, N5–N8, subject to file-touch audit.

## Layout
- Header, eyebrow "Everything on your plate," headline "Your work" (serif).
- **Week-progress bar** near the top: a horizontal amber-filled progress bar + text label ("X of Y done this week").
- Sections, in this exact order, each with a coloured status dot and a count in the section header:
  1. **Now** (amber dot) — active tasks, amber left-border accent on each row.
  2. **Waiting on** (terracotta dot) — external-dependency stalls, each row showing a small "Waiting · Nd" chip.
  3. **Captured inbox** — Alpha scope: empty by default, fed by the Global Bar's capture function (Ticket N8). Show the empty-state message ("Nothing captured yet — anything you jot into the input bar lands here to sort later.") until that's wired up.
  4. **Upcoming from roadmap** — near-term items not yet marked active.
  5. **Synced from tools** — Alpha scope: empty, with its own empty-state message ("Connect Linear, GitHub or your calendar later and synced items appear here."). No integration wiring in Alpha.
- Each task row: checkbox (empty circle, or filled amber with a check + strikethrough text once done), task title, and a small tag showing which roadmap theme it traces to.

## Real requirement — never starts blank
**This tab must be pre-populated from confirmed Roadmap items on first visit.** Do not ship an empty state as the default for a new user — the Now/Waiting/Upcoming sections should already contain the real to-do items generated during Roadmap onboarding (Ticket R2/R3's backend, already built in Chunk 03).

## Acceptance criteria
- Section order and empty-state copy exactly as specified.
- Every non-empty task item traces to a real roadmap theme (shown via the tag).
- Captured inbox and Synced from tools sections exist and render correctly even though they're empty in Alpha — build the section shells now so no rework is needed when those features land later.
- Checkbox interaction correctly moves a task to a done/struck-through state.
