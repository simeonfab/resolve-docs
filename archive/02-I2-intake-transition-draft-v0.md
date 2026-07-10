---
status: superseded
superseded_by: "briefs/02-I2-intake-transition.md"
note: "Earlier draft of the I2 brief. Renamed on import (was '02-I2-intake-transition.md') to free the canonical name for the current version."
---

# Ticket I2 — Intake Transition + Bridge Animation

**Repo:** `ResolveOS-intake-frontend`
**Type:** Revision of the already-built I2 — final-screen restructure + re-paced bridge + dark hand-off.
**Reference file (pixel-exact):** `ResolvePM Intake Transition.dc.html` (updated version — the one that ends at the handoff card with a "Download report" secondary button; **not** the earlier version with the six output boxes/pills/"Not now").
**Depends on:** Ticket I1 (reuses the same shell/stepper) and the dark Sign-in re-theme (Ticket R1, in `cap-pm-cockpit-alaria`) for full end-to-end continuity.

> **Current state — read before starting.** I2 was already built to an *earlier* design on branch `claude/intake-transition-bridge-boxaa7` (commit `b914d6c`). That build had a six-box output grid, three carry pills, a "Not now" link, and a bridge that ended on a *light* sign-in. This ticket **revises that build** to the updated mockup below. Continue on that branch — do not start fresh.

---

## Part 1 — Final intake screen (restructure)

Same sidebar/stepper shell as I1, all four steps `done` except step 4 `active`.

**The screen now ends at the handoff card. Nothing below it.**

- **Trophy celebration block** (dark card): 🏆, serif headline "You just cleared the fog, [name]." + supporting line summarising the user's project. **All of this is dynamic from Call 1/2 data — name and project summary must not be hardcoded to the "rebranding" example.**
- **"Next steps" eyebrow** (terracotta, sentence case).
- **Handoff card** (dark, the redesign):
  - Amber "Decide this first" eyebrow with the check-circle icon.
  - Headline = the recommended action from Call 2 output (verbatim, dynamic).
  - Supporting paragraph (benefit-led, per the mockup copy) — never reads like a system dialog.
  - **"Download report"** as a single **secondary** button (mockup's `.secbtn` style) directly above the CTA. Wire it to the existing report generate/download behaviour that the removed output boxes previously used — that functionality is not lost, just relocated here.
  - **"Continue in Resolve"** — the dominant amber CTA. **Unchanged: same size and structure as the current build. Do not enlarge, stretch, or restructure it.**

**Explicitly removed from this screen (do not keep, do not hide-while-wired):**
- The entire output-template grid — all six cards ("Your plan for this week", "Explain the rebrand to someone else", "Download your rebrand project brief", "Update your team", "What to avoid right now", "Pick this up in Claude/ChatGPT").
- The "Or ask Resolve something specific…" textarea.
- The three carry-over pills ("Your first move, ready" / "Full roadmap" / "Project context").
- The "Not now" link.

**Reasoning (context, so the removals aren't second-guessed):** first move is already shown on the handoff card; the full roadmap is the big post-transition reveal, not handed over free here; project context alone isn't worth a chip; the full project report via "Download report" is enough output for this moment; a single dominant action is cleaner than CTA + "Not now".

---

## Part 2 — Bridge transition (re-paced + dark hand-off)

Motion, not a static page. **Runs once** on tapping "Continue in Resolve" (the mockup loops only for preview) — do not loop in production. Total ~6.8s of animation, then lands on the real dark sign-in.

**Four held beats, ~2s each, with smooth cross-transitions (not the old fast flash):**
1. **Trophy zoom (~0–2.3s):** the trophy box *already on the final screen* smoothly **grows/expands to take over the screen** — one continuous zoom, **not** a cut to a fresh fog screen.
2. **First move pins (~2.4–4.5s):** the user's **real** next task glides in with an amber check + "Bringing your first move with you." **Dynamic — not hardcoded "Finalize the brand name."**
3. **Workspace forms (~4.6–6.6s):** the Resolve mark forms, context chips ("First move", "Roadmap", "Context") fan in + "Setting up your workspace."
4. **Dark sign-in (~6.8s → holds):** settles on the dark "Create your Resolve workspace" preview (email field + "Send code"), then hands to the real dark sign-in.

**Continuity requirements:**
- Stays on **Ink + Amber the entire way**; DM Serif Display carries the headline throughout.
- Ends on a **dark** sign-in — **no dark→light flash mid-flow.** (The real sign-in is being re-themed dark in R1; see dependency below.)
- Keep the run-once + concurrent-backend behaviour already built: the final beat holds until the real `signup_url` is ready before redirecting, but the earlier beats **always play fully even on a fast backend** — nothing gets skipped.
- Honour `prefers-reduced-motion`.
- The animation runs on the Intake side **before** the redirect fires — do **not** run it across the actual page-navigation boundary.

---

## Cross-repo dependency (know this, don't act on it here)

The sign-in screen this redirects into lives in `cap-pm-cockpit-alaria` (Ticket R1) and is being re-themed from light to dark separately. Your bridge should end on the dark preview and redirect as normal. **If the redirect target is still the live *light* sign-in when you test, that is expected until R1's dark re-theme lands — flag it, do not restyle the sign-in from this repo or work around it.**

---

## Out of scope
- Any change to the Call 1–4 backend logic, gap-question generation, or recommendation algorithm.
- Restyling the actual sign-in screen (that's R1, other repo).

## Environment rule
Ephemeral container — **commit as you go and push.** Do not leave the deliverable uncommitted.

## Acceptance criteria
- Final screen matches the updated reference file exactly: trophy → "Next steps" → handoff card, nothing below. Output grid, textarea, pills, and "Not now" are gone.
- "Download report" is the single secondary button above the CTA and triggers the existing report download.
- "Continue in Resolve" is unchanged in size/structure.
- Trophy summary, recommended action, and first-move task all render from real Call 1/2 data — no hardcoded example content.
- Bridge plays once, four held beats ~2s each, beat 1 is a continuous trophy zoom (no cut), ends on the dark sign-in with no dark→light flash.
- Report back anything ambiguous rather than guessing.
