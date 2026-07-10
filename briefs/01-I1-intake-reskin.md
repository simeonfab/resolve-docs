---
status: current
---

# Ticket I1 — Intake Reskin, Screens 1–5

**Repo:** `ResolveOS-intake-frontend`
**Type:** Reskin — this flow already exists and works in production. This ticket changes the visual layer and a small number of interaction details only. Do not change the underlying data flow (Call 1–4, gap questions, recommendation logic) unless explicitly stated below.
**Reference files (pixel-exact):** `ResolvePM Onboarding.dc.html` (screens 1–2), `ResolvePM Bring It In.dc.html` (screen 3), `ResolvePM Confirm Understanding.dc.html` (screen 4, includes its loader), `ResolvePM Next Move.dc.html` (screen 5, includes its loader)

## Shared shell, all five screens
- Dark (`Ink`) left sidebar, 308px wide desktop, containing: brand mark, a 4-step vertical stepper (Get started / Bring it in / Confirm understanding / Your next move), and an "After this session" card pinned to the bottom explaining what unlocks once signed up.
- Top bar: brand mark, horizontal progress bar, a "Reset" control (top right) — this is the existing dev/testing reset, keep it.
- Step states: `done` (checkmark, amber-bordered number circle), `active` (amber border, lit-up text), upcoming (dim/muted).

## Screen 1 — Name entry
- Single centered field, autofocused, pre-filled example shows "Simeon" for reference only — must be empty by default in production.
- Enter key submits. **Continue button must be disabled until a name is entered** — no dead-end state.
- Eyebrow: "Let's get started" (sentence case).

## Screen 2 — Welcome → problem (fused reveal)
- This is **one screen**, not two — do not build an intermediate screen between name entry and this.
- Animated reveal sequence (see the file's keyframes for exact timing): "Hi [name]." appears as the serif hero and holds. Then "Welcome to Resolve" fades, replaced by the question "What do you need help with?" (Inter, not serif — this screen's one serif element is the "Hi [name]" line only). Three option cards animate in below, staggered.
- Options: **"I have a messy project"** — live, selectable, pre-selected/highlighted in the mockup as the demoed choice. **"I have a new idea"** and **"I'm stuck, not sure where to start"** — both visually present but disabled, labelled "Coming soon."
- Selecting the live option advances to screen 3 (Bring it in).
- The top step-counter/progress element is **hidden** during this specific screen — the sidebar stepper alone carries progress here.

## Screen 3 — Bring it in
- Large textarea is the visual hero, with a mic/voice-input icon docked bottom-right inside it.
- Below the textarea: three starter chips ("A rebrand that's stalled", "Too many notes, no plan", "Deciding between options") — tapping one pre-fills the textarea with a scaffold (existing behaviour, just reskinned).
- Helper row below that, in this exact hierarchy: **"Try an example"** (primary, filled/bordered amber style) · **"Add material"** (disabled, "Coming soon", dashed border, muted) · a text link on the right: "Coming from another AI chat? Get a prompt to pull it in."
- Footer: Back button + **"Build my path"** (disabled by default, with a small hint text "Add a little context to continue" next to it while disabled — enable once the textarea has content).

## Screen 4 — Reading loader → Confirm Understanding
- **Loader:** narrated, not a bare spinner. Amber ring spinner plus a 4-item checklist that ticks off in sequence (see the file's keyframe timing — ~5 second total loop for preview, runs once in production): "Reading what you shared" → "Separating what's decided from what's open" → "Spotting the gaps" → "Shaping your path." Headline: "Reading your project…"
- **Confirm screen**, after the loader:
  - Headline: "Here's what Resolve got from that." Subhead: "Correct anything wrong — it shapes everything that follows."
  - Four understanding cards in a 2×2 grid: 📁 Project · 🎯 What you want · 📍 Where it is · ❓ Biggest uncertainty. **Each card has its own pencil icon (top-right) for individual inline editing** — this replaces any single shared "Edit" button from the existing build.
  - Below the grid: "A couple of things Resolve still needs" — optional gap questions, each with its own input field. **Open question, do not decide unilaterally: are these required or skippable before continuing?** Build the UI as optional-looking (per the mockup) but confirm with product before enforcing either way at the logic layer.
  - Below that: "What Resolve is assuming" — a single amber-bordered highlight box with the assumption text and a **"Not quite?" inline challenge link** (terracotta, right-aligned) — tapping it should let the user correct the assumption inline, not navigate away.
  - Footer: Back + **"This looks right — build my path"** (forward action).

## Screen 5 — Weighing loader → Your next move
- **Loader:** same narrated pattern as screen 4's loader, different checklist copy: "Weighing what matters most" → "Checking against what you confirmed" → "Sequencing the work" → "Choosing your first move." Headline: "Weighing it up…"
- **Next-move screen:**
  - Page title "Here's what to do next." is **Inter, not serif** — the screen's one serif element moved to the dark recommendation card below.
  - Dark "Do this first" card (🎯 eyebrow in amber): serif headline stating the recommended action, supporting paragraph in lighter grey explaining why.
  - Below it: a lighter bordered "🏁 First milestone" card — a plain-language definition of what "done" looks like for this recommended action.
  - "What happens after this" — a 3-column timeline (This week / Next / Later), each a bordered card with an eyebrow, a short title, and an italic "Output:" line describing what that stage produces. The "This week" card is amber-bordered/emphasised (it's the active one).
  - Footer: Back + **Continue** — leads to the celebration/handoff (Ticket I2).

## Explicitly not in scope for this ticket
- The bridge transition animation and final trophy/handoff screen — that's Ticket I2.
- Any change to the actual Call 1–4 backend logic, gap-question generation, or recommendation algorithm.

## Acceptance criteria
- All five screens match their reference file pixel-for-pixel (colours, spacing, type, radii).
- Continue/Build-my-path buttons correctly gate on required input at each step.
- Both loaders show the narrated checklist, run once in production (not looping).
- Confirm Understanding's four cards are independently editable.
- Report back anything ambiguous rather than guessing, especially the gap-questions required/skippable question.
