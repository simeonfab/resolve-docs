# RESOLVE — SESSION CODIFICATION (14–15 July 2026)

**What this is:** the durable record of a long working session. Read this first; it points to
every other doc produced. Where it contradicts an older doc, THIS wins (it is newer).

---

## THE ONE-PARAGRAPH STATE OF PLAY

There was no incident (the "killed everything" chat lost its own uncommitted work). cap-pm and
Intake are intact. **Phases 0 and 1 are merged and live in production:** the rebuilt pipeline,
project-scoped queries, task generation, draft persistence, the report PDF, outcome-content
validation, and the fail-silent-handoff fix. **The pipeline now works end to end** — a real intake
(Business Community) produced 3 themes, 12 tasks, a good PDF, and held still on reload. **But two
walk-throughs exposed that the CORE JUDGEMENT is broken**, and that finding reframed the whole
project: we are no longer trying to make a machine auto-certify what a good theme is. **Decision
taken: put a human in the loop.** Phase 2 (the 16 screen tickets) has NOT started and is now
partly on hold pending the roadmap-format and elicitation decisions below.

---

## WHAT SHIPPED THIS SESSION (all in production unless noted)

- **Phase 0** (cap-pm `main` `2de51b2`, resolve-docs `d1577ae`): merged the rebuilt pipeline +
  query scoping; killed the stale specs that kept resurrecting the banned halt screen (F13);
  deleted 6 out-of-spec routes.
- **Phase 1 backend** (cap-pm, merged): task generation → real `work_items`; draft persistence
  (proven working); report PDF via `pdf-lib`; demo dataset unified. Intake (merged, live):
  outcome-answer content validation; fail-silent handoff fixed.
- **A production config fix:** `OPENAI_API_KEY` was scoped to Preview only, not Production — every
  model call was short-circuiting. Now fixed. `AI_TESTER_LIFETIME_GENERATION_LIMIT` raised.
- **A schema-drift record** committed (the retrospective migration; `refusals_kind_check` widened
  live so Gate 2 diagnostics can actually be written).

## THE TWO BIG FINDINGS (why the project pivoted)

**1. The theme referee is broken, proven on a strong model.** A controlled replay (same intake,
same prompts, only the model changed, 3× each) showed: `gpt-4.1-nano` breaks the CLASSIFIER (calls
every candidate a task) — a routing bug; but `gpt-4.1` FIXES the classifier and STILL fails
certification 9/9, because the criterion is self-contradictory (passes LEVERABLE for being ongoing,
fails NO_FINISH_LINE for having an endpoint — same theme, same moment). **A better model cannot save
an incoherent criterion.** Five attempts across every variable have now failed. Full detail: F33
brief. The "92% validation eval" everyone cites **cannot be found in any repo** — treat the criteria
as unproven.

**2. Decision: human-in-the-loop.** Stop trying to auto-certify theme quality for Alpha. The
pipeline proposes; the founder confirms/adjusts (the "Not quite" path). Rationale: people enjoy
being interviewed about their own work; the elicitation IS the product, and the reveal is the
payoff. This dissolves F33 as an Alpha blocker and relocates the hard problem from "judge the
output" to "design the questions." The automated criterion becomes a background research track.

## OPEN — DECISIONS NOT YET MADE

- **Is Now/Next/Later even the right roadmap format?** (Raised at the very end; never examined. It
  has been an unexamined assumption in the pipeline, the mockups, and the sequencing bug. **This is
  the subject of the next chat.**)
- **What is a "good roadmap"?** — the research spike (baseline question out to senior PMs).
- **What questions produce one?** — the elicitation redesign, downstream of the spike.
- **Do Next themes get a definition of done?** (Currently only the focus theme does — by design.)
- **Per-stage model routing** — the classifier must come off nano; how far to route strong.

## THE DOCUMENT MAP (everything produced this session)

| Doc | What it is | Status |
|---|---|---|
| `RESOLVE-AUDIT-REGISTER.md` | Every finding F1–F33 from the 13–14 July audits, with evidence | the evidence base |
| `ALPHA-BUILD-TICKETS.md` | THE build authority — 16 screen tickets, behaviour inline | Phase 0/1 done, Phase 2 pending |
| `ALPHA-EXECUTION-PLAN.md` | The phase plan (0→3) this session executed against | Phases 0,1 done |
| `SCREEN-CONTRACT-TEMPLATE.md` | The per-screen acceptance artifact (element/data/state) | for Phase 2 |
| `AUDIT-BRIEF-INTAKE.md` | The twice-run Intake audit brief | ready to run |
| `WALK-2-FINDINGS.md` | The 2nd walk-through's findings F34–F38, know/don't-know split | diagnosed |
| `WALK-2-FIX-PLAN.md` | Root causes (both providers agreed) + fix sequence P0–P5 | P0 done |
| `F33-REFEREE-REDESIGN-BRIEF.md` | The proven-broken criterion; the human-in-loop direction | **critical path** |
| `SPIKE-what-is-a-good-roadmap.md` | The research spike + baseline question for senior PMs | in progress |
| `BETA-BACKLOG-additions.md` | Latency spike, per-stage routing, fail-open judge, missing eval | parked |
| this doc | the session tie-together | — |

## THE THREE STANDING TRUTHS (do not relearn a fourth time)

1. **A decision that lives only in prose loses to an instruction in the repo.** The banned halt
   screen was IN the spec; the builder followed it faithfully. Propagate decisions to the FILES.
2. **"Built" ≠ "reachable and working".** The floor, the halt screen, N9, task sequencing — all
   built, none reachable/working when checked. Acceptance requires a CLICK, not a build.
3. **Never bundle a judgement with a generation in one call. Fires 0/50.** Re-learned repeatedly.
   (Corollary from this session: never let the cheapest model make the product's core judgement.)
