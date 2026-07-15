# WALK 2 — FIX PLAN

**Both providers (Codex + Claude Code) independently agreed on every root cause.** Where they
differ, Claude found MORE. Nothing below is a guess; every item cites the evidence.

**Sequencing principle: answer the model question FIRST.** Three of the five findings are
downstream of it. Fixing them before we know whether the criterion is broken or merely badly
served would mean fixing the wrong thing — possibly twice.

---

## P0 — THE MODEL QUESTION *(1 hour, blocks P1)*

**The whole pipeline runs on `gpt-4.1-nano`** — classifier, referee, task judges, floor. Every one
of the 31 calls.

**The evidence that it matters:**
- The same theme was rejected `NO_FINISH_LINE` for *"being a TASK, not a THEME"* and, on the next
  attempt, for *"implying an ongoing process rather than a finished state."* **Opposite verdicts,
  same axis, same input.**
- Two `DISTILLED` verdicts flatly contradict each other (*"closely mirrors the intake"* vs *"not a
  near-verbatim copy"*).
- The classifier cut **all three** candidates (TASK/TASK/OUTCOME → zero levers), which is what
  forced the degrade path that blanket-promoted every theme as provisional.

**THE TESTS — do these before touching a prompt:**

**P0.1 — What model did the validation eval run on?** The ~2,880-call eval that locked EVIDENTIAL
and MINIMAL scored **92–95% pass**. Production is producing contradictions. **If that eval ran on a
stronger model than nano, we validated a criterion on one model and shipped it on another — a
regression we caused ourselves.** Answerable from the repo today. **Find it.**

**P0.2 — Replay the same intake on a strong model.** Same prompts, same criteria, project
`725384bf`, swap `OPENAI_MODEL` only. Then:
- **Verdicts become coherent** → the criterion was partly fine and **F33 was partly a routing bug**.
  We've spent four failed prompt passes blaming a concept for a model's limits.
- **Verdicts stay contradictory** → the criterion really is wrong. F33 stands as filed, and it is
  no longer a Beta spike — it is blocking Alpha.

**P0.3 — The EVIDENTIAL/MINIMAL deadlock.** Independent of model. In the real run:
- EVIDENTIAL rejected a DoD for lacking proof → the generator **added a survey**
- MINIMAL then rejected it because **the survey was "removable"**

**MINIMAL rejected exactly what EVIDENTIAL demanded.** Under the AND rule both must pass, so the
loop oscillated and gave up. Decide: are these two clauses reconcilable, or does MINIMAL need to be
scoped so it cannot strip the evidence EVIDENTIAL requires?

---

## P1 — TASK SEQUENCING *(the bug a user feels; both audits, identical root cause)*

**`work_items.sequence` and `due_date` have READERS BUT ZERO WRITERS.** Nothing in `src/` ever
writes them.

**Where phase dies:** `allowedNow` / `nextCandidates` are computed
(`roadmap-generation.ts:1031-32`), then **flattened into one array of `{title, theme_index}`** at
`:1054-56`. Phase is discarded **at assembly**, one layer above persistence. The todo draft type
(`roadmap-onboarding.ts:63-67`) has no phase field, so the insert *couldn't* write it even if it
tried. Per-task `priority` is hardcoded `'medium'` at insert (`:655`).

**Why the Work tab shows 12 in "Now":** it never looks at `sequence` or `due_date`. "Now" =
`confirmation_state='confirmed'` AND `status IN (todo, in_progress)` (`spine.ts:244-262`).
"Upcoming from roadmap" = `draft`/`suggested` (`:345-364`) — **permanently empty**, because confirm
inserts every task as `confirmed` + `todo`. So "everything is Now" is a **classification artifact**,
not a null-default.

**Knock-on:** `dueToday()` filters on `due_date`, which has no writer — so the **Dashboard "Today"
card can never populate, ever.** Not "you have nothing today"; structurally incapable.

**FIX:**
1. Carry phase through assembly — the todo type gains a phase/sequence field; stop flattening.
2. Write `sequence` at insert, and **derive `due_date` from it** (N7: dates are derived from the
   Now/Next/Later sequence, **never model-generated**). Theme-level derivation already works
   correctly — mirror it at task level.
3. Fix the Work tab's grouping so Now/Next/Later mean what the spec says, and "Upcoming from
   roadmap" can actually populate.
4. Stop hardcoding per-task `priority: 'medium'`.

**A first week is 3–5 tasks, not 12.** Decide whether Now is capped.

---

## P2 — THE HANGS *(31 calls, ~50s measured from `ai_usage_events`)*

| Stage | Calls | Wall-clock |
|---|---:|---:|
| Candidate generation | 1 | ~5s |
| Classify | **3 serial** | 4.7s |
| Choose focus | 1 | 2.3s |
| DoD referee ×3 attempts | **15 serial** | ~19s |
| DoD regeneration | 2 | 4.2s |
| Task generation | 1 | 6.2s |
| Task judges | **8 serial** | 12.6s |
| **Total** | **31** | **~50s** |

**P2.1 — Parallelise the independent batches.** 26 of the 31 calls sit in sequential `for` loops
over **independent** work: classify (`roadmap-generation.ts:747`), the 5 referee clauses per attempt
(`referee.ts:225-228`), the 8 task judges (`:1038-42`). Nothing depends on the previous result.
**Parallelising these changes no verdict and collapses the wall-clock.**
⚠️ Isolation is preserved — parallel ≠ bundled. **N1 still holds: never put a judgement and a
generation in one call.** Separate calls that run concurrently are still separate calls.

**P2.2 — The DoD loop re-litigates verdicts it cannot fix.** Each attempt re-judges **all five
clauses**, but regeneration can only rewrite the **DoD** — so the theme clauses (LEVERABLE,
NO_FINISH_LINE, DISTILLED) get re-judged on every round and can never be fixed by the thing being
regenerated. **~15 of the 31 calls were re-litigating an unwinnable verdict.** Only re-judge the
clauses a regeneration can actually affect.

**P2.3 — The 30-second "skeleton" is NOT A LOADER.** It is the `ExitTransition`'s **static
facsimile** (`exit-transition.tsx:134-190`) — hardcoded "Theme 1 of 3", grey bars, and
**button-styled `<div>`s that aren't controls**. It calls `window.location.assign('/roadmap')` at
4.2s; `/roadmap` then blocks on the **entire pipeline** (`page.tsx:884`) with no `loading.tsx` and
no Suspense. **The browser sits frozen on the transition's last frame for the full ~50s.** The user
is staring at fake buttons.

**P2.4 — Build the narrating loader. There is currently NOT ONE IN THE REPO.** The spec is binding:
*"Every processing wait = amber spinner ring + a checklist that ticks off named stages. **Never a
bare spinner.**"* Today: a grey pulse, bare button text ("Saving your roadmap…"), and static
facsimile text. Nothing narrates. **This is the single worst moment in the product** and it's the
one place the spec's own rule was ignored.

**P2.5 — Confirm re-authenticates on every insert.** The API log shows a `GET /auth/v1/user`
before **each** of the 15 POSTs — 30+ sequential round trips, **~6.6s** server-side. Batch the
inserts; authenticate once.
*(Note: the perceived ~15s exceeds the measured 6.6s. The residual is unexplained and needs a
client-side timing of the confirm `fetch` to settle. Do not assume.)*

---

## P3 — THE PROVISIONAL FLAG *(downstream of P0 — do not fix before it)*

All three themes came back `provisional: true`. **Gate 2 never even ran** — the floor is skipped
entirely when `outcome_confirmed = true` (`roadmap-onboarding.ts:398`). There is no
`floor_gate2_diagnostic` row.

What actually happened: the **generator's classifier** (which embeds the same F33 lever criterion,
`roadmap-generation.ts:126, :206`) cut all three candidates → the theme pool emptied → the
**degrade path blanket-promoted all three** as provisional (`:826-839`).

