---
status: current
note: "Top-level plan (9 Jul 2026). Verify live state against Notion Active Priorities before acting - this repo holds artifacts, not state."
---

# RESOLVE ALPHA — MASTER PLAN v1 (9 July 2026)

**Purpose of this document:** the standalone design doc for the Resolve Alpha project.
Attach it to any fresh chat, alongside the Notion Active Priorities page
(`390d8b53b7a881dda5e4daedbc76d54c`) and the phase-specific files listed per phase.
It states the long-term goal, every phase, what runs in parallel, what blocks what,
and which files each phase needs attached. Notion remains the live source of truth —
fetch it before acting; this doc is the map, not the state.

---

## 0. THE LONG-TERM GOAL

**Alpha =** a tester brings a messy project into Resolve (Intake), signs in, confirms
a roadmap, and lands in a working cockpit that helps them understand **what matters,
what to do next, and where the project now lives.**

Explicitly OUT of Alpha (Beta parking lot): deep ResolveOS orchestration, real agent
chat, roles-in-frontend execution, production hardening, monetisation/API cost model,
prompt-injection fencing (required only before any agent chat exists).

**The differentiation being tested:** **not just any direction — the right direction.**
Resolve gives direction, shows honestly what it is confident in versus what it inferred,
and refuses to manufacture a plausible plan out of nothing. Only Gate 1 (no real outcome)
halts. Gate 2 (no lever) marks affected themes provisional rather than refusing — see
briefs/BETA-EPIC-SHARPEN-LOOP.md. This supersedes the earlier 'structure and refusal'
framing.

**Two definitions everything rests on (final, tested):**
- A **theme** is a **lever**: continued effort keeps producing value, it has no
  natural endpoint, so someone must choose a stopping point. A **task** is spent
  after one push. An **outcome** cannot be pushed directly.
- A **definition of done** is the chosen stopping point: it must be **EVIDENTIAL**
  (evidence the outcome occurred — not the artifact, not a proxy) and **MINIMAL**
  (nothing removable while still evidencing the outcome).

**Architecture rule (non-negotiable, evidence-backed):** generation and enforcement
never share a model call. Generators propose; a separate floor refuses insufficient
intake; a separate referee rejects bad themes/DoDs clause-by-clause under the AND rule.

---

## 1. CURRENT STATE (as of 9 July 2026 — verify against Notion before acting)

**Live in production:** Intake screens 1–6 (I1+I2, dark sign-in, entry seam, exit
transition); N1 App Shell/Nav; R1–R4 reskins; N3 Dashboard (merged with spine).
**Known deferred:** N3 expand-in-place (parked for the animation pass); minor visual
bugs (un-itemised); seam pixel residual; desktop layout pass (mobile-first by design).
**The engine is broken and being replaced:** roadmap generation calls no model —
string concatenation, hardcoded 'medium' priority, no due dates, `{action} for
{theme}` titles, verbatim theme copying, a theme_index linkage bug, and a browser
round-trip. The pipeline in Phase 1 replaces all of it.
**Pipeline validation status:** referee clauses proven; floor Gate 1 (outcome)
proven; floor Gate 2 (lever) proven on the diagnostic set with the corrected
definition; generation-from-scratch known weak (manufactures on thin input) — which
is exactly what the floor+referee contain. All proof is on constructed data; the
sessions (Phase S) test generalisation on real humans.

---

## 2. THE PHASES

### PHASE 0 — VERIFY & FIX (gate for everything; ~1 day)
No building on unverified ground. Two items:
- **0.1 Parser fix.** The referee's strict verdict parser silently drops ~30% of
  verdicts (`PASS / reason` without the literal `REASON:` label). Loosen the parser
  or force structured output. Everything downstream trusts the referee.
