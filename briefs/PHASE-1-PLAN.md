# Phase 1 Plan — Pipeline Build (grounded, 11 July 2026)

Every claim below is grounded in code read this session, the live DB, or the pipeline
modules — or is explicitly marked OPEN. Sources: Intake `api/resolve.js` +
`public/index.html`; ResolvePM `roadmap-onboarding.ts`, `roadmap-onboarding-form.tsx`,
`project-handoffs.ts`, `spine.ts`, `update-drafts.ts`; live Supabase schema + RLS;
pipeline modules 00/03/05/08; RESOLVE-ALPHA-MASTER-PLAN.

---

## A. Decisions the code read settles

### A1. Floor placement: ResolvePM, post-handoff, before theme generation.
Three grounded reasons, none of them principle-arguments:
1. **The floor was validated on the post-Call-2 data shape.** Its eval intakes
   (`floor-intakes-with-outcomes.json`) are `{outcome, project, top_priorities,
   recommended_action, what_not_to_do}` — the session row. The Intake gap-question
   loop runs between Call 1 and Call 2, before `top_priorities` /
   `recommended_action` / `what_not_to_do` exist. Placing the floor there feeds it
   less than it was proven on. Placing it post-handoff feeds it exactly the
   validated shape.
2. **Outcome capture in Intake is Stage 1 work, explicitly provisional until
   Phase S** ("machinery now; question set after" — master plan). An Intake
   placement forces that deferred work onto the critical path.
3. **Intake is the unstable surface right now** — `api/resolve.js` loads the two
   RETIRED role files by path (Finding B). No Intake deploy until that's
   reconciled. ResolvePM placement keeps Finding B off the Alpha critical path.

### A2. The outcome problem solves itself via Gate 1.
No `outcome` column exists (live DB, verified) and no current intake captures one.
Rather than inventing outcome capture: **the floor's own design handles it.** Gate 1 =
"outcome present." Absent → INSUFFICIENT → the floor's single question IS the outcome
question ("If this works, who is better off, and how?"). The user's answer is stored as
`project_sessions.outcome` with `outcome_confirmed = true` — true by construction, the
user typed it. Floor re-runs with outcome present → Gate 2 (lever). When Phase S later
moves outcome capture into Intake (Stage 1), the floor finds it present and skips
straight to Gate 2. Clean migration path, zero invented data, zero Intake changes now.

### A3. The halt surface: one new interstitial before R2, in the existing
chrome-free build sequence. The post-auth flow (R2→R3→R4) already has the shell and
progress bar. The floor interstitial shows the one question + an input; answer appends
to `gap_answers` (the **paired `{question, answer}` structure — verified landed** in
both frontend `collectGapAnswers` and API `formatGapAnswers`; the old
successDefinition/deadline bug survives only in `snapshots/`), floor re-runs, proceeds
on SUFFICIENT.
**Constraint from cap-pm's own rules: no screen build without its mockup.** The
interstitial has no dc mockup. → A small mockup task precedes ticket 1.4. Flagged, not
skipped.

### A4. Referee wiring: behind the existing seam.
`/api/roadmap-onboarding/check-theme` already exists — server-side POST `{text}` →
`{ok, warning}`, called by R2's "Not quite" (`roadmap-onboarding-form.tsx`, read).
Upgrade its internals from `canStandAsTheme` (keyword heuristic, deleted) to the
referee's three theme clauses. **Frontend change for "Not quite": zero.** The full
referee (3 theme clauses + 2 DoD clauses, AND rule, regenerate loop N=2) wraps Stage 4
generation server-side before anything reaches R2.

### A5. The parser bug dies by construction, using infrastructure that already exists.
`update-drafts.ts` (read) already contains the exact pattern the referee needs:
OpenAI **Responses API with strict `json_schema` structured output**, plus
`assertAiUsageAllowance()` and `recordGenerationAttempt()` usage tracking. Referee
verdicts as `{verdict: "PASS"|"FAIL", reason: string}` strict schema = the
`REASON:`-label parser bug (~30% silently dropped) cannot occur. Ticket 0.1 is
absorbed into 1.1, not a separate fix to fragile text parsing.
**Acceptance for 0.1 stands:** replay the eval JSONLs with 0 dropped verdicts —
**OPEN: confirm the eval JSONLs were actually saved out** before that container
reset. If lost, acceptance becomes a re-run of the clause evals on the 10 synthetic
intakes instead. Do not skip; pick whichever is possible.

### A6. Call 3 (Intake's QA self-check) is condemned but not executed yet.
It's the self-validation gap in production (same model checks its own Call 2 output).
The referee supersedes its function. But removing it is an Intake deploy → **deferred
to the Intake pass** (with Finding B and Stage 1 outcome capture). Logged, not lost.

---

## B. Schema migration (grounded in live DB + real RLS)

All RLS policies verified: `user_id = auth.uid()` isolation pattern
(`cap_pm_authenticated_user_isolation_*`). The new table copies it.

