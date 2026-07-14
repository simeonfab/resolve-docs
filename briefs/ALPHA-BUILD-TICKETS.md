# RESOLVE ALPHA — BUILD TICKET PACK

**This document + the attached mockups is everything you need. Nothing else will be
attached. Do not ask for files. Do not reconstruct anything from memory.**

**The outcome this pack delivers:** Simeon runs intake → ResolvePM end to end on a
phone-width browser and sees **exactly** the designed product, with every button working and
no critical backend faults. Every element listed in these tickets must be clickable and do
what it says.

---

# PART 0 — COORDINATES

| | |
|---|---|
| **ResolvePM repo** | `simeonfab/cap-pm-cockpit-alaria` — Next.js 14 / Supabase / Vercel |
| **Intake repo** | `simeonfab/ResolveOS-intake-frontend` — static site. Prod deploy `4e6c16e`. ⚠️ Its Vercel project is named **`resolve-intake-test-a`** — that is CORRECT despite the name. |
| **Docs repo** | `simeonfab/resolve-docs` |
| **Framework repo** | `simeonfab/ResolveOS` — role definitions |
| **Supabase** | project `urmwilgdevgkdnntojnl` ("CAP PM Dashboard") |
| **Dev account** | `simeonfab2@gmail.com` |
| **Live test project** | `af8abaa8-e008-4baa-82a1-27c87671bdd6` (YGOBrain) · session `cee60b5e-017f-4668-b95f-3e573ca5d53e` — the ONLY project produced by the real pipeline. Use it for data-reality checks. |
| **cap-pm `main`** | **`2de51b2` — Phase 0 DONE.** Was `3d6b560`. Now carries the rebuilt pipeline, Gate-1-only halt, referee wired, refusal logging, project-scoped queries, and the six deleted routes. |
| **Pipeline branch** | `claude/referee-integration-drafts-uuadkz` @ `b7b98ce` — ⚠️ **STALE SHA. DO NOT BRANCH OFF IT.** This work IS cap-pm `main` (`2de51b2`) now, merged in T0.1. |
| **Query-scope branch** | `claude/route-audit-design-specs-cytvw6` @ `85812f2` — ⚠️ **STALE SHA. DO NOT BRANCH OFF IT.** This work IS cap-pm `main` (`2de51b2`) now, merged in T0.1. |
| **N9 Roadmap tab** | `claude/roadmap-tab-step-3-cwrt6p` @ `d746473` — ⚠️ **NEVER MERGE THIS BRANCH.** It is ~30 commits stale; its diff DELETES the pipeline, floor, referee, global bar and team work. **Cherry-pick its 2 page files only.** |
| **Docs filing branch** | `claude/resolve-docs-filing-updates-d114cl` @ `e350b16` — ✅ **MERGED to resolve-docs `main` (`760eaf4`)** in T0.2. |

**Key tables:** `projects` · `project_sessions` (`outcome`, `outcome_confirmed`,
`project_understanding`, `recommended_action`, `top_priorities`, `what_not_to_do`,
`gap_answers`) · `initiatives` (`name`, `summary`, `priority`, `definition_of_done`,
`inferred_from`, `provisional`, `project_id`, `user_id`) · `work_items` · `refusals` ·
`knowledge_base_items`.

---

# PART 1 — NON-NEGOTIABLES

**Violating any of these fails the ticket, however good the code is.**

**N1 · Never bundle a judgement with a generation in one call.** It fires 0/50. Generation
and enforcement NEVER share a call. This has been re-learned three separate times (the floor,
the stand-up router, the orchestrator router). Every new model call is judged against this.

**N2 · Never ask the founder to supply the direction they came for.** The founder came to us
for direction; asking them "what should you work on?" hands our job back. If we know the
**outcome** and the **current state**, deriving the path between them **IS the product**.
- **Gate 1 (no outcome) is the ONLY legitimate halt in the entire app.** No outcome = nothing
  to aim at.
- **Gate 2 (no lever) NEVER halts.** It marks themes `provisional` and logs to `refusals`.
- If you find any other code path that halts the user and asks a question, **that is a bug.**
- An escalation question ("what is the ongoing lever here?") was shipped once. It is banned.

**N3 · Built ≠ wired ≠ shipped.** A feature is not done until a user can REACH it and PRESS
it. An unreferenced component is dead code. Three real incidents: the floor had zero callers;
the halt screen rendered nowhere; the team chat merged with no link to it.

**N4 · Verification means an ARTIFACT, not a claim.** Screenshots of the actual interaction,
query results, SHAs. **A passing build is NOT verification.** Never end a ticket by telling
Simeon to go check it himself. **The builder never certifies its own work.**

**N5 · Commit and push after every working step.** Work lost twice to container resets.
A local-only commit is not durable.

**N6 · Every repo-state claim is a hypothesis.** Fetch first. `git log --all` before believing
anything is built. This week produced five confidently-asserted false claims about repo state.

**N7 · Dates are never model-generated** (derived from Now/Next/Later sequence).
**Provisionality is never self-reported** (computed from `inferred_from`).

**N8 · Do NOT touch the generation pipeline machinery** (`roadmap-generation.ts`,
`referee.ts`, `floor.ts`) except where a ticket explicitly says so. It is correct and proven
(44 refusal rows, honest provisional flags, traceable `inferred_from`).

---

# PART 2 — HOW EVERY TICKET IS BUILT AND ACCEPTED

**Mobile-first, 390px.** Desktop is OUT of Alpha scope for the app. The intake flow (pre-auth)
is desktop-width (~1180px). Do not build desktop app layouts.

**The mockup is the truth for pixels. This document is the truth for behaviour.** Where they
disagree, this document wins and you FLAG it.

**Brand system is LOCKED. Do not introduce new styles.** DM Serif Display for exactly ONE
headline per screen; Inter everywhere else; 10px/6px radii; sentence-case labels. Light
surface is the base; **Dark Ink is reserved for one dominant moment per screen** (win card,
do-first card, welcome veil) — never a dark theme throughout.

