---
status: current
---

# Stage 6 — Assemble

**Type:** deterministic. **Model call:** NO. This stage is code, not a prompt.
**Purpose:** take the referee-approved themes, tasks, definition of done and refusals,
and lay them out. No judgement happens here — all judgement already happened upstream
under enforcement. If you find yourself wanting a model call here, something upstream
didn't do its job.

## Input

- Approved themes (each passed the referee's theme checks)
- The chosen theme + its approved definition of done
- The task pool (surfaces classified TASK)
- Refusals (both user-stated `what_not_to_do` and system `refused` from Stage 4)
- The ranking from Stage 4 Part D

## Sequence → dates (derived, never generated)

The roadmap is a sequence, not a calendar. Derive dates from position; never ask a
model for a deadline (it invents plausible lies).

- **Now** = the chosen theme + the tasks that advance its definition of done. Cadence:
  this period (e.g. this week).
- **Next** = the other eligible themes, in ranked order. Cadence: the following period.
- **Later** = themes blocked by `what_not_to_do` or by an unresolved uncertainty.
  Unscheduled.

Dates, if shown, are computed from these buckets and the cadence, in code. They are
editable by the user (in the Work view), not authored by the model.

## Tasks

Each task title must state its own completion condition (observable finish line). If a
task needs a separate definition-of-done sentence written for it, its title is wrong or
it isn't a task — send it back. Tasks under Now must each advance the chosen theme's
definition of done; a task that advances nothing is not on the roadmap.

## Showing the cut (this is a feature, not a footnote)

The refusals are surfaced, because the cut *is* the value — a roadmap that silently
contains everything has decided nothing. But showing the cut re-tempts the user to add
it back. Resolution:

- Do NOT list cut candidates inline on the roadmap as if they were options.
- DO surface a single quiet line ("N things were deliberately left out, and why") that
  routes to the Knowledge Base / decisions record, where each refusal is stored with
  its reason.
- Seen, not offered.

## The linkage integrity rule (fixes a live bug)

Every work item's title must reference the theme it is actually linked to. The current
pipeline freezes titles at draft time and carries them through a browser round-trip,
producing items whose title names one theme while their link points at another. In this
pipeline: titles are generated server-side against the final, referee-approved theme
set, after any cut — never frozen before the cut, never round-tripped through the
client.

## Output

The roadmap object the app renders:

```
{
  "now": {"theme": "...", "definition_of_done": "...", "tasks": ["...", ...]},
  "next": [{"theme": "...", "inferred_from": "...", "provisional": true|false}],
  "later": [{"theme": "...", "blocked_by": "<refusal or uncertainty>"}],
  "cut": [{"candidate": "...", "reason": "..."}],   // routed to KB, not shown inline
  "open_questions": ["..."]
}
```

## Provisional flag (derived in code, not by a model)

A theme is marked `provisional` if its `inferred_from` traces to a single thin source
(e.g. one sentence, or `recommended_action` alone). The UI renders provisional themes
differently. This is computed here from attribution — never self-reported by the model.
