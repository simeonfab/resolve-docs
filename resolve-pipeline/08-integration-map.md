---
status: current
note: "[INFERRED] sections pending verification via Ticket 0.2 (read the actual intake generation calls; change nothing)."
---

# 08 — Integration Map: new pipeline → current Resolve intake

This is the missing layer between the module drafts (01–07) and the system that
actually exists. It states what each module replaces or augments, how outputs map onto
the live `project_sessions` schema, where the intake *process* changes, and — honestly
— which parts of this are grounded in code I've read versus inferred.

## Confidence labelling (read this first)

- **[SCHEMA]** — grounded: the `project_sessions` columns are confirmed from the live DB.
- **[INVESTIGATED]** — grounded: the roadmap side (`roadmap-onboarding.ts`, `initiatives`,
  `work_items`, the R2/R3/R4 screens, the confirm action) was read in the earlier
  source investigation.
- **[INFERRED]** — NOT grounded: the intake *generation* calls (the ~4 OpenAI calls that
  produce `project_understanding`, `recommended_action`, `top_priorities`,
  `what_not_to_do`) have NOT been re-read. Treat every claim about their internal
  structure as a hypothesis to verify against the code before building.

## The current system (as understood)

**Intake (Intake repo, static site) [INFERRED for call internals]:**
Multi-screen discovery → ~4 sequential OpenAI calls (role/governance prefix injected,
per commit 5e0f6f5) → writes `project_sessions`:

| column [SCHEMA] | type | current meaning [INFERRED] |
|---|---|---|
| project_understanding | jsonb | {goal, state, project, assumptions, uncertainty, ...} |
| recommended_action | text | single suggested next step |
| top_priorities | jsonb | list of priorities |
| what_not_to_do | jsonb | [{item, reason}] — **captured, never read downstream** |
| confirmed_tools | jsonb | tools the user confirmed |
| gap_answers | jsonb | answers to gap questions |
| project_plan_report | text | the narrative report |

**Roadmap (cap-pm-cockpit-alaria) [INVESTIGATED]:**
`getRoadmapOnboardingDraft` reads latest `project_sessions` → `buildThemeDrafts`
(string extraction, no model) → R2 theme cards → confirm action at 3rd card →
`confirmRoadmapOnboarding` inserts `initiatives` + `work_items`. **Zero model calls.
Priority hardcoded 'medium'. No due dates. Titles = `{action} for {theme}`. Themes
copied verbatim.** This is what the pipeline replaces.

## Module-by-module mapping

### Stage 1 Elicit → Intake discovery screens [INFERRED]
Replaces/augments the front intake screens. The key ADDITION is the **outcome anchor**
— today's intake captures goal/state/project but NOT "if this works, who is better off."
A1 testing showed the outcome adds nothing at *generation* but is *required by the
referee*. So: Elicit must produce and confirm an `outcome`, stored as a NEW field.
→ **Schema change:** add `outcome` (text) + `outcome_confirmed` (bool) to
`project_sessions`, OR nest under `project_understanding.outcome`. Prefer a top-level
column so downstream reads are explicit.

### Stage 2 Structure → the intake generation calls [INFERRED]
Maps onto whichever call currently produces `project_understanding` + `what_not_to_do`.
ADDITION: each surface must carry `inferred_from` (source span) and a `provisional_class`.
Today's output has no per-surface attribution.
→ **Schema change:** `project_understanding.surfaces` becomes
`[{text, provisional_class, inferred_from}]` rather than loose prose. `what_not_to_do`
gains `reason_source: stated|inferred`.
→ **VERIFY FIRST:** read the actual intake call that writes these columns before changing
its output contract.

### Stage 3 Floor Gate → NEW, between intake and roadmap [INVESTIGATED context]
There is no floor today — roadmap generation always produces something. This is a NEW
isolated call that runs after `project_sessions` is written and BEFORE
`buildThemeDrafts`/its replacement. On INSUFFICIENT it must be able to halt and surface
one question on a screen.
→ **Process change:** the Confirm Understanding screen (or a new interstitial) must be
able to display "we need one more thing: {question}" and loop back, instead of always
proceeding to themes.
→ **Storage:** log floor verdict + question (new small table or a `project_sessions`
field) so it's auditable.

