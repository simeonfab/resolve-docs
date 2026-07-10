---
status: superseded
superseded_by: "mockups/ (ResolvePM_*_dc set) + strategy/resolve-brand-reference.md"
note: "Patched the archived chunk-03 spec's missing visual treatment. Visual authority is now the dc mockups + brand reference. Records the light-Surface-first decision (4 Jul)."
---

# Roadmap Onboarding — Visual Spec

**Why this exists:** `03-roadmap-onboarding-gate.md` specified data flow and interaction logic only — no visual treatment. That gap is why the built screen inherited old styling instead of the actual brand system. This spec fills that gap.

## Confirmed: light-Surface-first (4 July 2026)

ResolvePM matches Intake's actual pattern — light Surface background by default, dark reserved for specific dominant moments (e.g. the confirmation banner below). Not dark-throughout, which was the pre-existing app's inherited theme, not the actual brand system.

## Brand tokens in use (from `resolve-brand-reference.md`)
- Surface (page bg): `#F2EDE4`
- Ink (dark elements, primary text): `#1A1A18`
- Card bg: `#FFFFFF`
- Border: `#DDD8CE`
- Amber (accent, CTAs, active states): `#C8A040`
- Terracotta (eyebrow labels, section headings): `#8B4A2A`
- Type: DM Serif Display for H1/H2 only (one dominant serif element per screen, never two); Inter for everything else
- Radius: 10px default, 6px small
- Card padding: 16px

## Interaction model — confirmed 4 July 2026 (replaces the static form-field version below)

**Rejected:** stacked labeled input boxes (Goal / Measurable target / Definition of done as separate fields, all themes visible at once). Simeon's own words: "tiring... more fields to enter... this is not a great way." Confirmed via a working mockup — colors/fonts were approved, the box-based interaction was not.

**Confirmed model: conversational, one card, one theme at a time.**
- Themes present as a single stacked card (like reviewing one card at a time, not a scrolling form of many).
- **One framing statement, said ONCE, not repeated per theme:** e.g. "Based on what you told us, we've organized this into three themes." This is the moment that signals SYNTHESIS — the themes are not the user's raw words parroted back, they're Product Director's transformation of that raw input into structured goals. Critical distinction, not cosmetic: re-surfacing the user's own words as a question ("is this what you said?") wastes their time and feels redundant, since they just said it. The copy must communicate "we turned this into a theme," not "did we hear you right." Exception: if the user already explicitly framed something as a theme/roadmap item themselves, reflecting it back directly is fine — the synthesis framing applies to the general case, not universally.
- **Per-theme confirmation is now a fast yes/no, not multiple redundant options.** Simeon explicitly rejected the earlier 3-option model (yes/adjust/type-own) as redundant. Quick scan, tap yes or no per theme.
- **One single catch-all at the end of the whole set, not a per-theme escape hatch:** "Did we miss anything?" — one free-text/add moment after all themes are reviewed, not a "type your own" option duplicated on every single theme card.
- One theme fully reviewed before moving to the next (swipe/step-through, not "everything visible and scrollable at once").
- Keep to ~3 themes to start, to avoid overwhelming the user — matches the already-locked "2-3 additional goals" default.
- **Content requirement, not just UI:** the actual per-theme confirmation copy must be genuinely varied/specific to that theme's real content, not a repeated question template applied mechanically to each theme. This is a Product Director content-generation requirement, separate from the visual structure — flagging so it doesn't get lost as "just a UI thing."

**To-do list — future flourish, explicitly NOT needed now, noted so it isn't lost:** an animated "sheet of paper" reveal, showing completed steps crossed out at the top (e.g. "Initialise," "Resolve PM") with the next upcoming item highlighted. Nice-to-have polish for later, not part of this build.

**Mockup verified 4 July 2026** — Simeon confirmed this direction is "definitely more in mine, for sure," with the caveat that it likely needs further sharpening once built for real (exact wording, spacing, micro-interactions). Confirmed as the direction to build against, not a final pixel-perfect spec — Codex should expect some follow-up refinement after the first real build.

---

## Superseded section below (kept for reference only — do not build as originally written)


### Top confirmation banner (the "we think your goal is X" moment)
This is the screen's one dominant moment — treat it like Dashboard's win card, not like a form field.
- Background: `--ink` (`#1A1A18`)
- Eyebrow label: "ROADMAP CONFIRMATION", Terracotta, small, sentence case (not the current ALL CAPS treatment — brand voice explicitly avoids ALL CAPS labels)
- Headline: DM Serif Display, white/light text — this is the one serif element on the screen
- Body: "We think your goal is [X], based on what you told me" — plain Inter, per the naming resolution ("you told me," never "Intake")

### Tabs (Themes / To-do list)
- Simple underline-style tab switcher, not filled buttons — active tab gets Amber underline, inactive tabs Ink Soft text
- Sits directly below the confirmation banner, light Surface background from here down

### Theme cards
- Background: `--card-bg` white, border `--border`, 10px radius, 16px padding — NOT the current dark bordered-box treatment
- Eyebrow: "Theme [n]" + "Grounded in: [source]", Terracotta, small
- Fields (Goal, Measurable target, Definition of done, Context): Inter labels, clean bordered inputs on white background (not dark inputs on dark background as currently built) — labels above fields, generous spacing between fields (24px minimum), not cramped
- Agent-suggested values pre-filled (already fixed functionally) — style them as normal filled text, not placeholder-grey, so it's clear they're real suggestions, not empty prompts
- "Drag" handle: subtle, Ink Ghost color, not a bordered button

### To-do list items
- Same card treatment as themes (white bg, bordered, 16px radius)
- Fields: Activity, Trace to theme (dropdown), Why this matters — same spacing rules as theme cards

### Confirm button
- Full-width, Amber fill, Ink text, 10px radius
- Loading state (already fixed functionally): text changes to "Confirming roadmap...", button dims slightly and disables — keep this behavior, just apply it against the correct button styling once the rest of the visual system is in place

## What NOT to do
- Do not keep dark backgrounds throughout — that's the old theme, not the brand system
- Do not use bordered dark input boxes on a dark card — inputs should be light, clean, on white cards
- Do not use ALL CAPS section labels — brand voice is sentence case
- Do not add a second serif element anywhere on this screen — the confirmation banner headline is the only one