**Global motion tokens** (WAAPI `element.animate`, `fill:'both'`; staggers apply
`delay + (index × stagger)` in DOM order):
- `--e-spring` `cubic-bezier(.34,1.56,.64,1)` — arrivals, expansions, sheets
- `--e-out` `cubic-bezier(.22,.61,.36,1)` — decelerate reveals, restore
- `--e-std` `cubic-bezier(.4,0,.2,1)` — dims, press, tab colour
- `--e-in` `cubic-bezier(.55,0,.85,.35)` — exits, route-out
- amber focus: border `#C8A040` + `box-shadow:0 0 0 3px rgba(200,160,64,.22)`

**Loaders narrate.** Every processing wait = amber spinner ring + a checklist that ticks off
named stages. Never a bare spinner. Loaders run ONCE in product (the mockups loop for preview).

**Dev controls in the mockups ("↻ Replay") are MOCKUP-ONLY. Never build them.**

**⚠️ THEME COUNT IS 1–3, NEVER HARDCODED.** The pipeline returns however many themes are
EARNED — scarcity is the point. A real run produced TWO. Every progress dot, grid column,
counter and copy string derives from the actual count. The mockups show 3 as demo content;
"three" is never a contract. A screen that says "Three themes" when two exist is a bug.

## ACCEPTANCE — every ticket, no exceptions

A ticket is done when **every element row in it** has been:
1. **Clicked in a real browser**, at 390px, against **real data** (project `af8abaa8`).
2. Observed to do exactly what the row says.
3. Captured as an **artifact** (screenshot / query result / SHA).

Plus: every state (loading / empty / error / settled) has been **seen**, not assumed. And
**the builder is not the verifier** — a different provider or Simeon signs off.

**An unclicked button is an unbuilt button.**

---

# PHASE 0 — CONSOLIDATE TRUTH  *(do this first; everything stands on it)*

> ## ✅ PHASE 0 IS COMPLETE — 14 July 2026
> All four tickets below are DONE and landed on `main` (fast-forward, no force).
> - **cap-pm `main` = `2de51b2`** (was `3d6b560`) — T0.1 + T0.4.
> - **resolve-docs `main` = `760eaf4`** (was `7ee0804`) — T0.2 + T0.3.
>
> ⚠️ **The branch SHAs `b7b98ce` and `85812f2` are now STALE — that work IS cap-pm
> `main` (`2de51b2`). Do NOT branch off those dead SHAs; branch off `main`.**
>
> - **T0.1 — COMPLETE** (cap-pm `main` `2de51b2`). Pipeline + query-scope consolidated as a
>   clean fast-forward (`route-audit` was already stacked on `referee`, so no conflicts).
>   Verified: `buildThemeDrafts` / `buildTodoDrafts` / `canStandAsTheme` deleted;
>   `evaluateFloorSufficiency` and `runRefereeChecks` each have real callers.
> - **T0.2 — COMPLETE** (resolve-docs `main` `760eaf4`). Docs filing branch `e350b16` merged.
> - **T0.3 — COMPLETE** (resolve-docs `main` `760eaf4`). Canonical pipeline specs (00–08)
>   rewritten so Gate 1 is the only halt and Gate 2 degrades to provisional + `refusals`.
>   Files: `00-README.md`, `03-floor-gate.md`, `05-referee.md`, `08-integration-map.md`.
> - **T0.4 — COMPLETE** (cap-pm `main` `2de51b2`). Six out-of-spec routes deleted, dangling
>   links cleaned, `tsc --noEmit` green.
>
> The ticket text is kept below as the record of what was done. **Phase 1 is the next work.**

## T0.1 · Merge the two proven branches to `main`  — ✅ DONE (cap-pm `main` `2de51b2`)
Merge `claude/referee-integration-drafts-uuadkz` (`b7b98ce`), then
`claude/route-audit-design-specs-cytvw6` (`85812f2`). Resolve conflicts in favour of the
LATER branch for query scoping and the pipeline branch for generation.
**Verify after:** `git grep buildThemeDrafts buildTodoDrafts canStandAsTheme` on main returns
NOTHING (they must be deleted). `evaluateFloorSufficiency` and `runRefereeChecks` each have a
real caller. Report the merge SHA.

## T0.2 · Merge the docs filing branch  — ✅ DONE (resolve-docs `main` `760eaf4`)
`claude/resolve-docs-filing-updates-d114cl` (`e350b16`) into `resolve-docs` main.

## T0.3 · Kill the spec that resurrects the banned halt screen ⚠️ HIGH VALUE  — ✅ DONE (resolve-docs `main` `760eaf4`)
`resolve-docs/resolve-pipeline/` (00–08) **still says "escalate with a question instead of
shipping a bad plan."** A previous agent read that, built a halt screen asking the founder
for their "lever", and shipped it. **The spec caused the bug.** Only cap-pm's convenience
copies were fixed.
Grep the canonical specs for every instance of escalate-with-a-question / halt /
ask-the-founder-for-a-lever. Rewrite each to N2: never halt, never ask the founder to supply
the direction; generate, mark provisional, log to `refusals`, show reasoning, invite
correction. **Gate 1 is the only halt.** Report every file changed.

## T0.4 · Delete the out-of-spec routes  — ✅ DONE (cap-pm `main` `2de51b2`)
Delete: `/feedback` · `/updates` · `/roadmap/presentation` · `/settings/database-health` ·
`/work-tracker` (redirect stub) · `/auth/callback` (confirm no external auth provider uses it
first — OTP sign-in has no callback screen).
**KEEP** the "Reset onboarding flow" control in **Settings** — it is a real spec'd feature
(T2.11). Delete only the copy that sits on the legacy roadmap page (which dies in T2.7).

---

# PHASE 1 — BACKEND SPINE  *(the buttons need something to do)*

## T1.1 · TASK GENERATION — CRITICAL, BLOCKS THREE SCREENS
**The bug:** the rebuilt pipeline generates only THEMES. `taskPool` is populated ONLY as an
accident of misclassification (`roadmap-generation.ts:544`) and is **wiped entirely on the
degrade path** (`:616`). So `todos = []` and the (correct) insert loop in
`roadmap-onboarding.ts:560-593` never runs.
**Evidence:** project `af8abaa8` has 2 initiatives and **ZERO `work_items`**. Every older
project has 10–11.
**A roadmap with no work is not a product.** The To-do reveal, the Work tab and the Dashboard
win all read from `work_items`.