### Stage 4 Extract Themes → replaces `buildThemeDrafts` entirely [INVESTIGATED]
This is the core replacement. `buildThemeDrafts` (string concatenation) and
`buildTodoDrafts` (`{action} for {theme}` template) are DELETED and replaced by:
Part A generate candidates (model), Part B isolated classification (model),
Part C what_not_to_do filter, Part D choose one + candidate DoD.
→ **Move server-side:** current generation round-trips through a hidden browser form
(`firstThreeThemes` slice at form:795). New generation runs server-side; nothing frozen
at draft time, nothing round-tripped.
→ **Kills the theme_index linkage bug** by construction (titles generated against the
final approved set, after the cut).

### Stage 5 Referee → NEW, wraps Stage 4 output [proven component]
No equivalent today. Sits between theme generation and the R2 cards: only referee-passed
themes reach the user. Isolated reject-only calls, AND rule, regenerate loop, retry cap.
→ **Screen impact:** R2 theme cards now render *approved* themes. The existing "Not
quite" correction path stays — but now it re-runs the referee, not the keyword
heuristic `canStandAsTheme` (which is DELETED).

### Stage 6 Assemble → replaces `confirmRoadmapOnboarding` insert logic [INVESTIGATED]
The insert into `initiatives`/`work_items` stays, but the VALUES change:
- `initiatives.name` = distilled theme (not verbatim session sentence)
- `initiatives.definition_of_done` = referee-approved DoD (not the mad-lib template)
- `work_items.title` = states its own completion condition (not `{action} for {theme}`)
- `work_items.priority` = derived from Now/Next/Later, not hardcoded 'medium'
- `work_items.due_date` = derived from sequence+cadence, not NULL
- `initiatives.provisional` = NEW, derived from `inferred_from`
→ **Schema change:** add `initiatives.provisional` (bool), `initiatives.inferred_from`
(text/jsonb). Add a refusals store (new table `refusals` or reuse a column) for the cut.

### Stage 7 Input Safety → cross-cutting [not blocking Alpha]
Applies at every call that ingests intake text. Not required for the roadmap pipeline
to function; required before any agent reads intake/KB. Note: Supabase MCP already wraps
query results as untrusted — mirror that discipline in app calls.

## The process changes, before → after (the "slightly adjusted intake" you asked for)

| Step | Today | After |
|---|---|---|
| Discovery | screens → 4 calls | + establish & confirm the **outcome** |
| Understanding | Confirm Understanding (4 cards) | + a 5th card: the outcome, editable |
| **Floor** | *(none)* | **NEW: sufficiency gate; can halt & ask 1 question** |
| Theme gen | string concat, browser round-trip | server-side model gen + isolated classify |
| Theme review | R2 cards from raw drafts | R2 cards from **referee-approved** themes |
| "Not quite" | re-runs keyword heuristic | re-runs the **referee** |
| Confirm | inserts template rows | inserts **approved, distilled, sequenced** rows |
| Dates | never set | **derived** from sequence |
| The cut | invisible | **surfaced** (quiet line → KB) |

## Build order against the current system (revised, integration-aware)

1. **Fix the parser bug** (referee dependency). Half a day.
2. **Verify the intake calls** [INFERRED → grounded]: read the actual intake generation
   code before touching Stage 1/2 mappings. Do not build on inference.
3. **Floor gate standalone** — build + test against the 10 synthetic intakes. Highest
   risk. Must decline the ones that should be declined.
4. **Referee standalone** — package validator-v2 as the enforcement layer + regenerate
   loop. Proven component.
5. **Replace `buildThemeDrafts`/`buildTodoDrafts`** with Stage 4, server-side. Delete
   `canStandAsTheme`. Wire the referee in front of R2.
6. **Replace `confirmRoadmapOnboarding` values** with Stage 6 derivations + schema adds.
7. **Chain end-to-end**, test on synthetics then the 2 real tester intakes.
8. **Acceptance test:** whole pipeline vs today's string-concat pipeline, same intakes,
   Simeon blind-grades. Go/no-go.

## What must be verified before building (do not skip)

- [ ] Read the real intake generation calls; correct every [INFERRED] claim above.
- [ ] Confirm where `outcome` should live (new column vs nested).
- [ ] Confirm the R2 "Not quite" correction path can call the referee.
- [ ] Confirm the confirm-screen can host a floor-gate halt/question loop.
- [ ] Schema migration plan for: `outcome`, `initiatives.provisional`,
      `initiatives.inferred_from`, refusals store.
