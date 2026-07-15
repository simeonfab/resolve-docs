# RESOLVE — AUDIT REGISTER

**Compiled 14 July 2026.** Every finding from every diagnostic and audit run 13–14 July.

**Purpose:** one place to see every known issue *before* deciding how to fix any of them.
Nothing here is a plan. This is the evidence base a plan gets built on.

**Status key:** `CLOSED` (resolved, evidence recorded) · `FIXED-UNMERGED` (fix exists on a
branch, not on main, not proven in a browser) · `OPEN` (real, unfixed) · `BLOCKED` (needs a
decision from Simeon) · `SYSTEMIC` (a process failure, not a code bug)

---

## THE AUDITS THEMSELVES

| # | Audit | Scope | By | Outcome |
|---|---|---|---|---|
| A1 | Git incident diagnostic | cap-pm repo state | Claude Code | No incident. Found 1.4 was never committed. |
| A2 | Intake null-outcome diagnostic | Intake git + Vercel + Supabase | Claude Code | False alarm. Nothing broken. |
| A3 | Preview walk (run 1) | Live pipeline, real intake | **Simeon, by hand** | Found the banned halt screen. |
| A4 | Halt-path enumeration | cap-pm halt paths + refusal logging | Claude Code | Found Gate 2 *also* halting, and logging nothing. |
| A5 | Route audit | Every reachable route vs design spec | **Codex** | 6 routes not in spec. Race on work-item persistence. |
| A6 | Route audit (independent) | Same scope | **Claude Code** | Agreed on routes. **Disagreed on the work-items root cause.** |
| A7 | **Intake audit (FE + BE)** | — | *not yet run* | — |
| A8 | **ResolvePM audit (FE + BE)** | Beyond routes | *not yet run* | — |

**A5 vs A6 is the most valuable thing we did.** Two providers, same brief, one material
disagreement (F10 vs F11) — and the deeper cause won. **Running audits twice is now standard.**

---

## WHAT IS ACTUALLY TRUE RIGHT NOW

- **cap-pm `main` = `3d6b560`.** Intact. No incident ever happened. It still runs the OLD
  string-concat generator, the legacy `/roadmap`, and the leaky queries.
- **All the real work is on branches, unmerged and unproven in a browser:**
  `claude/referee-integration-drafts-uuadkz` (`b7b98ce` — the rebuilt pipeline) and
  `claude/route-audit-design-specs-cytvw6` (`85812f2` — the query scoping fix).
- **Intake production = `4e6c16e`.** Live and correct. The outcome question works.
- **The pipeline has run exactly once, on one real intake** (session `cee60b5e`, project
  `af8abaa8`, YGOBrain). It produced 2 provisional themes, **0 work items**, and 44 refusal rows.

---

## FINDINGS — PIPELINE & GENERATION

| # | Finding | Status | Evidence |
|---|---|---|---|
| **F1** | "A chat killed everything" — repo damage | **CLOSED** | No incident. `main` = `3d6b560`, ancestor check PASS, no force-push, rules 1–12 intact. The chat lost its own uncommitted container work and reported it as repo damage. |
| **F2** | **1.4 (generation replacement) was NEVER COMMITTED — anywhere.** The string-concat generator was live in production. Every prior "it's gone" claim was false. | **FIXED-UNMERGED** | `git log --all`: no 1.4 commits in any ref. Rebuilt: `662cd4b`. |
| **F3** | Floor built, merged, **zero callers**. Never fired. | **FIXED-UNMERGED** | `01a3489` |
| **F4** | Referee built, merged, **zero callers**. Never fired. | **FIXED-UNMERGED** | `079a54c` |
| **F5** | `check-theme` used `canStandAsTheme` (keyword heuristic), not the referee | **FIXED-UNMERGED** | `079a54c` — re-pointed at the referee |
| **F6** | **The escalation path HALTED the user with the BANNED question** — *"What is the ongoing lever here?"* It re-introduced through a back door exactly what the product reversal outlawed. **The spec told it to.** | **FIXED-UNMERGED** | `e908c77`. Found by Simeon on the preview (A3). |
| **F7** | **Gate 2 was ALSO halting.** `roadmap-onboarding.ts` halted on *any* `failed_gate` — in the very build whose brief said "Gate 2 NEVER halts". | **FIXED-UNMERGED** | `e908c77`. Found by A4; nobody asked for it. |
| **F8** | The escalation halted and **logged nothing**. `runRefereeChecks` never called `createRefusal`. | **FIXED-UNMERGED** | `3e491dc` |
| **F9** | Gate 2 was **silent on the passive path** — `evaluateFloorForSession` passes `persistRefusal: false`. So `refusals = 0 rows` never meant "nothing fired"; it meant **"nothing was recorded"**. | **FIXED-UNMERGED** | `3e491dc`. `refusals` now at **44 rows** (34 `referee_reject`, 10 `cut`). |
| **F10** | **NO WORK ITEMS ARE EVER GENERATED.** The rebuilt pipeline generates only THEMES. `taskPool` is populated **only as an accident of misclassification** (`roadmap-generation.ts:544`) and is **wiped entirely on the degrade path** (`:616`). So `todos = []` and the (correct) insert loop never runs. **A roadmap with no work is not a product.** | **OPEN — CRITICAL** | A6. DB: `af8abaa8` = 2 initiatives, **0 work_items**. Every other project has 10–11. |
| **F11** | **Race on persistence.** `void persistRoadmap()` fires and Continue is enabled **without awaiting it**. Navigate away mid-flight → initiatives insert, work items don't. | **OPEN** | A5. Secondary to F10, but independently real — would cause intermittent partial writes even after F10 is fixed. |
| **F12** | Generation runs on **every pre-confirmation `/roadmap` load** — non-deterministic across reloads, several sequential LLM calls of latency, and refusal-row spam. | **OPEN** | Self-reported by the builder. Fix = persist the draft. |
| **F13** | **The CANONICAL specs in `resolve-docs/resolve-pipeline/` are STILL STALE** — they still say *"escalate with a question"*. Only cap-pm's convenience copies were corrected. **The next agent will rebuild the halt screen straight out of them.** | **OPEN** | Flagged by the builder of `b7b98ce` itself. |