- **0.2 Read the intake generation calls.** The ~4 OpenAI calls in the Intake repo
  that write `project_understanding` / `recommended_action` / `top_priorities` /
  `what_not_to_do` are [INFERRED] — never read. Read them; correct every inferred
  claim in the integration map before Phase 1 touches Stages 1–2.
**Parallel-safe:** 0.1 and 0.2 are independent; run simultaneously.
**Attach:** `08-integration-map.md`, `05-referee.md`. Repo: both
(`cap-pm-cockpit-alaria` for 0.1's eventual home; `ResolveOS-intake-frontend` for 0.2).

### PHASE 1 — THE PIPELINE (the engine; the critical path)
Replaces string-concat roadmap generation with the validated pipeline.
Build order (dependencies, not preference):
- **1.1 Referee service.** Package validator-v2 clauses as isolated reject-only
  calls + regenerate-with-reason loop, retry cap N=2, AND rule. Proven component;
  this is packaging. *Blocks: 1.3, 1.4.*
- **1.2 Floor service.** Gate 1 (outcome present) + Gate 2 (lever present, corrected
  wording — "continued effort keeps producing value, no natural endpoint"; do NOT
  use finish-line or diminishing-returns wording). Halts with ONE question on
  INSUFFICIENT. *Blocks: 1.4. Parallel with 1.1.*
  
  ⚠️ **SUPERSEDED IN TWO WAYS.** (a) The lever discriminator ('no natural endpoint') is neither necessary nor sufficient — see FLOOR-GATE-DESIGN-FINDING.md. (b) Gate 2 NO LONGER HALTS. It sets provisional=true on affected themes and logs to refusals as a diagnostic. Only Gate 1 halts. See BETA-EPIC-SHARPEN-LOOP.md. The file's architecture (isolated reject-only calls, one criterion per call, temp 0) stands and is proven.
- **1.3 Schema migration.** Add `outcome` (+confirmed) to project_sessions;
  `initiatives.provisional` + `inferred_from`; a refusals store; wire
  `what_not_to_do` to be READ (currently captured, dropped). *Parallel with 1.1/1.2.*
- **1.4 Generation replacement.** Delete `buildThemeDrafts`, `buildTodoDrafts`,
  `canStandAsTheme`. Replace with Stage 4 (model generation + SEPARATE isolated
  classification call + what_not_to_do filter + choose-one + candidate DoD),
  server-side, no browser round-trip. Kills the theme_index bug by construction.
  *Needs 1.1 + 1.2 + 1.3.*
- **1.5 Assembly.** Deterministic (no model): distilled names, approved DoD,
  self-completing task titles, priority + due dates DERIVED from Now/Next/Later
  sequence, provisional flag computed from `inferred_from`, the cut surfaced
  (quiet line → Knowledge Base). *Needs 1.4.*
- **1.6 Chain test + ACCEPTANCE GATE.** Wire end-to-end; test on the 10 synthetic
  intakes then the 2 real tester intakes; then the go/no-go: new pipeline vs current
  string-concat pipeline, same intakes, Simeon blind-grades. **Nothing ships to
  production without passing this.**
**Attach:** `00-README.md` + the specific stage module(s) for the ticket
(`01`–`06`), `08-integration-map.md`. Repo: `cap-pm-cockpit-alaria` (+ Intake repo
for Stage 1–2 work only). For 1.4 also attach `synthetic_intakes.json`.

### PHASE S — SESSIONS (parallel with everything; feeds Phase 1)
The async manual discovery sessions (protocol v3 + the 5 adjustments in Notion:
pre-register predictions; verbatim corrections before re-filing; action-by-day-7 as
the only success metric; the floor self-test; analyse after 2, don't wait for 5).
- **S.1** Run sessions 1–2 as recruits reply. **S.2** Cross-compare, decide on 3–5.
- **Hard rule: do NOT finalise pipeline Stages 1–2 (the intake questions/schema)
  until at least two sessions are done.** The machinery (Phase 1) proceeds; the
  question set waits for real humans.
- Outputs feed: intake question spec (I-questions), schema-strain fixes, the
  hand-made roadmap as 1.6's benchmark, the proof-for-Mani artifact.
**Attach:** the session protocol doc + Notion. No repo.

### PHASE 2 — FRONTEND BATCHES (resume the queue the engine unblocked)
Per the locked Alpha v2 plan and current merge state:
- **2.1 Batch B remainder: N9 Roadmap tab → N4 Work → N6 Knowledge Base → N7
  Settings.** N9 must REUSE the single existing Now/Next/Later derivation
  (`buildRows`) — which Phase 1.5 replaces with real sequence, so N9 lands after
  1.5 or builds against the new field. N4 is where users edit derived dates. N6
  hosts the refusals/cut record. Standing rule: strip old scaffolding entirely.
- **2.2 Batch C: N5-lite** (Team roster + intros ONLY — no chat, no orchestration;
  `team_threads` unused until Beta).
- **2.3 Batch D: N2 OS Reveal + N8 Global Bar** (last; N2 needs N3; N8 uses the
  *Contextual* variant mockup, NOT `ResolvePM_Global_Bar_dc.html` which is a
  rejected exploration).
**Parallelism:** 2.1 screens are parallel-safe with each other after 1.5 (per the
file-touch-audit rule); Batch C after B; D last. Batch B *layout* work could start
before 1.6 passes, but anything rendering roadmap data should wait for 1.5's real
fields — building against the string-concat data means rebuilding.
**Attach per ticket (STANDING RULE):** master plan (this doc) + that step's brief
(`/mnt/project/NN-*.md`) + ONLY that step's mockup(s) (`ResolvePM_*_dc.html`).
Never attach `ResolvePM_Agent_Chat_dc.html` (pulls Beta chat into N5-lite) or
`ResolvePM_Global_Bar_dc.html` (rejected — use Contextual variant).

### PHASE 3 — ANIMATION PASS (single pass, after screens exist)
Standing decision (7 July): build everything, then ONE animation pass — do not block
the build queue on motion. Every build prompt carries a one-line motion intent so DOM
structure won't fight the eventual animation (the I2-trap mitigation).
- **3.1** Simeon finishes per-page animation mockups (his track; parallel with
  Phases 1–2; the only person-bound serial work).
- **3.2** The pass itself: N3 expand-in-place (parked defect — respec'd by the
  animation brief, don't fix early), page transitions, the deferred minor visual
  bugs, general timing slowdown class of fixes.
- **3.3** Seam pixel residual diagnosis (Intake repo; independent, anytime).
**Blocks/blocked:** 3.1 can start now. 3.2 needs the target screens built (after
Batch B at minimum; ideally after D so it's genuinely one pass). 3.2 is the LAST
code work before acceptance.
**Attach:** the animation mockup file(s) for the specific page + that page's
original brief. Repo: `cap-pm-cockpit-alaria` (3.3: Intake repo).

### PHASE 4 — ALPHA ACCEPTANCE & TESTERS
- **4.1** End-to-end walkthrough on a REAL project (not the fixture — standing
  data-reality rule), mobile-first: messy project → Intake → sign-in → floor →
  themes → confirm → cockpit. Every screen against its mockup.
- **4.2** Re-check N3 Dashboard against real post-pipeline data (flagged in Notion:
  "will need re-checking against real data post-1.5").
- **4.3** Invite the ~5 personal-circle testers; instrument (Clarity is live);
  watch for the product bar: do they understand what matters / what's next / where
  it lives. Session participants are natural first testers.
**Alpha is DONE when:** a stranger's messy project produces a roadmap that passes
the referee, renders in the cockpit, and the tester takes their first action
(day-7 signal — same metric as the sessions).

---

## 3. PARALLELISM MAP (what can run simultaneously)

```
NOW ──────────────────────────────────────────────────────────►
Phase 0.1 parser ──┐
Phase 0.2 intake ──┼─► 1.1 referee ──┐
                   │   1.2 floor  ───┼─► 1.4 generation ─► 1.5 assembly ─► 1.6 GATE
                   │   1.3 schema ───┘                                        │
Phase S sessions ══╪══════════════ (feeds 1.4 question-set + 1.6 benchmark) ══╪══►
Simeon: 3.1 animation mockups ═════════════════════════════════════════════► │
                   │                    2.1 Batch B (data-screens after 1.5) ─┼─►
                   │                                    2.2 Batch C ─► 2.3 D ─┼─►
                                                              3.2 anim pass ──┴─► 4
```
- **Always parallel:** Phase S; 3.1 mockups; 0.1 with 0.2; 1.1/1.2/1.3 with each other.
- **Hard serial:** 1.4 → 1.5 → 1.6; Batch B data-rendering after 1.5; C after B;
  D after C; 3.2 after screens; 4 after 3.2 + 1.6.
- **The two human-bound serial constraints:** Simeon's animation mockups (3.1) and
  Simeon's session availability (S). Everything else is agent-parallelisable.

## 4. STANDING RULES (carry into every fresh chat)
1. Fetch the live Notion page before acting; `/mnt/project/` briefs may be stale.
2. `git fetch origin`; diagnose against `origin/main`, never local (bitten twice).
3. Data-reality check on a REAL project before/after any screen build; fixtures mask.
4. Attachment rule: master plan + that step's brief + only that step's mockups.
5. Strip old scaffolding entirely on Dashboard/Work/Team/KB screens.
6. Branch-first: build on branch → verify on preview vs real data → merge. Mind the
   branch stack; merge in stack order.
7. Every build prompt carries a one-line motion intent.
8. Generation and enforcement never share a call; AND rule; never route on one clause.
9. Dates derived, never generated. Provisionality derived from `inferred_from`,
   never self-reported.
10. Internal vocabulary ("lever", "validate WTP", "definition of done") never
    reaches the user's screen — human words only.
11. Commit early and often; uncommitted work is unrecoverable on container reset.
12. Only show if earned — in reports, in roadmaps, and in generation (the floor).

## 5. FIRST TWO TICKETS (write these next)
- **TICKET 0.1 — Referee parser fix.** cap-pm-cockpit-alaria (or wherever validator
  code lands). Tolerant verdict extraction + separate strict-format-adherence
  logging, or structured output. Acceptance: 0 silently-dropped verdicts on the
  existing eval JSONL replay.
- **TICKET 0.2 — Intake call verification.** ResolveOS-intake-frontend. Read-only.
  Map the actual generation calls (count, prompts, models, output contracts, where
  each project_sessions column is written). Deliverable: corrections to
  `08-integration-map.md`'s [INFERRED] sections. Change nothing.

## 6. REFERENCE FILE INDEX
**Pipeline modules (attach per Phase 1 ticket):** `resolve-pipeline/00-README.md`,
`01-elicit.md`, `02-structure.md`, `03-floor-gate.md` (Gate 2 wording superseded —
use the corrected discriminator in this doc/Notion), `04-extract-themes.md`,
`05-referee.md`, `06-assemble.md`, `07-input-safety.md` (Beta-gated),
`08-integration-map.md`.
**Test data:** `synthetic_intakes.json`, `floor-intakes-with-outcomes.json`.
**Frontend briefs/mockups:** `/mnt/project/00-MASTER-PLAN.md` (superseded by this
doc for sequencing; still holds ticket detail), `NN-*.md` briefs,
`ResolvePM_*_dc.html` mockups (per-ticket only).
**Session protocol:** Async Manual Discovery Session Protocol v3 (+ Notion's 5
adjustments).
**Notion:** Active Priorities `390d8b53b7a881dda5e4daedbc76d54c` (live source of
truth); Roles epic `393d8b53b7a8810ea8d1df77834670b6`.
