---
status: historical
note: "Session handoff (30 Jun 2026) - consumed."
---

# Resolve — Session Handoff Brief (30 June 2026)

Use this to brief the ChatGPT orchestrator on everything decided tonight. This is a full catch-up, not a summary — read it in full before doing anything.

---

## What Resolve is

Resolve is an AI-assisted project clarity tool for non-technical founders and solo PMs. It takes scattered project context (notes, old AI chats, half-finished plans, things spread across tools) and turns it into a clear path forward with one concrete next move.

It is NOT called ResolveOS externally. The internal layers are:
- **Resolve Intake** — the entry flow (current focus, only thing being built)
- **ResolveOS** — the internal AI framework/intelligence layer (not a product, not marketed)
- **ResolvePM** — the PM dashboard (not current focus, parked)

---

## Current focus — locked

**Resolve Intake only.** Specifically: the Existing Messy Project journey. One route, validated externally by 5 real testers, before anything else moves.

Exit criteria before expanding scope:
- [ ] 5 external (non-family) testers complete the flow
- [ ] They understand each screen's purpose without explanation
- [ ] They reach the last screen and can say what they'd do with the output
- [ ] Known issues from previous tests have been fixed and re-tested

Do not propose, design, or scaffold anything outside this. If a good idea comes up, log it as a backlog item. The scope discipline doc is in `resolve-intake-focus-directive.md`.

---

## Intake flow structure (redesigned tonight)

| Step | Screen | Purpose |
|---|---|---|
| 0 | Name + journey select | Name personalises all downstream screens. Journey options: Existing Messy Project (active), New Idea (coming soon), Stuck/Not Sure (coming soon) |
| 1 | Bring it in | Large brain dump input. Optional supporting material (collapsed). Clearbase import shown as seeded example — see below. |
| — | Processing reveal | 2.5s animated ring: "Reading your project… Finding the patterns… Path found." Auto-advances. No button. This is an emotional anchor — do not remove. |
| 2 | Understanding + gaps | 4 summary cards derived from input. Confirm/Edit/Reframe. Two targeted questions only (not a sorting system). Assumptions shown transparently. |
| 3 | Your next move | Single dark focus card with one concrete recommendation. First milestone. 3-phase roadmap. One open decision. |
| 4 | Win screen + continue | 🏆 win banner ("You just cleared the fog, [name]."). Recommended first action as primary card. 6 secondary action cards relevant to the project. Free text input as fallback. |

---

## The seeded example (Clearbase)

Clearbase is a fictional third-party note/planning app. On Screen 1, it appears as if the user imported their project summary from Clearbase. This makes the flow feel like it integrates with tools people already use, shows the value of a structured import, and gives testers something realistic to react to.

**The fictional project:**
- User: small business owner, handmade homewares e-commerce
- Business: "Nook & Thread" — trading for 4 years, wants to rebrand to move upmarket and justify higher prices
- What exists: Shopify store, 3 name shortlists, mood board (Canva), logo draft v2, 1,400 existing customers, 8k Instagram
- Where it's stuck: no finalised name, no customer migration plan, no agreed order of work
- Deadline: 8 weeks before peak gifting season
- Resolve's recommendation: lock the name first — everything else is blocked until then

This data flows through every subsequent screen — every card, recommendation, roadmap, and action references this project specifically. Nothing is generic after Screen 1.

---

## Design direction — do not lose or change without explicit instruction

**Palette:**
- Body: `#F2EDE4` (warm cream)
- Ink: `#1A1A18`
- Amber: `#C8A040` — active states, progress bar, imports
- Terracotta: `#8B4A2A` — eyebrows, section labels, card labels
- Cards: `#FFFFFF`
- Nav: `#1E1E1A` (dark)
- Primary CTAs: ink-black (`#1A1A18`), not teal

**Typography:**
- Display: DM Serif Display — H1 and H2 only (one dominant serif thing per screen)
- Body: Inter — everything else

**Tone:**
- Conversational, plain English, active voice
- No jargon, no ALL CAPS labels
- Sentence case + icon instead of label-only

**Emotional anchors (do not remove):**
1. The processing reveal (S1→S2) — animated amber ring, 3-phase label sequence, auto-advances
2. The win banner (S4) — dark background, 🏆 icon, personalised to name and specific project

---

## Layout — responsive

