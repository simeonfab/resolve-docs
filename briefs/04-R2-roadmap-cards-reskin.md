---
status: current
---

> ⚠️ **SUPERSEDED by [`briefs/ALPHA-BUILD-TICKETS.md`](./ALPHA-BUILD-TICKETS.md) where they differ** — that pack (THE AUTHORITY for the Alpha build) carries the binding element / action / motion contract per screen. Use this brief for background only.


# Ticket R2 — Roadmap Theme Cards Reskin

**Repo:** `cap-pm-cockpit-alaria`
**Type:** Reskin, with one real behaviour clarification (see below). Backend (theme generation, `initiatives` writes, theme-vs-task check) already exists from Chunk 03.
**Reference file (pixel-exact):** `ResolvePM Roadmap Cards.dc.html`
**Parallel-safe** with every other ticket in this repo.

## Layout
- Brand mark top-left, standalone (chrome-free build sequence — no tab bar here).
- **3-segment progress bar** at top: Themes → To-do → Roadmap. This screen is segment 1.
- Eyebrow: "Building your roadmap."
- Stacked card treatment: current card in front, a peek of the next card behind it (subtle, lower opacity, capped size — do not let it grow with content, this caused a real bug in an earlier build pass).
- Progress dots (one per theme, 3 total): `done` (amber), `current` (dark), upcoming (muted border).

## Per-card content and behaviour
- Small tag line above the headline (confirm exact wording against the file — use what's actually delivered there).
- Theme headline (serif).
- **Two buttons: "Yes, add to my roadmap"** (amber, primary) and **"Not quite"** (bordered, secondary).
- **"Yes"**: theme drops into a growing "On your roadmap" list below the card stack (each entry: amber check-pop animation + theme name), and the flow advances to the next card.
- **"Not quite" — real behaviour clarification, build exactly this:** opens an inline correction panel within the same card (not a new screen) — a labelled input ("What should this theme be instead?") and a "Save and continue" button. Saving swaps the theme's wording to the corrected version, **re-runs the theme-vs-task check server-side against the corrected text** (existing logic from Chunk 03), and then behaves like "Yes" — the corrected version is what gets added to the roadmap and the flow advances.
- **There is no reject/remove path in Alpha.** Every theme ends up on the roadmap one way or another — either accepted as-is or accepted after correction. Do not build a way to discard a theme entirely.
- After the third theme: an end-state card — "That's your roadmap taking shape." / "Three themes, in your order of leverage. Next, Resolve turns them into a to-do list." with a **Continue** button leading to Ticket R3 (To-do List).

## Acceptance criteria
- Matches the reference file pixel-for-pixel.
- "Not quite" always opens inline correction, never a reject/discard action.
- Correction re-triggers the theme-vs-task server check before the corrected theme is accepted.
- Real theme data used throughout (from the actual Roadmap onboarding generation logic already built) — not the "Nook & Thread" example content.
- Progress dots and the "On your roadmap" running list both update correctly as themes are accepted.