**Build:** a real task-generation stage. Generate the first-week task pool from the confirmed
themes (weight the Now theme). **Isolated calls — generation, classification and judgement
NEVER share a call (N1).** Assemble deterministically into `work_items` with `initiative_id`
AND `project_id`. Respect `what_not_to_do` as a hard constraint (blocked work goes to Later,
never Now). Do not let a degrade path silently wipe the pool.
**Verify:** run it on `af8abaa8` and query `work_items` — real rows, correct `project_id`,
correct `initiative_id`. Paste the rows.

## T1.2 · PERSIST THE DRAFT + AWAIT THE WRITE
Two bugs, one fix.
- **Non-determinism:** generation currently runs on EVERY pre-confirmation `/roadmap` load —
  several sequential LLM calls, a different roadmap each reload, and `refusals` row spam.
- **Race:** the confirm screen calls `void persistRoadmap()` and enables Continue **without
  awaiting it**. Navigate away mid-flight → initiatives insert, work items don't.

**The spec requires stored state:** *"the Dashboard win and the confirmed Roadmap are stored,
stable state — not recomputed on load. Never show a refresh affordance."*
**Build:** generate once, persist the draft, read it on reload. Confirmation `await`s the
write and only then enables Continue.
**Verify:** reload `/roadmap` 3× — identical roadmap each time, no new `refusals` rows.

## T1.3 · INTAKE — THE OUTCOME ANSWER IS UNVALIDATED (highest leverage in the pack)
**Repo: `ResolveOS-intake-frontend`.**
**What happened, for real:** Call 1 asked *"If YGOBrain works, how will Yu-Gi-Oh! players be
better off, **and how would you know?**"* The founder answered *"they would be able to make
better decks easier and keep track of the meta."* That gives the **beneficiary** and skips the
**observable change** completely. **Nothing required the second half.** It was written to
`project_sessions.outcome` with **`outcome_confirmed = TRUE`** — so downstream it looks MORE
trustworthy than an inferred outcome while missing the exact part that makes an outcome usable.
Every theme that run came back provisional. **This soft outcome is the root cause of a weak
roadmap.** Doctrine: *Gate 2 firing means our INTAKE QUESTIONS are wrong — fix upstream.*

**Build:**
- Validate the **content** of the answer, not its presence. A non-empty string is not an
  outcome. It must contain a **beneficiary** AND an **observable or verifiable change**.
- Validation is a **SEPARATE, ISOLATED model call** (N1). Never bundled with extraction.
- On a half-answer: **re-ask ONCE**, naming what is missing, in plain language. No lecturing,
  no loops.
- Still partial after the re-ask: **accept it, write it, but set `outcome_confirmed = FALSE`.**
  An unverifiable outcome must never masquerade as a confirmed one — downstream provisionality
  depends on that flag being honest.
- **Do NOT reword the question.** The question is fine. The CAPTURE is unenforced.

## T1.4 · INTAKE — FAIL-SILENT HANDOFF
`buildResolvePmHandoffPayload` (`index.html:~1787-88`):
`top_priorities: recommendationData?.topPriorities || []`.
`validateRecommendationShape` DOES require both fields (`resolve.js:215`, `:216-228`) — so the
`|| []` fires **only when the entire `recommendationData` object is missing**, i.e. total
failure. **A total failure is handed off as an empty-but-valid-looking payload.**
**Build:** a missing `recommendationData` is an explicit, visible failure. Never hand off a
silently-empty payload. Find and fix every other silent fallback in the handoff path.

## T1.5 · THE PROJECT REPORT PDF
§6 (Intake Transition) has a **"Download report"** button, and §16 (Knowledge) has an intake
report row. **Audit first:** does a generator exist behind `project_sessions.project_plan_report`?
If not, build it — it is the founder's tangible take-away. Report what you find before building.

## T1.6 · UNIFY THE DEMO DATASET
The mockups mix TWO fictional projects: a rebrand ("Nook & Thread", the roadmap themes,
Settings project, intake starter chips) and a generic SaaS startup ("Pick the auth provider",
"Landing copy from Priya", "Ship pricing copy" on Dashboard / OS Reveal / Shell / Global Bar).
**Pick ONE story — Resolve's own project — so every screen tells the same one.** Applies to
intake (§1–6), roadmap cards (§8), reveal (§10), Dashboard (§12), roadmap/work tabs (§13–14),
Settings (§17).
**⚠️ No binary finish-line themes.** "Finalise brand name" would be REJECTED by our own
referee (it has a finish line) — it teaches the wrong concept on every screen. Use levers, e.g.
"Creating your path". Content only; no logic changes.

---

# PHASE 2 — THE SCREENS
### In journey order. Each ticket lists EXACTLY what is in the mockup.

---

## T2.1 · §1–2 INTAKE: NAME ENTRY + WELCOME/PROBLEM REVEAL
**Mockup:** `ResolvePM_Onboarding.dc.html` · desktop ~1180px, pre-auth
**Chrome:** 4-step left stepper · dark topbar with amber progress bar (25/50/75/88/100%) +
"Reset" pill (present on EVERY intake screen; restarts intake from the top).

**§1 Name entry.** Progress 25% · "Step 1 of 4". Eyebrow "Let's get started"; serif hero
"What's your name?".

| Element | Type | Behaviour |
|---|---|---|
| Name field | text input, **autofocus** | **Enter key submits** (equals Continue) |
| Continue | ink button + arrow | **disabled until a name is entered**; advances to §2 |
| Reset | topbar pill | restarts intake from the top |

**§2 Welcome → problem (fused reveal).** Step counter HIDDEN here (the sidebar stepper carries
progress). The question is **Inter** — the one serif element is the "Hi [name]." hero.

| Element | Type | Behaviour |
|---|---|---|
| "I have a messy project" | path option, **live, pre-selected** | advances to §3 |
| "I have a new idea" | path option | **disabled — "Coming soon"** |
| "I'm stuck, not sure where to start" | path option | **disabled — "Coming soon"** |

