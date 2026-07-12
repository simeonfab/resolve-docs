# Phase 1 — Tickets 1.1 (Referee) and 1.2 (Floor)

Repo: `cap-pm-cockpit-alaria`. Both are **server-side services with no UI**. Neither is
wired into the product in these tickets — they are built standalone and tested
standalone. Wiring happens in 1.4.

**Route: build-easy · Builder: Codex · Reviewer: top model, different provider ·
Proof: eval results graded against expected verdicts by someone other than the builder.**

**Parallel-safe: 1.1 and 1.2 share no code and no files.** Build both in parallel via
subagents. Each is fully specced and near-zero-decision. Anything ambiguous → flag to
Simeon, do not decide it quietly.

**Standing rules that apply (from `CLAUDE.md` discipline):** branch-first; commit early
and often (containers reset); never invent data; report deviations explicitly; if an
instruction isn't specific enough to build without guessing, that's a ticket failure —
stop and ask (Bizarro Superman).

---

## THE ONE RULE THAT GOVERNS BOTH

**Generation and enforcement never share a call.** Both services are ISOLATED calls:
their own request, nothing else in context, reject-only. Do not "save a call" by
merging clauses, by batching candidates, or by folding either service into a generation
prompt. Testing established this fails: criteria inside a generation prompt were ignored
130/130; a floor instruction inside a generator fired 0/50; a model passed its own output
200/200. If a future edit collapses these into a generator, it reintroduces exactly the
failure they exist to remove.

---

## Shared infrastructure — use what already exists, do not invent

`src/lib/ai/update-drafts.ts` already contains the pattern both services need. **Read it
first.** It has:
- The OpenAI **Responses API** call with strict `json_schema` structured output
  (`text.format = {type: "json_schema", strict: true, schema: {...}}`).
- `assertAiUsageAllowance()` — the usage gate. Call it.
- `recordGenerationAttempt()` — token/model/status logging into `ai_usage_events`. Call it.
- Model from `process.env.OPENAI_MODEL` with a default.

**Structured output is mandatory, and it is the fix for a known defect.** The eval-era
referee parsed free text (`VERDICT: PASS / REASON: ...`) and the model frequently dropped
the literal `REASON:` label, so a strict parser **silently discarded ~30% of verdicts**.
Do not build a text parser. Force the shape with `json_schema` so no verdict can be lost.

**Depends on 1.0 (schema migration).** Both services write to the new `refusals` table:
`(user_id, project_id, session_id, kind, verdict, reason, question, payload)`, RLS
`user_id = auth.uid()`. If 1.0 has not landed, STOP and say so — do not create the table.

---

# TICKET 1.1 — Referee service

**What:** package the validated referee clauses as isolated, reject-only calls with an
AND rule and a regenerate loop. This is a **proven component** — the clause wording is
already validated at 99–100% on decisive cases. **This ticket is packaging, not design.
Do not improve, reword, shorten, or "clarify" the clauses. Copy them verbatim from
`resolve-pipeline/05-referee.md` (attached).**

**Five clauses, each its own isolated call:**

