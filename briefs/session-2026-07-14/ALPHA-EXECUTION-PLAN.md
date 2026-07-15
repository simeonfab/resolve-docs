# ALPHA EXECUTION PLAN — from current state to the Implementation Brief

**14 July 2026.** Authority: `ResolvePM_Implementation_Brief_dc.html` (the consolidated
build + motion brief). This plan gets the product from what exists today to: *Simeon walks
intake → app end to end and sees exactly the brief, with every button and the backend working.*

**The acceptance test (the outcome, verbatim):** one full run-through on a phone-width
browser — intake screens 1–6 → bridge → sign-up 7 → build sequence 7.5/8/9/10 → OS reveal 11
→ Dashboard 12 → each tab 13–17 → global bar on every tab — where every screen matches its
brief section and **every element row in the brief has been exercised**. No legacy pages, no
debug controls outside spec, no dead buttons.

---

## CURRENT STATE (verified, one paragraph)

cap-pm `main` = `3d6b560`: old generator, legacy `/roadmap`, leaky queries, floor/referee
unwired. The real work sits on two branches: `claude/referee-integration-drafts-uuadkz`
(`b7b98ce` — rebuilt pipeline, Gate-1-only halt, referee wired, refusal logging; **exercised
once in a browser on a real intake**) and `claude/route-audit-design-specs-cytvw6`
(`85812f2` — project-scoped queries, prod-verified by query). N9 (Roadmap tab) is built on a
stale branch (`d746473`) — cherry-pick 2 files, never merge. Intake prod (`4e6c16e`) is live
and correct. Critical known gaps: **F10** no work items are ever generated · **F12** nothing
is persisted as stable state (brief requires "stored, not recomputed") · **F25** the outcome
answer is unvalidated · **F26** fail-silent handoff. Full register: `RESOLVE-AUDIT-REGISTER.md`.

---

## PHASE 0 — CONSOLIDATE TRUTH  *(half a day; mostly merges + docs)*

**0.1 Merge the two cap-pm branches to `main`** — pipeline (`b7b98ce`) then query-scope
(`85812f2`). Both are branch-verified (one browser walk, one prod query). Everything after
builds on one base instead of a branch stack. *(Router: mechanical → Codex/Sonnet.)*

**0.2 Merge the resolve-docs filing branch** (`e350b16`) — kills the stale INDEX (F30).

**0.3 File the Implementation Brief into resolve-docs as THE authority.** Add a supersede
note to briefs 01–14 and the old motion spec: "superseded by the Implementation Brief where
they differ." Fix the still-stale canonical pipeline specs (F13: "escalate with a question"
must die in `resolve-pipeline/` too). Update Notion Current Context to point at the brief.
**This step prevents the F6 failure from ever recurring — do not skip it.**

**0.4 Delete the out-of-spec routes** (F19): `/feedback`, `/updates`,
`/roadmap/presentation`, `/settings/database-health`, `/work-tracker`, `/auth/callback`
(confirm no provider uses it). **Correction to F20:** the brief *keeps* "Reset onboarding
flow" in Settings §17 as a real feature (clears session, keeps roadmap) — so delete the
legacy `/roadmap` instance only (dies with the page in 2.4), keep Settings' per spec.

---

## PHASE 1 — BACKEND SPINE  *(the buttons need something to do)*

**1.1 F10 — TASK GENERATION (critical, blocks screens 9, 12, 14).** New pipeline stage:
generate the first-week task pool from the confirmed themes (Now theme weighted), isolated
calls (generate ≠ classify ≠ judge — 0/50 rule), deterministic assembly into `work_items`
with `initiative_id` + `project_id`. Feeds: To-do reveal counter ("2 of 12"), Work tab
"never blank", Dashboard win. *(Router: build-hard, data-touching → top model, high effort;
verify: other provider.)*

**1.2 F12 + F11 — PERSIST THE DRAFT; AWAIT THE WRITE.** The brief is explicit: *"the
Dashboard win and the confirmed Roadmap are stored, stable state — not recomputed on load.
Never show a refresh affordance."* Persist the generated draft once; confirmation
`await`s (kills the F11 race); reloads read, never regenerate. Also ends refusal-row spam.

**1.3 Intake: F25 + F26.** Outcome answer content-validated (beneficiary + observable
change; isolated call; re-ask once; still partial → accept with `outcome_confirmed=false`).
Handoff: a missing `recommendationData` is a visible failure, never `|| []`. *(Intake repo;
deploys to prod — Intake has real traffic exposure, so branch → preview → verify → merge.)*

**1.4 Unify the demo dataset** (brief "Still open"): one story — Resolve's own project —
across intake chips, roadmap cards, Dashboard, Settings, Part E. Content pass, no logic.
Theme wording stays placeholder per Settled §4.

---

## PHASE 2 — SCREEN CONTRACT BUILD  *(the brief, section by section)*

**The ticket rule for every item below:** the ticket body is the brief's section pasted
**verbatim** (elements & actions + motion + decisions) — no summarising, no pointing at a
mockup and hoping. Acceptance = every element row clicked in a browser against real data,
every state seen, artifact per row, **builder ≠ verifier**. This rule exists because the
last round's tickets carried the mockup and dropped the brief — that is how "built to spec"
failed.

In journey order:

- **2.1 §7.5 Floor Refusal reskin.** The built halt screen (from `9c5de37`) vs the new
  mockup: states Default→Filled→Submitting→Passed, dynamic reason/question flexing 8→30
  words, "Re-running the check", "That's the piece we needed." — and **no chrome**: the
  global bar must NOT render during the build sequence (it does today — Simeon's screenshot).
- **2.2 §8–10 build-sequence contract pass** (R2/R3/R4 are built): correction path re-runs
  the **referee** server-side; approved-row motion; **screens handle a variable theme count**
  (pipeline may return 1–2; "3" in the brief is the demo, not a contract); §10 gets the
  provisional marker treatment A + the Product Director input (wire it to the N8 orchestrator
  "ask" intent — do not build a second ask path).
- **2.3 §11–12 OS Reveal + Dashboard contract pass** (N2/N3 built): E-motions, expand-in-place
  for Waiting-on/Today, win routes by `winRouteLabel`, avatar menu.
- **2.4 §13 Roadmap tab.** Cherry-pick N9's two page files onto current main (NEVER merge
  `d746473`'s branch). Then: remove the Product Director FAB (global bar absorbs it), F3
  inline expand with "Go to work item", F5 matrix OUT, provisional tooltip on by-theme header
  only. **Legacy `/roadmap` dies here.**
- **2.5 §14 Work tab G2–G5** (N4 is static rows today): check-off collapse + progress
  advance, row-tap editor (Title·Theme·End date·Notes), capture-to-inbox **via the N8
  orchestrator capture path** (confirmation-gated — no second capture mechanism), completed
  view. Depends on 1.1.
- **2.6 §16 Knowledge LIBRARY** (biggest new build): search + filter chips, typed-card
  two-column grid, add-sheet (note/file/paste), item preview + download, **the cut-list
  ("what not to do") gets its home here** (resolves F21 — the brief overrides the old N6
  contradiction), Recommended-action card removed, global-bar captures land here.
- **2.7 §17 Settings contract pass** (N7 built): rows per spec, toggles, Reset-onboarding
  behaviour exactly as written (clears session, **keeps the saved roadmap**).
- **2.8 Global bar contextual + gx + onboarding locks:** per-tab prompt/suggestions, mic,
  gx grouped sheet; **tab locking with padlock badges until roadmap confirmed** (Global Bar
  gn); bar hidden during the build sequence; docked on every tab after reveal.
- **2.9 §6–7 bridge + sign-in exit transitions** (new motion): 9.2s bridge with the **seam
  contract** (R1's first frame === bridge beat 4; eyebrow/subhead/footer at opacity:0),
  D1–D3 exit, code-entry error states, resend cooldown. "Download report" PDF: **audit
  first** — if no generator exists behind `project_plan_report`, it's a Phase 1 add.
- **2.10 §15 Team = "Coming soon" gate.** N5 is built and merged; the brief descopes it.
  Feature-flag it off behind the Coming-soon header — **do not delete it** (it returns
  post-Alpha; router + chats are proven).
- **2.11 Surface I — toasts + undo:** one toast at a time above the bar, 4s depletion undo
  on check-off (cancels the pending collapse), plain confirms. Small but it's what makes
  G2 feel safe.

*(Router for Phase 2: mechanical contract passes → Codex/Sonnet; new-surface builds
2.5/2.6/2.8/2.9 → top model; every verify → the other provider or Simeon.)*

## PHASE 3 — AUDIT THE FIXES, THEN RETEST

- **3.1** Run the Intake audit (A7) and the ResolvePM audit (A8) — **twice each, Codex +
  Claude Code, independently** — using the Implementation Brief as the element-by-element
  checklist (it says it was built for exactly this). Screenshots per screen, click per row.
- **3.2** Fix round from the audit diffs (should be small if 2.x held the ticket rule).
- **3.3 THE RETEST = Simeon's end-to-end walk** on a fresh real intake, phone width.
  Outcome met when the walk matches the brief and nothing is dead.
- **3.4** Then the product-quality track resumes: 4–5 real intakes → **1.6 blind grade**
  (the go/no-go on whether the roadmap is any good — unchanged, still pre-registered,
  still graded by the other provider).

---

## DECISIONS THE BRIEF LEAVES OPEN — Simeon to call

1. **Mic buttons** (intake textarea §3 + orchestrator): the brief itself says "confirm
   Alpha scope." In or out? *(Real dictation is non-trivial; browser SpeechRecognition is
   Chrome-only-ish.)* → my rec: **out for Alpha**, keep the icon disabled like "Add material".
2. **"Download report" PDF** (§6): if no generator exists, build it in Alpha or stub the
   button? → my rec: build it — it's the founder's tangible take-away; but audit first.
3. **Team gate copy** (§15): confirm "Coming soon" header treatment over hiding the tab
   entirely (tab bar is designed as five).
4. **Theme count**: confirm screens treat 1–3 themes as valid (scarcity is the point);
   the brief's "3" reads as demo content, not a contract. Silence = I proceed on 1–3.

## STANDING RISKS CARRIED IN

- The floor **criterion** stays known-wrong (F33, Beta spike) — Gate 2 marks provisional,
  which contains the damage. Do not reopen.
- N5 double-merge (F28): watch the Team tab for duplication artefacts when gating it.
- 21 orphan rows + `project_id NOT NULL` migration: after Alpha, per the filed ticket.