**Motion — fused reveal** (7000ms loop in the mockup; **plays ONCE in product**):
- `rHello` 0→9% — "Hi Simeon." hero fades up `translateY(14px)→0`, then **holds** the whole time
- `rWelcome` 6→31% — "Welcome to Resolve" fades in under the hero, holds briefly, fades out
- `rPrompt` 28→38% — "So, / What do you need help with?" fades up `translateY(10px)→0`
- `rO1/2/3` 40/46/52% — the three path options each fade up `translateY(12px)→0` in turn
- `rSide` 30→42% — sidebar stepper fades in as the question appears

---

## T2.2 · §3 INTAKE: BRING IT IN
**Mockup:** `ResolvePM_Bring_It_In.dc.html` · Progress 50%
The large textarea is the hero. Helper hierarchy: "Try an example" (primary) · paste-prompt
(link) · "Add material" (disabled).

| Element | Type | Behaviour |
|---|---|---|
| Context textarea | text input, **hero** | holds the dump; **its content gates "Build my path"** |
| Mic button | icon, bottom-right of textarea | **DISABLED for Alpha** (Simeon's call). Render it disabled, like "Add material". |
| Starter chips ×3 | e.g. "A rebrand that's stalled" | **prefill a scaffold into the textarea** |
| Try an example | primary helper button | **fills the textarea with a worked example** |
| Paste from another AI chat | link | prompt to pull in an existing conversation |
| Add material | dashed button | **disabled — "Coming soon"** |
| Back | button | returns to §2 |
| Build my path | ink button + arrow | **disabled until content** ("Add a little context to continue"); triggers the reading loader → §4 |

---

## T2.3 · §4 INTAKE: READING LOADER → CONFIRM UNDERSTANDING
**Mockup:** `ResolvePM_Confirm_Understanding.dc.html` · Progress 50→75%
Show what Resolve extracted and let the user correct it **before it drives everything**.

| Element | Type | Behaviour |
|---|---|---|
| Understanding card ×4 | editable cards | per-card **pencil opens inline edit**. The four: 📁 Project · 🎯 What you want · 📍 Where it is · ❓ Biggest uncertainty |
| Gap question inputs ×2 | text | "A couple of things Resolve still needs" — **the outcome question is REQUIRED (see T1.3); the rest are SKIPPABLE** |
| "Not quite?" | inline challenge link | opens inline correction on the amber **"What Resolve is assuming"** card |
| Back | button | returns to §3 |
| "This looks right — build my path" | ink button | triggers the weighing loader → §5 |

**Motion — reading loader** (5000ms loop; **runs once in product**):
- Spinner ring: `spin` 900ms linear ∞ — amber-topped ring rotates 360°
- Checklist ×4 sequential at **17 / 37 / 57 / 79%** — each row: label greys→ink, circle fills
  amber (border+bg), check fades in. **Stages: reading → separating decided/open → spotting
  gaps → shaping.** On completion the confirm screen replaces the loader.

---

## T2.4 · §5 INTAKE: WEIGHING LOADER → YOUR NEXT MOVE
**Mockup:** `ResolvePM_Next_Move.dc.html` · Progress 88→100%
The single highest-leverage action + the shape of what follows. **The dark "Do this first"
card holds the one serif element; the page title is Inter.**

| Element | Type | Behaviour |
|---|---|---|
| "Do this first" card | **dark hero**, display | the single recommended action + first-milestone card beneath it |
| Timeline "This week" | flagged card, amber border | marked active; "Next" and "Later" cards follow (display only) |
| Back | button | returns to §4 |
| Continue | ink button | leads to the celebration handoff (§6) |

**Motion — weighing loader** (identical mechanic to §4): spinner 900ms; checklist steps at
17/37/57/79%. **Stages: weighing what matters → checking against confirmed → sequencing the
work → choosing your first move.**

---

## T2.5 · §6 INTAKE TRANSITION + THE BRIDGE  *(new motion, high craft)*
**Mockup:** `ResolvePM_Intake_Transition.dc.html`
Turn "leaving intake" into an earned reward, then carry the moment into sign-up **without a
hard cut**. **The screen ENDS at the handoff card — nothing renders below it.** Copy is
benefit-led, never "send session data".

| Element | Type | Behaviour |
|---|---|---|
| Download report | secondary button (dark), **sits directly above the CTA** | downloads the full project report PDF (see T1.5) |
| Continue in Resolve | primary amber CTA | plays the ~9.2s bridge → settles into §7 |

⚠️ The "Not now" link and the 3 carry-pills were **REMOVED** (Flow Spec A3). Do not build them.

**Motion — bridge transition** (9200ms, 4 held beats; **auto, no click**; Ink+Amber throughout):
- **Shared glow** `glowpulse` 9.2s — amber radial pulses opacity .35↔.85 on the shared clock
- **Beat 1 · trophy zoom** 0.0–2.3s `fog` — the trophy card grows `scale(.62→1.16)` in **ONE
  continuous zoom**. ⚠️ **8→25% is a SINGLE easing segment — do NOT subdivide it; subdividing
  caused judder.** Caption: "You just cleared the fog."
- **Beat 2 · first move pins** 2.4–4.5s `pin` — amber-check chip carrying **the user's REAL
  next task (bind live, NEVER hardcode)** glides in `translateY(22px)→0`. Caption: "Bringing
  your first move with you."
- **Beat 3 · workspace forms** 4.6–6.6s `brand` — Resolve mark (44px tile + wordmark) + 3
  context chips fan in. Caption: "Setting up your workspace."
- **Beat 4 · sign-in settles** 6.8s → holds `settle` — renders the canonical sign-in minimal
  state, **pixel-identical to §7's first frame**. **No eyebrow / subhead / footer here.**

---

## T2.6 · §7 SIGN-UP / LOGIN (OTP) + EXIT TRANSITION
**Mockup:** `ResolvePM_Sign-in.dc.html`
Create the workspace via **one-time code — NEVER a magic link. All copy says "code".**
Dark shell, amber glow, serif headline.
⚠️ **SEAM CONTRACT: §7's initial frame === bridge beat 4.** Eyebrow/subhead/footer sit in the
DOM at `opacity:0` so **layout never jumps**; they fade in only on the landed screen.

| Element | Type | Behaviour |
|---|---|---|
| Email field | text input | gates "Send code" |
| Send code | amber button | reveals the 6-digit code entry state |
| Code boxes ×6 | digit inputs | states: **filled / active (amber ring) / err (terracotta)**. Wrong or expired → **inline error, NO reload** |
| Change email | back link | returns to email entry |
| Verify and continue | amber button | enters the app — **the Gate 1 check runs HERE**: passes → build sequence (§7.5/§8); fails → Floor refusal |
| Resend code | link, cooldown | **disabled with a countdown** ("Resend available in 0:42"); becomes an active amber link after, and on the error state |
| "Already have a workspace? Sign in" | footer link | existing-workspace login path |

**Motion — landed fade-in + exit to roadmap** (6500ms):
- Eyebrow/subhead/footer, **on land only** (`.landed`): `fadein` 520ms, staggered
  **420/560/700ms**, fade up `translateY(6px)→0`. ⚠️ **Bridge beat 4 must NOT render these.**
- **Exit D1 · verified** 0.0–2.2s `xdark` — dark layer holds: 56px amber check, "You're in.",
  "Setting up your workspace…"
- **Exit D2 · amber bloom + wash** 2.2–3.6s — amber bloom ripples from centre `scale(.4→11)`;
  light surface fades up beneath the **anchored brandmark**; dark layer fades out
- **Exit D3 · roadmap lands** 3.6s → holds `xcard` — the first theme card scales in with the
  Roadmap Cards spring `cubic-bezier(.34,1.56,.64,1)` — **reuse verbatim**.
  ⚠️ **The brandmark position is IDENTICAL across dark/light — that is the through-line.**

---

## T2.7 · §7.5 GATE 1 HALT — THE ONE DELIBERATE STOP
**Mockup:** `ResolvePM_Floor_Refusal.dc.html`
Sits in the R2 light shell. 3-segment progress (done · done · on). **Fires rarely.**
**Register: honest, not sorry — never an error, never a form. The question ELICITS, it does
not advise.** **No escape hatch** (see N2).
Eyebrow/headline/consequence are **static**. **Para-1 reason and the one question are DYNAMIC
from the floor service and must flex ≈8→30 words.** Prop: `accent` (default `#C8A040`).

⚠️ **The global bar must NOT render during the build sequence.** It currently does — this is a
bug visible in a real screenshot. The build sequence (§7.5–§11) is **chrome-free**.

| State / Element | Behaviour |
|---|---|
| Question card | dynamic hero — shows the ONE generated question; the card grows and Para 1 reflows (must handle short AND long payloads) |
| Answer textarea | Default (empty, button off) → Filled (focused, button on) |
| Continue | off until content; on submit → Submitting |
| Submitting | "Re-running the check" + spinner; **input locks; nothing implies the outcome** |
| Passed | **"That's the piece we needed."** — brief amber-check + 3-dot loader, then hands straight to Roadmap Reveal. **No confetti. No "you passed".** |
| (no skip control) | **deliberate omission — the only path out is answering** |

**Motion:** submit spinner `sp` 0.7s linear ∞ · passed 3-dot loader `pl` 1.1s ∞ stagger .18s
(three amber dots pulse opacity .3↔1 in sequence while the roadmap builds).

---

## T2.8 · §8–10 THE BUILD SEQUENCE (mobile, chrome-free)
**Mockups:** `ResolvePM_Roadmap_Cards.dc.html` · `ResolvePM_Todo_List.dc.html` ·
`ResolvePM_Roadmap_Reveal.dc.html`
A full-screen sequence with **no app chrome** — the user "signing" their roadmap before the OS
appears. A **3-segment progress bar** threads it: Themes → To-do → Roadmap.
**These three screens are already built (R2/R3/R4) — this is a contract pass, not a rebuild.**

### §8 Roadmap theme cards
Confirm the strategic themes **one at a time**. Progress dots done/current.
**No reject/remove path in Alpha — correction only.** Stateful (index, approved[], correcting).
⚠️ **1–3 themes, derived from the data. Never hardcode 3.**

| Element | Type | Behaviour |
|---|---|---|
| "Yes, add to my roadmap" | amber button | pushes the theme into "On your roadmap" (check-pop), advances index |
| "Not quite" | outline button | opens the inline correction field (**auto-focus**) |
| "Save and continue" | amber inline button | swaps this theme's wording to the typed version, then approves + advances. **Re-runs the theme-vs-task check SERVER-SIDE — this must hit the REFEREE, not a keyword heuristic.** |
| Continue | end card, after the LAST theme | hands to §9 |
| ↻ Replay | **mockup only — DO NOT BUILD** | |

**Motion:** card arrival `cardIn` .45s spring — `scale(.94) translateY(10px)→none`; a faint
"behind" card offsets to show more remain; inner tag/headline/buttons fade up staggered
(.05/.16/.3s). Approved row drop-in: row slides `translateX(-14px)→0` (.45s), its amber check
pops `scale(1.6→1)` (.5s spring).

### §9 To-do list reveal
The concrete first-week list built from the roadmap. Progress: seg1 done, seg2 on.
Counter "2 of 12 complete" (**derive from real data — depends on T1.1**).
Headline: **"Here's where to begin."** (approved).

| Element | Behaviour |
|---|---|
| "See it as a roadmap" | ink button → §10 |
| ↻ Replay | **mockup only — DO NOT BUILD** |

**Motion — auto-plays on mount:** meta-steps ×2 sequential (`rowIn` .35s → check pop .5s →
strike @ .3s/1.5s): "Set up workspace" then "Confirm your roadmap" — row fades up, amber check
pops, text strikes through and greys. "Up next" **dark row** @2.7s (`rowIn` .4s) — the first
real task surfaces as the dark card. Upcoming rows ×2 + footer @3.25/3.5/3.9s — fade up
`translateY(6px)→0`; **the Continue button lands last.**

### §10 Roadmap reveal
The plan as a **theme × Now/Next/Later grid**. Progress: seg1+2 done, seg3 on.
**The provisional theme carries the marker here (treatment A — the line).**

| Element | Behaviour |
|---|---|
| Provisional marker + tooltip | on the inferred theme label. Hover: "Provisional — inferred by Resolve, not stated…" Driven by the theme's `provisional` boolean from the roadmap service. |
| Product Director input + Send | ask about the roadmap or add something before entering the app. ⚠️ **Wire to the EXISTING N8 orchestrator "ask" intent — do NOT build a second ask path.** |
| "Open my roadmap" | ink button → OS assembly (§11) → Dashboard |
| ↻ Replay | **mockup only — DO NOT BUILD** |

**Motion — column-by-column reveal** (`rvA` .5s ease each, staggered): theme labels first
(`rd1/2/3` @ .2/.35/.5s) fade up `translateY(8px)→0`. Then **Now → Next → Later columns**
(`rd4/5/6` @ 1.4/2.0/2.6s) — column headers + cells fade in per column; **Now is
amber-emphasised.** Product Director card last (`rd7` @ 3.3s) drops in.

---

## T2.9 · §11–12 OS ASSEMBLY + DASHBOARD  *(N2/N3 built — contract pass)*
**Mockups:** `ResolvePM_OS_Reveal.dc.html` · `ResolvePM_Dashboard.dc.html`

### §11 OS assembly → Dashboard
The grand ONE-TIME reveal of the whole OS. Two versions: **first-run grand** and **calm
every-open**. Both auto-play and land on the Dashboard. **No user controls.**
- **First run (grand):** welcome veil 0–1.5s — logo pops `scale(.8→1)`, "Welcome in, Simeon /
  Your operating system is ready" fades up, then the dark veil fades out. Header 1.7s drops in
  `translateY(-8px)→0` (.5s). Tab bar 2.0s slides up `translateY(110%)→0` (.55s). **4 cards +
  global bar 2.4–3.8s** (`rvUp` .5s each) — **win → blocked → today → pointer populate in
  leverage order**, then the global bar rises.
- **Every-open (subtle):** no veil; header + 4 cards + bar settle in a quick calm fade
  (same `rvUp`, tighter delays, .05–.5s).

### §12 Dashboard
Answers "what do I do next?" **Four cards, fixed order.**
⚠️ **Win content is STORED/STABLE — not recomputed on load** (depends on T1.2).
Props: `winHeadline`, `winSupport`, `winRouteLabel` (enum: opens work item / agent chat /
roadmap), `blockedNote`.

| Element | Type | Behaviour |
|---|---|---|
| **Win for today** | **dark card — the WHOLE CARD is the tap target, no button** | routes contextually by `winRouteLabel`: work item → opens it · agent-relevant → that role's chat · roadmap → the tab |
| Waiting on (blocked) | card | **expands IN PLACE** → detail: stalled-since, waiting-on, note, + **Send reminder** / **View in Work** |
| Today (schedule) | card | expands in place; meetings + to-dos with times |
| Worth a look (pointer) | card | routes to the Roadmap tab |
| Avatar | header, top-right | opens the profile menu (Profile · Settings · Sign out) |
| Global bar | floating pill | opens the orchestrator chat sheet (E4) |

**Motion — Flow Spec Surface E (literal):**
- **E1 entrance** (≈940ms): topbar 340ms out `translateY(-8px)` → card stack 460ms spring
  delay 120ms **stagger 70ms** `translateY(18px) scale(.98)` → global bar 480ms spring delay
  460ms `translateY(30px)`. **Tab bar static.**
- **E2 blocked expand** (tap "Waiting on"): backdrop dims 220ms; **other cards blur + dim to
  .5**; the card swaps IN PLACE for the detail panel `translateY(10px) scale(.96)→none` (420ms
  spring); detail rows fade up stagger 55ms.
- **E3 collapse** (tap backdrop): reverse of E2, ≈300ms, `--e-in`.
- **E5 win press + route-out:** card presses `scale→.975→1` (240ms); screen exits left
  `translateX(-28px)` (280ms in). **Idle loop: the route arrow drifts +3px on a 2.6s
  ease-in-out ∞ — always running.**
- **E6 pointer route-out:** press `scale .99` + bg tint to `#FBF4EA` (260ms); screen exits left.
- **E7 profile menu:** avatar presses + amber focus ring; backdrop dims; menu unfolds from
  `transform-origin: top right`, `scale(.9) translateY(-6px)→none` (300ms spring); rows
  stagger 40ms.
- **E8 switch tab:** directional — outgoing exits `translateX(-24px)` (240ms in), incoming
  enters `translateX(28px)→none` (400ms spring); **active tab colour greys→amber mid-transition
  (300ms)**. Flip the sign for backward nav.

---

## T2.10 · §13 ROADMAP TAB — KILL THE LEGACY PAGE
**Mockup:** `ResolvePM_Roadmap_Tab.dc.html` · motion: Flow Spec F
⚠️ **What a real user sees TODAY at `/roadmap` is a LEGACY page** — Quarter/Status/RAG
filters, "grouped by delivery quarter", raw CRUD, and a "Reset onboarding" testing tool. **It
is in no spec. It dies in this ticket.**
⚠️ **The designed page EXISTS on branch `claude/roadmap-tab-step-3-cwrt6p` (`d746473`) and
matches the mockup near-pixel. NEVER MERGE THAT BRANCH — it is ~30 commits stale and its diff
DELETES the pipeline, floor, referee, global bar and team. CHERRY-PICK ITS 2 PAGE FILES ONLY**,
rebuilt against current main. It reads `listInitiatives`, which is now project-scoped.

**The living plan. Stateful view toggle.**

| Element | Type | Behaviour |
|---|---|---|
| **By phase ↔ By theme** | segmented toggle | **By phase** = Now/Next/Later stacked (Now amber). **By theme** = per-theme cards, each with Now/Next/Later rows |
| Provisional marker + tooltip | **by-theme card HEADER ONLY** | **NOT repeated per action.** Hover: "Provisional — inferred by Resolve, not stated in your intake" |
| Action card | Now/Next/Later row | **(F3) expands INLINE** → context line + **"Go to work item"**. **One open at a time.** |
| Global bar | floating pill | "Ask about your roadmap" → orchestrator sheet (routes roadmap intents to the Product Director) |
| Bottom tabs / avatar | nav | switch tab (E8) / profile menu |

**DECISIONS (locked):** **Remove the Product Director FAB** — the global bar is the single
entry point for roadmap questions. **F3 inline action-expand is IN** — it carries "Go to work
item", the only link from the plan to execution. **F5 "whole picture" matrix overlay is OUT
for Alpha (Beta).**

**Motion — Flow Spec Surface F:**
- **F1 entrance:** header 340ms out → title 420ms out delay 80ms → toggle 360ms spring →
  phase groups ×3 460ms spring delay 260ms stagger 80ms → global bar 480ms spring delay 620ms.
- **F2 view toggle:** thumb slides `translateX(0↔100%)` (320ms spring); **directional** —
  outgoing view exits ∓18px (200ms in), incoming enters ±24px→none (380ms spring) **after the
  swap at midpoint**.
- **F3/F4 expand/collapse:** press `scale .99`; detail animates **measured height
  `0→scrollHeight`** (360ms spring); inner rows stagger 50ms; collapse reverses (240ms in) and
  **removes the node**.
- **F5/F6:** deferred to Beta — **do not build.**

---

## T2.11 · §14 WORK TAB — BUILD THE INTERACTIONS
**Mockup:** `ResolvePM_Work_Tab.dc.html` · motion: Flow Spec G
The **execution** surface (distinct from Roadmap's **plan**).
Week-progress bar (e.g. 17% · "2 of 12 done" — **derive from real data**).
⚠️ **"Pre-populated from confirmed roadmap — NEVER BLANK."** This depends entirely on **T1.1**.
Sections: **Now** (amber) · **Waiting on** (terracotta, "Waiting · Nd") · **Captured inbox**
(Alpha empty) · **Upcoming from roadmap** · **Synced from tools** (Alpha empty).

⚠️ **The static mockup shows resting rows only. The interactions below ARE the build spec.**

| Element | Type | Behaviour |
|---|---|---|
| **Task tick (circle)** | **the ONLY complete control** | completes the item (check-pop, strike, **row collapses out**); advances the progress bar. ⚠️ **Wire with `stopPropagation`** so it doesn't also open the editor. |
| **Task row body** | tap | **(G3) opens the item editor** (push from right): Title · Theme · End date · Notes + **Save** / **Mark complete**. ⚠️ **Tapping the row NEVER completes.** |
| **"N done this week ›"** | progress row | **(G5) opens the Completed view** (push from right), grouped **This week / Earlier**; footer "View all in Knowledge" |
| Global bar | floating pill | "Add a task, or ask what's next" — **type to capture into the inbox (G4)**, or open the chat sheet (E4). ⚠️ **Route G4 through the EXISTING N8 orchestrator capture path — it is already confirmation-gated. Do NOT build a second capture mechanism.** |

**Motion — Flow Spec Surface G:**
- **G1 entrance:** header/title fade; **progress bar fills 0→17% over 640ms**; 5 sections
  spring in stagger 80ms; global bar rises.
- **G2 check-off:** tick fills amber + pops `scale 1.25→1` (300ms spring); text strikes to .6;
  **row collapses out (height/margin→0)** 340ms @440ms; bar advances n/12→(n+1)/12.
- **G3/G5 editor / completed:** panel pushes `translateX(100%)→0` (420ms spring); rows stagger
  45ms; back slides off right (300ms in).
- **G4 capture → inbox:** inbox "Empty" collapses; new row grows in `translateY(-8px)` (380ms
  spring); count flips to "1 item"; amber **"Just captured"** chip.

---

## T2.12 · §15 TEAM TAB + AGENT CHATS — WIRE IT, TO SPEC
**Mockups:** `ResolvePM_Team_Tab.dc.html` · `ResolvePM_Agent_Chat.dc.html`
⚠️ **The brief says "Coming soon / out of Alpha scope". IGNORE THAT — Simeon's decision:
N5 is already BUILT AND MERGED, so wire it and make it correct and true to the spec below.**
Drop the "Coming soon" header.

The AI roster: **Product Director · Growth Lead · Tech Lead.**

| Element | Type | Behaviour |
|---|---|---|
| Agent card ×3 | tap | opens that lead's individual chat (**project-contextualised greeting** + starter-prompt chips) |
| Stand-up room | stacked-avatar row | group chat — **only the RELEVANT leads respond**, each reply **labelled with who's speaking** |
| Starter prompt chips | in chat | send that prompt (e.g. "Help me decide", "Turn this into tasks") |
| Send / input | chat footer | message the lead or the room |
| Back arrow | chat header | returns to the Team tab. ⚠️ **Chats drill in — NO bottom nav inside a chat.** |

**Motion:** roster intros ×4 on **first visit only**, staggered (`rvUp` .5s @ .15/.4/.65/.95s)
— three agent cards + the stand-up row fade up `translateY(12px)→0`; **static thereafter**.
**Chat conversation motion (send → thinking → streamed reply) is DEFERRED post-Alpha** — only
the global-bar → chat opening (E4) is specced.
⚠️ N5 was merged TWICE after a force-push — **check the roster for duplicate rows.**

---

## T2.13 · §16 KNOWLEDGE BASE — SHIP THE LIBRARY  *(the biggest new build)*
**Mockup:** `ResolvePM_Knowledge_Base_Tab.dc.html` · motion: Flow Spec H
⚠️ **The static mockup is READ-ONLY. DO NOT BUILD THAT VERSION.**
**DECISION (locked): ship the LIBRARY (Flow Spec H).** Reasons: global-bar captures already
write to `knowledge_base_items` and a read-only view has nowhere to show them; and **the cut
list ("what not to do") has no home anywhere else in the product.**
**Remove the Recommended-action card — the Now theme supersedes it.**

**A library you both read from and add to** — the home for intake knowledge, completed work,
global-bar captures, and **the cut list**.

| Element | Type | Behaviour |
|---|---|---|
| Search + filter chips | **All / Intake / Completed / Files / Notes** | filter the grid; **active chip = ink pill** |
| Typed card | tap | opens a preview/document screen (**push from right**) with a **Download** action |
| Add tile / ＋ / global bar | tap | **bottom sheet: Write a note / Attach a file / Paste text** → drops a fresh card into the grid |
| Global bar | floating pill | "Add to your library, or ask about your project" — captures land here; or opens the chat sheet (E4) |

**Layout:** two-column **typed-card grid**; badges: **Intake · Done · Files · Notes**.
⚠️ **`what_not_to_do` is currently NOWHERE in the UI** — `getIntakeContext` (`:378`) doesn't
even SELECT the column. It is a HARD constraint in the pipeline (blocked themes are pushed to
Later, never Now), so **a user currently sees a theme demoted with no explanation.** Surface it
here as the cut list — **a quiet pointer: seen, not offered.**

**Motion — Flow Spec Surface H (library):**
- **H1 library reveal:** header/head fade; cards reveal grid-y `translateY(14px)
  scale(.96)→none` (420ms spring, **stagger 55ms**); global bar rises.
- **H2 filter chip:** grid reflows + fades up (300ms out); **non-matching cards fade to
  `display:none`**; active chip = ink pill.
- **H3 add / brain-dump:** bottom sheet rises `translateY(100%)→none` (460ms spring); options
  stagger 55ms; **the new card lands top-left** `scale(.9)→none`; the grid shuffles down.
- **H4 open item preview:** preview pushes in from right (420ms spring) with a Download action
  (**same push as G3/G5**).

---

## T2.14 · §17 SETTINGS
**Mockup:** `ResolvePM_Settings.dc.html`
Account and session management. **NOT a tab** — opens from the header avatar (the tab bar is
full at five).

| Element | Type | Behaviour |
|---|---|---|
| Avatar menu | header profile | Profile · Settings · Sign out (Settings opens this screen) |
| Account row | "Simeon Fabian" | opens account detail (chevron) |
| Workspace rows | Project · Plan | Project (chevron); Plan "Alpha · early access" (display only) |
| Daily briefing | toggle · **ON** | morning nudge on the win for today |
| Waiting-on reminders | toggle · **OFF** | ping when something's stalled too long |
| **Reset onboarding flow** | button · Session | ⚠️ **KEEP THIS — it is a real spec'd feature.** Clears the session and replays intake from the start, **but keeps the saved roadmap intact.** |
| Sign out | Session | ends the session (sits **below** Reset) |

---

## T2.15 · THE GLOBAL ORCHESTRATOR (all tabs)
**Mockups:** `ResolvePM_Global_Bar_Contextual.dc.html` · options: `ResolvePM_Global_Bar.dc.html`
The floating pill (**chosen placement: gb**), **docked above the tab bar on EVERY tab once the
app shell has revealed — and NOT during the chrome-free build sequence** (§7.5–§11). It is
currently rendering during the build sequence: **that is a bug.**
Supersedes the old "Search workflows · ⌘K" sidebar element entirely.

| Element | Behaviour |
|---|---|
| Pill | tap → expands into ONE sheet driving **ask · find · go to · capture · confirmed-create** |
| Contextual suggestions | "Try" list, **tab-aware**: Dashboard "What should I focus on?" · Work "Add a task to Now" · Roadmap "What's missing from this plan?" |
| Mic | **DISABLED for Alpha** (Simeon's call) |
| Result rows (gx) | grouped **Ask / Find / Go to / Capture·create** → route to agent chat, open a found item, navigate, or capture/create. **Esc closes.** |

⚠️ **The N8 orchestrator engine already EXISTS and is merged** (isolated router, five intents,
confirmation-gated writes, router proven 5/5). **Wire the UI to it. Do not rebuild the engine,
and do not add a second router.**

**Tab bar:** fixed bottom bar. **LOCKED ORDER: Dashboard · Roadmap · Work · Team · Knowledge.**
Align every mockup to this. ⚠️ **During onboarding only Roadmap is unlocked — the rest are
VISIBLE BUT LOCKED with a padlock badge until the roadmap is confirmed** (Global Bar · gn).
**Settings is not a tab.**

---

## T2.16 · CROSS-CUTTING — TOASTS, CONFIRMATIONS & UNDO (Surface I)
**Mockup:** `ResolvePM_Feedback_Motion.dc.html`
**ONE toast at a time**, seated **just above the global bar**. A new action **fast-swaps** the
current one (160ms out, then in).
⚠️ **When Undo fires, CANCEL any pending follow-through** — e.g. a check-off's row-collapse
timer.

| Element | Behaviour |
|---|---|
| **Undo** | toast action, **reversible** — reverses the action (task returns, tick un-fills); **cancels the timer + pending follow-through** |
| Toast (plain) | **non-reversible** — Send reminder / Save — short confirm, **no Undo, no timer** |

**Motion:** **I1 confirm + undo** — dark pill rises `translateY(20px)→none` (360ms spring);
**amber depletion bar `scaleX(1→0)` over 4000ms**; auto-dismisses if the window lapses.
**I2 undo tapped** — toast dismisses (260ms in); the completed row returns
`translateX(-10px)→none` (300ms spring). **I3 plain confirm** — rises as I1, no timer/Undo,
slides out after 2600ms.

---

# PHASE 3 — AUDIT, THEN RETEST

**T3.1 · Audit the build — TWICE, INDEPENDENTLY** (once Codex, once Claude Code; neither sees
the other's output). Walk **every ticket above, element row by element row**, at 390px, against
real data. Screenshot each screen. Click each row. Report **MATCHES / DIVERGES (say how)**.
*(This method already paid: two independent route audits disagreed on a root cause, and the
deeper one was right.)*

**T3.2 · Fix round** from the audit diffs.

**T3.3 · THE RETEST — Simeon's end-to-end walk.** Fresh real intake, phone width, all the way
through. **The outcome is met when the walk matches this pack and nothing is dead.**

---

# APPENDIX — THE THREE FAILURES THIS PACK EXISTS TO PREVENT

1. **A decision that lives only in prose loses to an instruction that lives in the repo.**
   The banned halt screen was IN the spec; the builder followed it faithfully. The reversal
   was recorded in Notion and never propagated to the spec files. **That is why T0.3 exists,
   and why every ticket above carries its behaviour INLINE rather than pointing at a mockup.**

2. **"Built" never meant "reachable and working".** The floor had zero callers. The halt screen
   rendered nowhere. N9 was built and never merged while a legacy page served real users.
   **That is why acceptance requires a click, not a build.**

3. **Generation and enforcement shared a call.** Fires 0/50. Re-learned three times.