```sql
-- 1. Outcome anchor (written by the floor loop per A2; later by Intake Stage 1)
ALTER TABLE project_sessions ADD COLUMN outcome text;
ALTER TABLE project_sessions ADD COLUMN outcome_confirmed boolean NOT NULL DEFAULT false;

-- 2. Derived provisionality (Stage 6). provisional computed in code from inferred_from.
ALTER TABLE initiatives ADD COLUMN inferred_from jsonb;
ALTER TABLE initiatives ADD COLUMN provisional boolean NOT NULL DEFAULT false;

-- 3. Refusals: floor verdicts, referee rejections, the cut. Audit store.
CREATE TABLE refusals (
  id uuid PRIMARY KEY DEFAULT gen_random_uuid(),
  user_id uuid NOT NULL,
  project_id uuid,
  session_id uuid REFERENCES project_sessions(id),
  kind text NOT NULL CHECK (kind IN ('floor_insufficient','referee_reject','cut')),
  verdict text, reason text, question text, payload jsonb,
  created_at timestamptz NOT NULL DEFAULT now()
);
ALTER TABLE refusals ENABLE ROW LEVEL SECURITY;
CREATE POLICY cap_pm_authenticated_user_isolation_refusals ON refusals
  FOR ALL USING (user_id = auth.uid());
```

Not added, deliberately: `work_items.sequence` and `due_date` already exist (verified)
— Stage 6's Now/Next/Later derivation uses them. The user-visible cut goes to
`knowledge_base_items` (exists, verified) as a row, separate from the `refusals` audit
row — two different jobs, two homes.

---

## C. Build order (dependencies, not preference)

**1.0 — Schema migration** (section B) on a Supabase branch, reviewed, then applied.
Small, unblocks everything. *(= master plan 1.3, pulled first: it blocks nothing and
everything downstream reads it.)*

**1.1 — Referee service** (cap-pm, server-side). Package the five clauses **verbatim
from `05-referee.md`** as isolated calls via the `update-drafts.ts` structured-output
pattern. AND rule; regenerate-with-reason loop, N=2; every rejection logged to
`refusals`. Absorbs 0.1 per A5. *Proven component; this is packaging.*

**1.2 — Floor service** (cap-pm, server-side; parallel with 1.1). Gate 1 (outcome
present) + Gate 2 with the **corrected lever wording from the master plan** ("continued
effort keeps producing value, no natural endpoint" — `03-floor-gate.md`'s own Gate 2
wording is superseded, its note says so). Output contract `{verdict, reason, question}`
via structured output. Verdicts logged to `refusals`.
**Standalone test before wiring — the highest-risk assumption:** run against the 10
synthetic intakes; must return INSUFFICIENT on the no-lever ones (dementia P3, charity
P7, local-news P10) and SUFFICIENT on clear-lever ones. If it can't discriminate as a
separate call, STOP — the pipeline doesn't get built on top of a floor that doesn't
hold. (It failed as a prompt instruction 0/50; the separate-call version is the unproven
piece.)

**1.3 — Floor interstitial mockup** (small, parallel): the one-question halt screen in
the R2 shell. Required by cap-pm's no-screen-without-mockup rule before 1.4 builds it.

**1.4 — Generation replacement + wiring** (needs 1.0–1.3). Delete `buildThemeDrafts`,
`buildTodoDrafts`, `canStandAsTheme`. Stage 4 server-side: model generation → SEPARATE
isolated classify → `what_not_to_do` filter (finally READ — it's in the session row,
verified) → choose one + candidate DoD → referee. Wire the floor + interstitial in
front. Upgrade `check-theme` internals to referee theme-clauses (A4). Stage 6
derivations replace `confirmRoadmapOnboarding` VALUES (distilled names, approved DoD,
self-completing titles, priority from Now/Next/Later, due_date from sequence,
provisional from `inferred_from`); keep its existing manual-rollback pattern —
Supabase JS has no transactions (known constraint, and the current code already
handles it that way, verified).

**1.5 — Chain + acceptance.** End-to-end on synthetics, then the 2 real tester
intakes. Then the go/no-go: new pipeline vs current string-concat, same intakes,
**Simeon blind-grades**. Nothing verifies its own work — the builder does not grade.
Production untouched until this passes. Branch-first throughout; verify on preview
against the canonical test project (`c0a10000-…0001`), never fixtures.

---

## D. Open items (named, not smoothed over)

1. **Eval JSONLs** — saved out or lost? Determines 1.1's acceptance path (replay vs
   re-run). Ask/check before 1.1 starts.
2. **Floor-question answer storage** — proposed: answer appends to the `gap_answers`
   paired array (so all downstream readers see it) + the verdict/question row in
   `refusals`. Confirm at 1.4 ticket-writing.
3. **`confirmRoadmapOnboarding` work-item insert tail** — initiatives portion read in
   full (first theme `high`, rest `medium`, manual rollback); the todo-insert
   continuation read in part. Stage 6 replaces these VALUES anyway; the builder reads
   the full function before touching it.
4. **Finding B (retired role files in Intake)** — off the critical path with this
   placement, but a hard blocker on ANY Intake deploy. Belongs to the refresh/Intake
   pass with Call-3 removal and Stage-1 outcome capture.
5. **Per-call model choice** — cap-pm uses `OPENAI_MODEL` env (verified); referee/floor
   calls are tiny. Config decision at 1.1/1.2, not architecture.

## E. Routing (one line, per the overlay)

Plan: done here. 1.0: mechanical → Codex. 1.1/1.4: build-hard, data-touching → top
model, high effort. 1.2: build-hard, highest-risk → top model, high effort, and its
standalone test is graded against expected verdicts, not self-certified. 1.3 mockup:
design pass. 1.5 verification: different provider than whoever built 1.4.
