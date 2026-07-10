---
status: superseded
superseded_by: "briefs/00-MASTER-PLAN.md + /RESOLVE-ALPHA-MASTER-PLAN.md"
note: "Old chunk-plan generation, replaced by the I/R/N brief set."
---

# Chunk 04 — Dashboard

**Depends on:** `03-roadmap-onboarding-gate` (reads the confirmed to-do list/themes)
**Can run in parallel with:** `05-work`

## What this chunk does
Builds the Dashboard tab — the screen that answers "what do I do next?"

## Cards, in order

### 1. Win for today (primary, full width)
- Visual treatment per brand reference: background `--ink` (`#1A1A18`), matching the "win banner" pattern already established in Intake (dark background, do not remove/reinvent this pattern). Headline uses DM Serif Display — this is the one dominant serif element on this screen, no other Dashboard element should use serif type. Amber (`#C8A040`) for the accent/eyebrow label.
- The one highest-leverage next action, pulled from the confirmed near-term to-do list.
- Clickable straight into the relevant work — the entire card is the tap target (no separate open/close button).
- **Interaction pattern:** clicking the card expands it in place, dims the background around it, and surfaces detail inline. This pattern applies broadly across Dashboard cards, not just this one.
- **Routing is contextual to the item's type** — a work item opens into doing it, an agent-relevant item opens that role's chat, an email-shaped item drafts the email, etc. This requires each item to carry a type that determines where the click goes.
- **Persistence — important, do not get this wrong:** this must NOT be recomputed fresh on every page load. It is a persisted record (Supabase), computed once and re-read identically until explicitly superseded. Recomputing per session produces contradictory recommendations across sessions in the same day, which is broken UX, not a feature.
- **Trigger for the next recommendation:** when the current activity is marked done, the next activity from the confirmed to-do list surfaces automatically. This is the only trigger — no separate "refresh" mechanism.

**Secondary cards (2-4 below) — shared visual treatment:** white background (`--card-bg` `#FFFFFF`), border `--border` (`#DDD8CE`), 10px radius, 16px padding. Terracotta (`#8B4A2A`) eyebrow labels, sentence case (no ALL CAPS per brand voice rules). Sans-serif only (Inter) — serif stays reserved for the win card above.

### 2. Blocked / waiting-on card
- Narrower than a full dependency list. Surfaces things stalled because they need external input — distinct from the user's own next action. Explicitly not a general blockers/dependencies dump.

### 3. Schedule / briefing card
- Meetings + to-dos for today, calendar-sourced. **Alpha scope note:** if calendar integration isn't available yet, this card can show whatever to-do items are due today from the Work list; full calendar integration is not required for Alpha.

### 4. Pointer / redirect card
- A less-directive nudge into another area — "go work on this" — distinct in tone from the primary win card (which is the strong directive).

## What was explicitly considered and cut — do not build this
A Dashboard-specific input/capture prompt was proposed and rejected. Reasoning: it would either be redundant with the global bar (same capture mechanism, different label) or it doesn't actually exist as a distinct function. The global bar (see `07-global-bar.md`) already covers input on every screen, including Dashboard. Do not add a separate capture box to Dashboard.

## Naming note
Do not use the word "Intake" anywhere in Dashboard copy or elsewhere in ResolvePM. If referencing the prior session's data, phrase it in plain language (e.g. "based on what you told us") — a final replacement term for "Intake" has not been decided, so use plain descriptive language rather than a specific product name.

## Acceptance criteria
- Win-for-today card reads from the persisted recommendation, not recomputed on each load
- Clicking any card expands in place with a dimmed background — no separate open/close buttons anywhere
- Marking the current win-for-today item done surfaces the next to-do list item automatically
- Blocked, schedule, and pointer cards are present and populated (schedule card can use Work due-dates if no calendar integration exists yet)
- No Dashboard-specific capture input exists anywhere on this screen
