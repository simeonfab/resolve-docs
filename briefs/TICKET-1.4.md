# Ticket 1.4 — Generation replacement + wiring

Repo: `cap-pm-cockpit-alaria`. **This is the ticket that kills the fake pipeline.**

`Route: build-hard (novel, data-touching) · Builder: Codex · Reviewer: top model,
different provider · Proof: real intake through the real flow, real DB rows, verified on
preview against a real non-fixture project — never a fixture.`

**Depends on (all merged to main, verified):** 1.0 schema · 1.1 referee · 1.2 floor ·
outcome capture live in Intake + cap-pm. **1.3 (refusal screen mockup) must exist before
you build any screen** — no screen build without its mockup. If it isn't there, build the
server-side work and STOP before the UI.

---

## What exists today (verified by code read — do not re-derive)

`getRoadmapOnboardingDraft` reads the latest `project_sessions` row → `buildThemeDrafts`
→ theme cards (R2) → user confirms at the 3rd card → `confirmRoadmapOnboarding` inserts
`initiatives` + `work_items`.

**`buildThemeDrafts` and `buildTodoDrafts` are pure string concatenation. There is no
model call.** Themes are copied verbatim from `top_priorities`. Titles are
`"{action} for {theme}"`. Priority is hardcoded `'medium'`. Dates are null.
`canStandAsTheme` is a keyword heuristic. `what_not_to_do` is captured at intake and
**never read**.

That is the thing being replaced.

## What must NOT change

- **`/api/roadmap-onboarding/check-theme` keeps its contract** (`POST {text}` →
  `{ok, warning}`). R2's "Not quite" already calls it. **Swap the internals only —
  frontend change for that path is ZERO.**
- The R2 card flow, the confirm-at-third-card interaction, and the existing shell.
- `confirmRoadmapOnboarding`'s manual-rollback pattern (Supabase JS has no transactions;
  the current code already handles this — keep it).

---

## THE RULE THAT GOVERNS THIS TICKET

**Generation and enforcement never share a call.** The floor and the referee are
ISOLATED, reject-only calls. **Do not** fold either into the generation prompt to "save a
call." Evidence: criteria inside a generation prompt were ignored 130/130; a floor
instruction inside a generator fired 0/50; a model passed its own output 200/200.

If you find yourself writing a prompt that both generates and judges, stop — that is the
defect this entire phase exists to remove.

---

## Build

### 1. Floor, in front of everything

In `getRoadmapOnboardingDraft`, before any generation:

- Run the floor service (already built, merged) against the session row:
  `{outcome, project, top_priorities, recommended_action, what_not_to_do}`.
- **SUFFICIENT** → proceed to generation.
- **INSUFFICIENT** → return the halt shape (`verdict`, `reason`, `question`) instead of
  a draft. Log to `refusals` (`kind='floor_insufficient'`). The route must be able to
  return "no draft, here is one question" as a first-class response — not an error, not a
  500.
- The user's answer to the floor question appends to the session's `gap_answers` paired
  array (so every downstream reader sees it), and re-runs the floor.
- **`outcome` may be null** — that is a legitimate Gate 1 INSUFFICIENT, not a bug.

### 2. Stage 4 — real generation, server-side

Replace `buildThemeDrafts` / `buildTodoDrafts` entirely. Delete them, and delete
`canStandAsTheme`.

Follow `resolve-pipeline/04-extract-themes.md`. The sequence is **separate calls**:

1. **Generate** candidate themes from the session data (model call).
2. **Classify** each candidate — a SEPARATE isolated call. Never classify inside the
   generation call (130/130 failure).
3. **Filter against `what_not_to_do`** — this field is finally READ. It is in the session
   row. Anything the founder said not to do is excluded.
4. **Choose one** and produce a candidate definition-of-done.
5. **Referee it** (already built): the three theme clauses + two DoD clauses, isolated
   calls, AND rule, regenerate-with-reason loop capped at N=2. Log every rejection to
   `refusals` (`kind='referee_reject'`).

**`top_priorities` and `what_not_to_do` are UNVALIDATED upstream and can arrive empty
silently** (`validateRecommendationShape` doesn't require them; the frontend papers over
it with fallbacks). Treat both as possibly-absent. Do not assume presence. Do not invent
content to fill a gap — if there is nothing to work with, that is a floor problem, not a
generation problem.

### 3. Referee behind `check-theme`

Replace `canStandAsTheme`'s internals in `/api/roadmap-onboarding/check-theme` with the
referee's **theme clauses** (LEVERABLE, NO_FINISH_LINE, DISTILLED). Same request/response
contract. **Never route on a single clause's verdict** — EVIDENTIAL has a measured ~40%
wobble in isolation and is only safe under the AND rule.

### 4. Stage 6 — assembly is DETERMINISTIC, not generated

Replace `confirmRoadmapOnboarding`'s VALUES (keep its structure and rollback):

- **Titles:** self-completing, from the real theme. Not `"{action} for {theme}"`.
- **Definition of done:** the referee-approved DoD → `initiatives.definition_of_done`
  (column exists).
- **Priority:** derived from Now/Next/Later sequence. **Not hardcoded `'medium'`.**
- **Dates:** derived from the sequence — **never generated by a model.**
  `work_items.sequence` and `due_date` already exist; use them.
- **`initiatives.inferred_from`:** the provenance of each theme (which session fields it
  came from).
- **`initiatives.provisional`:** **COMPUTED in code from `inferred_from`. Never
  self-reported by a model.**
- **The cut** (rejected themes): a row in `knowledge_base_items` for the user-visible
  version, plus a `refusals` row (`kind='cut'`) for audit. Surfaced as a quiet pointer to
  the Knowledge Base — **not listed inline.**

### 5. The refusal screen (only if 1.3's mockup exists)

Wire the halt shape to the screen. Pixel-exact to the mockup. If the mockup isn't
delivered, stop after step 4 and say so.

---

## Proof — not self-certified

- Run a **real intake** end to end through the preview: Intake → handoff → floor →
  generation → referee → R2 → confirm → DB.
- Show the **real `project_sessions`, `initiatives`, `work_items`, and `refusals` rows**
  from the live DB. **Query them and paste them.**
- Verify against a **real non-fixture project** — the canonical test project
  (`c0a10000-0000-4000-8000-000000000001`) and/or a fresh real intake.
  **`synthetic_intakes.json` does not exist and will NOT be recreated. Do not fabricate
  test intakes.**
- Confirm: no hardcoded `'medium'` priority anywhere; no null dates where a sequence
  exists; `what_not_to_do` demonstrably filtered something (or explain why it didn't);
  at least one `refusals` row written.
- **Do not grade your own output.** Report what happened; the reviewer decides.

## Do not

- Do not touch the Intake repo.
- Do not merge generation and enforcement into one call, for any reason.
- Do not tune the floor or referee criteria — the floor's criterion is known-imperfect and
  is a logged Beta spike (`FLOOR-GATE-DESIGN-FINDING.md`). Ship it as-is; it logs its own
  evidence.
- Do not touch production until 1.6's blind-graded acceptance passes. Branch-first,
  verify on preview.