**So our mental model was wrong on two counts.** The criterion doesn't only live in the floor — it
lives in the classifier, where nothing contains it. And "Gate 2 flags rather than refuses, so the
damage is contained" was **false**: the same broken axis rejects every candidate one layer earlier.

**Consequence: if every theme on every project is provisional, the marker means nothing.**

**Do not touch this until P0 answers whether the criterion or the model is at fault.**

**Also:** *"Expand membership beyond immediate network"* — the founder's own stated goal, handed
back to him as a theme — **was never seen by the referee.** The classifier cut it as TASK; the
degrade path promoted it. The DISTILLED clause that exists to catch exactly this **never ran on it.**

---

## P4 — DEFINITIONS OF DONE

**By design, at two levels:** generation writes a DoD **only for the chosen focus theme**
(`roadmap-generation.ts:881`), and `roadmap-onboarding.ts:476` **hardcodes `null`** for Next themes.
The canonical `06-assemble.md` schema agrees. **So this is a design gap, not a silent failure.**

**The real anomaly is the opposite of what it looks like:** the "good" DoD shipped on the Now theme
is **the final referee-REJECTED candidate** (`:933` keeps the last non-null DoD regardless of loop
status) — shipped with `provisional: true` per "mark provisional, never ship nothing".

**DECISION NEEDED (Simeon):** *"Would you know when it was met?"* is half the product. Do Next
themes get a DoD? Today, two themes in three ship without one.

**Inconsistency to fix regardless:** the client backfills a **placeholder DoD** when a theme is
corrected/renamed (`form.tsx:86-88`) — so a corrected Next theme gets a synthetic DoD while an
untouched one stays null.

---

## P5 — SMALL BUT REAL

**A judge failed and the task shipped anyway.** The final task-judge call **failed**
(`status=failed`, 18:41:08) and the code **fails open** — so **one of your 12 tasks was never
judged.** Fail-open on a judge is the wrong default: an unjudged task is precisely what the judge
exists to prevent. Fail closed, or retry, or mark it — but do not ship it silently.

---

## THE ORDER

1. **P0** — the model question. Cheap, and three findings hang off it.
2. **P1** — task sequencing. The bug a user feels, and it's a clean, well-understood fix.
3. **P2** — parallelise + the narrating loader. Biggest felt improvement per hour.
4. **P3** — the provisional flag. **Only after P0.**
5. **P4/P5** — decisions + the fail-open judge.