**Mobile (default):** Full screen, single column. No phone frame. Nav at top, content scrolls, bottom bar sticky.

**Desktop (768px+):** Two-column grid.
- Left: 280px dark sidebar (`#1A1A16`) with 4 step indicators (active/done/upcoming states that update as user progresses), divider, and a "after this session — unlock save/return" teaser badge. This is the paywall hook, visible throughout, not pushy.
- Right: content area, wider padding, slightly larger type (H1 36px, H2 28px)
- Nav spans full width
- Action grid goes to 3 columns on desktop

---

## Gamification — Phase 1 done, rest is future

**Implemented tonight:**
- Processing reveal animation (makes the AI reading feel earned, not instant)
- Personalised win banner on Screen 4 ("You just cleared the fog, [name]. A messy rebrand… turned into a focused path forward in under 10 minutes.")

**Future (do not build now):**
- Session-based streaks (sessions completed, not daily logins — Resolve is not a habit app)
- Project milestone log (running record of every project resolved)
- "Path taken" confirmation (user marks a recommended move done when they return)
- Clarity score (visible improvement as gaps close and decisions get made)
- Unlockable next steps (some S4 actions locked until first move complete)
- Full-screen "battle won" moments at real milestones (first move done, first paying customer)

**Rules that never change:**
- No daily streak pressure or guilt mechanics
- No points or leaderboards
- No badges for trivial actions
- The reward is the transformation itself feeling significant

Full brief in `resolve-gamification-brief.md`.

---

## Commercial goal

**£180/month short-term target** — covers Claude Pro + ChatGPT Pro subscriptions, which currently bottleneck engineering throughput when limits are hit.

- **Tier B (start here):** £30–40 per guided session/business plan for personal network (church contacts, friends with businesses). Low friction, high trust, reachable now.
- **Tier A (later):** ~£200 guided intake session for PMs/founders. Channel not yet defined — wait for Tier B evidence first.

---

## Tester recruitment — in progress

5 external testers is the target. Already sent: 50 LinkedIn DMs. 5 people have said yes. 2 have been sent the mockup link.

The test artifact is the HTML mockup. Testers are told it's a static prototype, buttons mostly highlight rather than process, nothing saves. They are asked to: screen-record + think out loud on each screen, then answer 4 questions at the end.

Copy-paste recruitment material is in `tester-recruitment-copy-paste.md`.

---

## Files produced this session

| File | Purpose |
|---|---|
| `resolve-intake-mockup.html` | **THE TEST ARTIFACT** — give to QA to review, then to engineer to implement |
| `resolve-gamification-brief.md` | Long-term gamification direction |
| `resolve-intake-focus-directive.md` | Scope discipline — what's in/out right now |
| `resolve-roadmap-clarity.md` | Phase-by-phase roadmap with unlock criteria |
| `resolveos-role-authority-diagnosis.md` | Parked — role epistemic authority design (do not act on yet) |
| `RESOLVEOS-SELF-HOSTING-GOVERNANCE.md` | Governance rules for when ResolveOS improves itself — store OUTSIDE the active ChatGPT project |
| `resolve-convergence-evaluation.md` | Evaluation of the 4-way product convergence (ResolveOS + ResolvePM + Intake + Runtime Surface) |
| `resolveos-competitive-matrix.md` | Feature comparison vs BMAD-METHOD and GitHub Spec-Kit |
| `tester-recruitment-copy-paste.md` | All tester outreach copy — DM, Indie Hackers post, LinkedIn post, screener, brief |
| `work-mode-activity-system-brief.md` | Work-mode system — Prompt 1 for orchestrator now, Prompt 2 for future ResolveOS implementation |

---

## What happens next — in order

1. **QA reviews the mockup** — treat the HTML file as the spec. Test each screen for clarity, comprehension, emotional weight, and data flow (does the Clearbase data carry through every screen correctly?). Flag anything that contradicts the design direction above.
2. **External testers run sessions** — collect screen recordings + answers to the 4 brief questions. Log results against exit criteria in the focus directive.
3. **Analyse results** — only after 3+ sessions. Do not redesign based on 1 or 2 data points.
4. **Engineer implements** — only after QA sign-off on the mockup and at least partial tester validation. The mockup HTML is the reference spec for implementation.
5. **Do not start anything else** until exit criteria are met.