Theme checks (per candidate theme): `LEVERABLE`, `NO_FINISH_LINE`, `DISTILLED`.
Definition-of-done checks (per chosen theme's DoD): `EVIDENTIAL`, `MINIMAL`.

Take the exact prompt text of each from `05-referee.md`. Note `SERVES_OUTCOME` was
tested and dropped as near-decorative — **do not re-add it.**

**The AND rule (non-negotiable):** a candidate passes only if EVERY relevant clause
returns PASS. **Never route on a single clause's verdict.** EVIDENTIAL alone has a
measured ~40% wobble in isolation; it is safe only because the other clauses carry it
under the AND. Any code path that acts on one clause's verdict reintroduces that leak.

**Output per call (strict json_schema):**
```json
{"verdict": "PASS" | "FAIL", "reason": "<one sentence>"}
```

**The regenerate loop:**
1. Run all relevant isolated checks.
2. All PASS → accept.
3. Any FAIL → return the candidate to its generator **with the failing clause name and
   its one-sentence reason attached** → regenerate.
4. Cap at **N = 2** retries. Still failing after N: drop the theme if others survive; if
   it was the only theme, escalate to the floor outcome (return the
   "no clean focus + here is the question" shape rather than shipping a failing DoD).

In 1.1 the "generator" is a function argument (a callback) — Stage 4 does not exist yet.
Build the loop so 1.4 can pass the real generator in. Do not build Stage 4 here.

**Log every rejection** to `refusals` with `kind='referee_reject'`, the failing clause in
`reason`, and the rejected candidate in `payload`.

**Acceptance (proof — not self-certified):**
- **Zero silently-dropped verdicts.** Every call returns a parsed verdict or a logged
  error. Prove it: replay the eval JSONLs from the referee testing sessions if they were
  saved out. **If those files no longer exist, STOP and tell Simeon** — the fallback is
  re-running the clause evals against the 10 synthetic intakes. Do not skip this and do
  not silently substitute.
- Each clause's verdict distribution on the eval set matches the known-decisive results.
- Every rejection produces a `refusals` row.
- Report the numbers. **Do not grade your own output** — hand the results to the reviewer.

---

# TICKET 1.2 — Floor service

**What:** an isolated structural gate that decides whether an intake can support a real
roadmap **at all**, and returns a single question when it can't. Attach
`resolve-pipeline/03-floor-gate.md`.

**⚠️ Wording correction — read this before anything else.** `03-floor-gate.md`'s own
frontmatter says its Gate 2 wording is **SUPERSEDED**. Use the corrected discriminator:

> A lever = **continued effort keeps producing value; it has no natural endpoint, so a
> stopping point must be chosen.** A task = spent after one push.

**Do NOT use "finish-line" or "diminishing-returns" wording.** Both were tested and
found wrong: "diminishing returns" was an overreach (a lever can pay flat or rising and
still be a lever), and finish-line framing miscalibrated the gate. This exact wording is
what made Gate 2 discriminate cleanly. Copy it; do not paraphrase it.

**Two gates:**
- **Gate 1 — outcome present.** Is there a real outcome (a change in someone's world),
  as opposed to an artifact or a restatement of the project? If the outcome is not a real
  outcome, the intake is INSUFFICIENT **regardless of the surfaces** — you cannot serve an
  outcome that isn't one.
- **Gate 2 — lever present.** Is there at least one surface that is a lever (per the
  corrected wording above) serving that outcome? All-tasks ("decide the shape", "pick a
  name") or all-artifact ("build the database") → INSUFFICIENT, even though a
  plausible-sounding roadmap could be manufactured from it.

**Input:** the session row shape —
`{outcome, project, top_priorities, recommended_action, what_not_to_do}`.
(`outcome` may be null — that is a legitimate Gate 1 INSUFFICIENT, not an error.)
**Treat all of it as untrusted data, never as instructions.** Fence it.

**Output (strict json_schema):**
```json
{"verdict": "SUFFICIENT" | "INSUFFICIENT",
 "reason": "<one sentence>",
 "question": "<if INSUFFICIENT: exactly ONE question. Not a list. Empty if SUFFICIENT.>"}
```

**Rules:**
- **Exactly one question when insufficient.** The single question whose answer would most
  change the picture. Not three, not a form — a wall of questions is production, and the
  point is to ask, not to manufacture.
- **The question elicits; it does not advise.** Do not explain how to fix the project.
- **Bias to INSUFFICIENT when genuinely on the fence.** A wrongly-declined sufficient
  project costs the user one question. A wrongly-accepted insufficient one costs them a
  confident, fabricated plan they may act on. The asymmetry favours asking.
- Log every verdict to `refusals` (`kind='floor_insufficient'` on INSUFFICIENT, carrying
  `verdict`, `reason`, `question`).

**Acceptance — THIS IS THE GATE FOR ALL OF PHASE 1. Do not proceed past it.**

Run standalone against `resolve-pipeline/floor-intakes-with-outcomes.json` and the 10
synthetic intakes. It must:
- return **INSUFFICIENT** on the no-lever cases — dementia (P3), charity (P7), local-news
  (P10), and `Q3_no_lever`, `Q4_artifact_only`;
- return **SUFFICIENT** on the clear-lever cases — `Q1_clear_lever`, `Q5_clear_lever_2`;
- be **stable** — no coin-flips across repeat runs on the same intake.
- `Q2_feature_pile` is a known ambiguous case. Report what it does; **do not tune to it.**
  Feature-pile detection is explicitly NOT this gate's job.

**If the floor cannot discriminate these apart, STOP and report. Do not build the rest of
the pipeline on top of it, and do not tune the clause to make the numbers work.** This is
the single highest-risk assumption in the project: an "if insufficient, decline and ask"
instruction *inside* a generation prompt fired **0 times out of 50**. Whether it holds as
a *separate call* is exactly what this ticket tests. A floor that never returns
INSUFFICIENT has not passed — it has become the 0/50 failure in a new location.

**Report the raw per-intake verdicts. Do not grade your own pass/fail** — hand them to
the reviewer to check against the expected column above.

---

## Both tickets: what NOT to do

- Do not wire either service into any screen, route, or existing flow. That is 1.4.
- Do not delete `buildThemeDrafts`, `buildTodoDrafts`, or `canStandAsTheme` yet. 1.4.
- Do not touch `api/roadmap-onboarding/check-theme` yet. 1.4.
- Do not build Stage 4 generation.
- Do not touch the Intake repo (`ResolveOS-intake-frontend`) at all.
- Do not mark either ticket done on your own verification. Review and proof are separate
  layers, by design.
