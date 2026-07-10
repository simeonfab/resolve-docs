---
status: current
note: "Imported from download copy '02-I2-intake-transition-1.md' (the current version); the copy without suffix was an older draft, now in archive/."
---

# Ticket I2 — Intake Transition + Bridge Animation

**Repo:** `ResolveOS-intake-frontend`
**Type:** Reskin of the final screen + a genuinely new piece (the bridge animation). This is the moment flagged all session as needing real design work — it now has one.
**Reference file (pixel-exact):** `ResolvePM Intake Transition.dc.html`
**Depends on:** Ticket I1 (reuses the same shell/stepper)

## Part 1 — Final intake screen (reskin)
- Same sidebar/stepper shell as I1, now showing all four steps as `done` except step 4 which is `active`.
- **Trophy celebration block** (dark card): 🏆 icon, serif headline "You just cleared the fog, [name]." — supporting line: "A messy rebranding process — turned into a focused path forward. That's the hard part done." (Note: "rebranding" here is from the example project; the real copy should reference the user's actual project type/summary, not be hardcoded to this example.)
- **"Next steps" / handoff card** (dark, below the trophy block) — this is the actual redesign:
  - Amber eyebrow with a small icon: "Decide this first"
  - Headline: the recommended action (from Call 2 output)
  - Supporting paragraph: "This becomes your first move inside Resolve — pinned and waiting, with everything you just untangled carried across. Pick up exactly where you left off, no re-explaining."
  - Three small pill chips: "Your first move, ready" · "Full roadmap" · "Project context" — communicating what actually carries across, not literal data.
  - **Single dominant amber CTA: "Continue in Resolve."** "Not now" is a quiet text link next to it, not a competing button.
  - Below the handoff card: a separate "Download report" row (dark, existing feature, keep as-is visually restyled to match).
- **Copy requirement, explicit:** this must never read like a system dialog ("send your confirmed session data") — it's benefit-led, framed as a reward, per the copy already written in the mockup.

## Part 2 — Bridge transition (new)
This is motion, not a static page. Total duration ~1.6 seconds, **auto-plays on tapping "Continue in Resolve" — no further click required**, then lands on the sign-up screen (Ticket R1, in the other repo).

Four beats, in order (exact timing per the mockup's keyframes):
1. **0.0s** — the trophy celebration moment (carried over from the screen the user was just on) holds briefly, then lifts/fades up and out.
2. **≈0.5s** — the recommended action pins into view with an amber checkmark popping in beside it ("Finalize the brand name" style — real action text, not hardcoded).
3. **≈1.0s** — the Resolve mark forms, with context chips ("Roadmap", "Context", etc.) fanning in beside it, communicating "setting up your workspace."
4. **≈1.6s** — settles into the sign-up screen (a faded/forming preview of the email field + button is visible during this beat, then the actual sign-up screen takes over).

**Visual continuity requirement:** nothing resets or hard-cuts during this sequence — stays on Ink background + Amber accent throughout, DM Serif Display carries the headline the entire way through. This is what makes it feel like one continuous product rather than two apps stitched together.

**Technical note:** since this crosses from a static site (Intake) into the Next.js app (ResolvePM), the bridge animation itself should run on the Intake side (before the actual navigation/redirect fires), landing the user on the real sign-up screen only once the animation completes. Do not attempt to run this animation across the actual page navigation boundary.

## Acceptance criteria
- Final screen matches the reference file exactly, with real (not hardcoded) recommendation/project data.
- Bridge animation plays automatically, ~1.6s total, in the four beats described, with no visual reset/hard-cut.
- Animation completes before landing on the real sign-up screen (Ticket R1's build).
- Report back if the cross-repo navigation timing (animation-then-redirect) is technically awkward — this is a reasonable thing to flag rather than force.
