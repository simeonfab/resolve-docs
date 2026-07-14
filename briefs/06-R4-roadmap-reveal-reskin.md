---
status: current
---

> ⚠️ **SUPERSEDED by [`briefs/ALPHA-BUILD-TICKETS.md`](./ALPHA-BUILD-TICKETS.md) where they differ** — that pack (THE AUTHORITY for the Alpha build) carries the binding element / action / motion contract per screen. Use this brief for background only.


# Ticket R4 — Roadmap Reveal Reskin

**Repo:** `cap-pm-cockpit-alaria`
**Type:** Reskin. Backend (Now/Next/Later grid data from confirmed themes/to-dos) already exists from Chunk 03.
**Reference file (pixel-exact):** `ResolvePM Roadmap Reveal.dc.html`
**Parallel-safe** with every other ticket in this repo.

## Layout
- Same chrome-free shell (brand mark, 3-segment progress bar — this screen is segment 3 of 3, final segment).
- Eyebrow: "Your roadmap." Headline (serif): **"Here's the whole picture."** — this is final copy, not placeholder.

## The grid — single unified structure, real technical requirement
- Grid: themes as rows (dark labelled boxes), **Now / Next / Later as columns**. **Must be built as one unified grid/table structure — do not use separate grid containers for the header row versus the theme rows.** This exact bug happened in an earlier build pass: separate grids drift out of alignment because nothing guarantees they stay in sync. One grid, column headers as part of it.
- "Now" column cells have an amber border (emphasised); Next and Later columns use the default border colour.
- Empty cells (a theme with nothing in Next or Later yet) show a dashed border and "Nothing yet" in muted text — do not leave truly empty/blank cells.
- **Reveal animation, exact order:**
  1. Blank.
  2. Theme label boxes (dark, left column) fade in first, with their text — one by one or together, but before anything else appears.
  3. Column headers + "Now" column cells fade in, text populating.
  4. "Next" column fades in after a beat.
  5. "Later" column fades in after that.
  (Reference the file's actual animation-delay values for exact timing — do not compress.)

## Product Director box, below the grid
- Card with an avatar ("P" in an amber circle), name "Product Director," role subtitle "Ask about your roadmap, or add something new."
- Prompt text: "What do you think — anything missing, or something you'd sequence differently?" (treat as swappable copy, structure is final).
- Text input + Send button — this should route into a real conversation with the Product Director role/agent (ties to Ticket N5, Team tab's agent logic) — **this is also the add-theme entry point**, not a separate UI pattern.

## Footer
- **"Open my roadmap" button** (dark, with arrow icon) — this is the actual moment the roadmap gets marked confirmed. Tapping it should trigger the same backend confirmation action already built in Chunk 03 (writes to `initiatives`/`work_items`, sets `roadmap_confirmed_at`) if it hasn't already fired earlier in the flow — **confirm with the team whether confirmation already fired after the third theme card (Ticket R2) or fires here instead; do not assume, this determines whether tapping this button needs to trigger a write or just navigate.**
- Leads to Ticket N2 (OS Reveal), which then settles onto the Dashboard.

## Acceptance criteria
- Single unified grid structure (not separate header/row grids) — this is a hard requirement, not a preference.
- Reveal animation matches the specified order and timing.
- Real theme/to-do data throughout.
- Product Director box is a real, functioning entry point into that role's chat/logic, not a static decoration.
- Confirmation-trigger timing question above is explicitly resolved (not assumed) before this ticket is considered done.