---

## FINDINGS — DATA LAYER

| # | Finding | Status | Evidence |
|---|---|---|---|
| **F14** | **CROSS-PROJECT LEAK.** `listInitiatives` / `listWorkItems` / `listNotes` / `listUpdates` (+ by-id variants) filtered by `user_id` only, never `project_id`. **Any user with >1 project saw them all mashed together.** | **FIXED-UNMERGED** | `85812f2`, prod-verified: the affected user went from **14 initiatives across 5 projects → 2** (the active project's). Leaked row `e1f75947` gone. |
| **F15** | Four creates (`notes`, `updates`, `integration_accounts`, `sync_events`) **never wrote `project_id` at all** — so scoping the reads without fixing the writes would have made every new row **invisible**. | **FIXED-UNMERGED** | `85812f2` |
| **F16** | **21 orphaned `work_items` with `project_id = null`.** Confirmed pre-Alpha debris: 18 `Demo:` rows seeded 1 June under `simeonfab2@`, 3 `test`/`test_item` rows from May (one under `j.orotayo@hotmail.co.uk`). Their parent initiatives are orphaned too. | **OPEN — low** | Rows deliberately left in place. `project_id NOT NULL` migration **proposed, not run**. |

---

## FINDINGS — FRONTEND / ROUTES

| # | Finding | Status | Evidence |
|---|---|---|---|
| **F17** | **`/roadmap` (post-confirmation) renders a LEGACY page** — Quarter/Status/RAG filters, "grouped by delivery quarter", raw CRUD. In no spec. **It is what a real user sees today.** | **OPEN** | A5 + A6 agree. |
| **F18** | **N9 (the DESIGNED Roadmap tab) was BUILT AND NEVER MERGED.** Branch `claude/roadmap-tab-step-3-cwrt6p` (`d746473`), ~30 commits stale. **DO NOT MERGE THE BRANCH** — its diff would delete the pipeline, floor, referee, global bar and team work. Cherry-pick the 2 page files only. | **OPEN** | A5 + A6 agree. A6: "matches `ResolvePM_Roadmap_Tab_dc` near-pixel". |
| **F19** | **SIX ROUTES NOT IN ANY SPEC, all reachable:** `/feedback`, `/updates`, `/roadmap/presentation`, `/settings/database-health`, `/work-tracker` (redirect stub), `/auth/callback` (no OTP callback screen exists). | **OPEN — delete** | A5 + A6 agree on all six. |
| **F20** | **A "Reset onboarding" TESTING CONTROL is live in `/settings` AND on `/roadmap`.** A real founder would see it. | **OPEN — delete** | A5 + A6 agree. |
| **F21** | **`what_not_to_do` is NOWHERE in the UI.** It is captured, and it is a **hard constraint** in the pipeline (blocked themes are pushed to Later, never Now) — so a user sees a theme demoted **with no explanation**. `getIntakeContext` (`:378`) doesn't even SELECT the column. | **BLOCKED — Simeon's design call** | **A GENUINE SPEC CONTRADICTION:** `INDEX.md` says N6 "hosts the refusals / cut record". The N6 brief and the Knowledge Base mockup contain **no such section**. Both auditors refused to invent it. Correctly. |
| **F22** | **NO SCREEN HAS EVER BEEN VERIFIED IN A BROWSER AGAINST ITS MOCKUP.** Every screen was verified by the agent that built it, in isolation, pre-merge. Both audits mapped routes from **source**, and both said the same thing: *"browser/pixel proof still absent."* | **SYSTEMIC** | A5 + A6. This is the root of the entire category. |
| **F23** | The I1 intake reskin is live in production but has **never been compared to its mockups**. | **OPEN** | — |
| **F24** | Simeon reports **formatting / spec divergences across multiple screens** — *"not matching the agreed roadmap format"*, *"some of this is a previous version"*. **Not yet enumerated.** | **OPEN** | Needs the FE audit (A8). |

---

## FINDINGS — INTAKE

| # | Finding | Status | Evidence |
|---|---|---|---|
| **F25** | **THE OUTCOME QUESTION ACCEPTS HALF AN ANSWER — AND STAMPS IT CONFIRMED.** Asked: *"how will players be better off, **and how would you know?**"* Answered: *"they would be able to make better decks easier and keep track of the meta"* — beneficiary given, **observable change entirely skipped.** Nothing required the second half. Written to `outcome` with `outcome_confirmed = TRUE`, so downstream it looks **more** trustworthy than an inferred outcome, while missing the exact part that makes an outcome usable. **This is the root cause of the weak roadmap.** | **OPEN — HIGHEST LEVERAGE** | A3. Doctrine: *Gate 2 firing means our INTAKE QUESTIONS are wrong. Fix upstream, never at the gate.* |
| **F26** | **Fail-silent handoff.** `buildResolvePmHandoffPayload` (`index.html:1787-88`): `top_priorities: recommendationData?.topPriorities \|\| []`. `validateRecommendationShape` **does** require both fields (`resolve.js:215`, `:216-228`) — so the `\|\| []` fires **only when the entire `recommendationData` object is missing**, i.e. total failure. **A total failure is silently handed off as an empty-but-valid-looking payload.** | **OPEN** | A2. *(Corrects an earlier backlog entry that wrongly claimed the validator didn't require them.)* |
| **F27** | Call 3 (Intake's QA self-check) — the same model checking its own Call 2 output. The self-validation gap. Superseded by the referee. | **OPEN — low** | Near-decorative, not harmful. Needs an Intake deploy. |

---

## FINDINGS — REPO / PROCESS

| # | Finding | Status | Evidence |
|---|---|---|---|
| **F28** | **N5 was merged TWICE** (`1bee9b7`, `f6058fd`) after a force-update of its branch. | **OPEN — watch** | Check the team tab for duplication artefacts. |
| **F29** | **Two divergent N8 orchestrators.** `d1cb2e6` (merged, authoritative) vs `6a474b1` (unmerged). The same feature built twice. | **RESOLVED** | Main's wins. Do not merge `6a474b1`. |
| **F30** | The resolve-docs **filing branch is unmerged** (`e350b16`). `INDEX.md` on main still marks Agent_Chat "BETA SCOPE" and Team_Tab "N5-lite" — **both stale, and both will mislead the next agent.** | **OPEN** | Merge it. |
| **F31** | **ResolveOS four-role model has NO extraction doctrine.** `business-analyst.md` (327 lines: requirement decomposition, stated vs implied vs assumption, gap-question discipline) was compressed to ~3 lines in `product-director.md`. **Intake's Call 1 depends on it** — and **F25 is arguably a direct consequence.** | **OPEN** | Roles epic. |
| **F32** | ResolveOS filing branches disagree. Use `claude/resolveos-docs-filing-2` (`0668c55`) — the coherent four-role tree. `filing-pass-2` has a mixed ten-role tree. | **OPEN — trap** | — |
| **F33** | **The floor's CRITERION is known-wrong and deliberately shipped that way.** "Lever = no natural endpoint" is neither necessary (a bounded intervention can be the true causal lever) nor sufficient ("do more marketing" is endless and constrains nothing). **DO NOT attempt another prompt pass — four were tried.** | **DEFERRED — Beta spike** | Architecture proven; only the criterion is wrong. |

---

## THE PATTERN BEHIND ALL OF IT

Three failures repeat, and none of them is carelessness:

1. **A decision that lives only in prose loses to an instruction that lives in the repo.**
   The escalation halt (F6) *was in the spec*. The builder followed it faithfully. The
   product reversal was recorded in Notion and never propagated to the spec files.
   **F13 means this is still true right now.**

2. **"Built" has never meant "reachable and working".** F3, F4, F17, F18, F21, F22 are all
   the same bug wearing different clothes: a thing was built, verified by its own builder,
   and never once clicked by anyone.

3. **Never bundle a judgement with a generation in one call.** It fires 0/50. Re-learned
   three separate times (the floor, the stand-up router, the orchestrator router).

**And the communication gap:** a mockup shows *appearance*. It says nothing about which
elements are interactive, what each does when clicked, what data fills it, or what it shows
when empty. So an agent builds something that looks right, verifies itself, and nobody ever
presses a button. The fix is the **Screen Contract**.
