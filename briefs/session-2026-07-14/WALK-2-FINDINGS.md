# WALK 2 — FINDINGS (14 July, project 725384bf)

**The headline: the pipeline works.** With `OPENAI_API_KEY` finally scoped to Production, a real
intake produced 3 themes, 12 tasks, 11 refusal rows, a good PDF, and a roadmap that held still
on reload. Nothing was invented. Simeon's verdict on the tasks: *"the actions are good… they are
reasonable tasks, and they make sense."* **That is the first time the product has done its job.**

Five findings below. Each is split into **what we KNOW** (observed / queried) and **what we DON'T**
(the thing the diagnostic must establish). No fixes proposed until the second column is filled in.

---

## F34 — TASKS ARE NEVER SEQUENCED INTO NOW / NEXT / LATER  🔴 the one a user feels

**KNOW:**
- All 12 `work_items`: `sequence = NULL`, `due_date = NULL`, `status = 'todo'`. Every one.
- The Work tab renders **all 12 under "Now"** — "0 of 12 done this week", "Now · 12 items".
- The themes DO carry priority (`high` / `medium` / `medium`) and the tasks DO carry
  `initiative_id` correctly (8 / 2 / 2 across the three themes).
- T1.1's own report said generation buckets candidates into `allowedNow` and `nextCandidates` —
  so a now/next split **exists at generation time**.
- The roadmap grid (§10) rendered Now / Next / Later columns with "Nothing yet" in Next and Later.

**DON'T KNOW:**
- Does `work_items` even *have* a phase column? (It has `sequence`, added by the retrospective
  migration — is that the intended carrier?)
- Is the generation-time `allowedNow` / `nextCandidates` split **dropped at persistence**, or was
  it never assigned in this run?
- What does the Work tab actually **group by**? If it buckets on `due_date` or `sequence` and both
  are null, "everything is Now" would be a null-defaulting artifact, not a decision.
- `T1.1` derives dates from the Now/Next/Later **sequence** (N7: dates are never model-generated).
  If sequence is null, **no date can ever be derived** — is that the same bug or a second one?

**Why it matters:** 12 tasks in "Now" is not a first week, it's a backlog. The Work tab's promise
is *"the first concrete moves, in the order they matter."* There is currently no order.

---

## F35 — 30-SECOND AND 15-SECOND HANGS, WITH NON-NARRATING LOADERS  🔴 user-facing

**KNOW:**
- After sign-in, before the theme cards: a **skeleton card** ("Theme 1 of 3", grey bars, live
  buttons) hung for **~30 seconds**.
- On the end card: **"Saving your roadmap…"** hung for **~15 seconds**.
- A single roadmap generation fires **~15–25 sequential model calls** (1 candidates + up to 6
  classify + 1 choose + up to 2 DoD + 1 task-gen + up to 10 task-judge).
- The build spec is explicit: *"Loaders narrate. Every processing wait = amber spinner ring + a
  checklist that ticks off named stages. **Never a bare spinner.**"*
- The 15s save is very likely **T1.2's new `await`** on the confirmation write — correct behaviour
  (it killed the partial-write race) with **no feedback**.

**DON'T KNOW:**
- Is 30s the *generation* itself, or generation + something avoidable (a serial call that could be
  parallel, a retry loop, an over-eager task-judge running 10 sequential judgements)?
- What is the actual call count and wall-clock breakdown per stage on this run?
- Is the skeleton card the R2 loading state, or a component rendering before its data resolves?
- Why does the confirm write take 15s — 12 inserts should be fast. Is it looping single inserts
  rather than batching?

**Why it matters:** 30 seconds of a dead skeleton with two live-looking buttons is the worst
moment in the product. And it's the one place the spec's own rule was ignored.

---

## F36 — `roadmap_draft` IS NULL  ⚠️ I MAY HAVE MIS-CALLED THIS

**KNOW:**
- `project_sessions.roadmap_draft` is **NULL** for this project.
- The column exists and the migration is applied.
- **BUT:** T1.2's builder explicitly said the draft is *"cleared on confirm and reset."* Simeon
  **confirmed** the roadmap. So a null draft post-confirmation may be **exactly correct**.
- Reloads before confirming **did** produce an identical roadmap, which is what T1.2 promised.

**DON'T KNOW:**
- Was a draft ever written at all? (Post-confirm state can't tell us — it would have been cleared.)
- **This is directly testable:** run an intake, stop BEFORE confirming, and query
  `roadmap_draft`. If it's populated, T1.2 works and this finding closes. If it's null, T1.2
  never wrote, and the identical reload was a coincidence of a different mechanism.

**I called this a failure too quickly. It is unproven, not broken.** Establish it before touching it.

---

## F37 — EVERY THEME IS `provisional: true` 🟡 the product question, not a bug

**KNOW:**
- All 3 themes: `provisional = true`, even with a good, content-validated outcome
  (`outcome_confirmed = true`).
- 11 refusal rows exist for this project.
- Doctrine: **Gate 2 firing means our INTAKE QUESTIONS are wrong. Fix upstream, never at the gate.**
- One theme is *"Expand membership beyond immediate network"* — which is close to Simeon's own
  stated goal handed back to him. That is the thing we said should NOT happen.

**DON'T KNOW:**
- **What do the 11 refusal rows actually say?** They are now the evidence base we deliberately
  deferred the Sharpen Loop to wait for. Nobody has read them.
- Is Gate 2 flagging because the intake genuinely has no lever, or because the **floor's criterion
  is the known-wrong one** (F33 — "lever = no natural endpoint", a Beta spike)?
- Is `provisional` being computed from `inferred_from` (per N7) or self-reported?

**Why it matters:** if EVERY theme on EVERY project is provisional, the marker means nothing.

---

## F38 — TWO OF THREE THEMES HAVE `definition_of_done: NULL` 🟡

**KNOW:**
- The Now theme (priority `high`) got a real, **evidential** DoD: *"Participants attend the event,
  and at least 50% report increased clarity about their career paths in a post-event survey."*
  That is a genuinely good DoD — it evidences the outcome, not an artifact.
- The other two themes: `definition_of_done = NULL`.

**DON'T KNOW:**
- Is the DoD **only generated for the Now theme by design**, or did generation fail for the other
  two and fall through silently?
- The pipeline has a DoD regenerate loop (up to 2 attempts) — did it run and give up, or never run?
- If a DoD can't be certified, the design says the theme should be marked provisional, **not**
  shipped with a null. Is null a silent failure?

**Why it matters:** *"Would you know when it was met?"* is half the product. Two themes ship
without an answer.

---

# THE DIAGNOSTIC — read-only, establish causes, propose nothing yet

Run this before any fix. **Two independent passes** (Codex + Claude Code) — that method has already
caught two bugs a builder swore were fine.
