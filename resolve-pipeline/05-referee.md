---
status: current
note: "Known issue: strict verdict parser silently drops ~30% of verdicts (Ticket 0.1)."
---

# Stage 5 — The Referee (enforcement)

**Type:** enforcement. **Model call:** yes — one ISOLATED call PER criterion PER
candidate. Never combined. **Purpose:** reject themes and definitions of done that
fail the criteria, and send them back to be regenerated with the reason attached.

This is the layer that makes the difference between "better than string concatenation"
and "actually good." The generators produce candidates; the referee is why the output
can be trusted. It must never share a call or context with generation.

## The two families of check

### Theme checks (applied to each candidate theme)

Run each as its own isolated call. A theme passes only if it passes ALL (the AND rule).

**LEVERABLE**
> A THEME is a lever the founder can push directly and keep pushing. It is NOT a theme
> if it is something you complete once (a task), a thing you build (an artifact), or an
> end-state nobody can push on directly (an outcome).
> THEME: {theme}  PROJECT: {one-liner}
> Reply VERDICT: PASS or FAIL / REASON: one sentence.

**NO_FINISH_LINE**
> A THEME has no intrinsic finish line — the world cannot tell you it is "done"; someone
> must choose when it is enough. If the world can tell you it is complete, it is a TASK.
> THEME: {theme}
> Reply VERDICT: PASS or FAIL / REASON: one sentence.

**DISTILLED**
> A THEME is a short lever DISTILLED from the intake, not a sentence copied out of it.
> FAIL if the theme is a near-verbatim copy of an intake sentence rather than an
> extracted lever.
> THEME: {theme}  INTAKE: {full intake text}
> Reply VERDICT: PASS or FAIL / REASON: one sentence.

(SERVES_OUTCOME was tested and found near-decorative — every failure of it was also a
LEVERABLE failure. It is omitted to avoid redundant calls. If a theme passes LEVERABLE
it effectively serves the outcome. Re-add only if evidence later shows a gap.)

### Definition-of-done checks (applied to the chosen theme's DoD)

Run each as its own isolated call. Passes only if BOTH pass (the AND rule).

**EVIDENTIAL** (verbatim, validated)
> A definition of done is EVIDENTIAL if it describes evidence that the OUTCOME OCCURRED
> — that the people named in the outcome did the thing the outcome names. It is NOT
> evidential if the evidence is only that the artifact was built (features shipped,
> platforms updated), or if it measures a PROXY instead of the outcome — speed, usage,
> satisfaction, or engagement standing in for the outcome itself.
> THEME: {theme}  OUTCOME: {outcome}  CANDIDATE: {definition_of_done}
> Reply VERDICT: PASS or FAIL / REASON: one sentence.

**MINIMAL** (verbatim, validated)
> A definition of done is MINIMAL if no element could be removed while still leaving
> evidence that the outcome occurred. If any part could be cut and the remaining
> statement would still evidence the outcome, it is NOT minimal.
> Ignore bare numeric thresholds (e.g. '3 players', '30%') — a lone number is not a
> removable element; do not fail a candidate solely for the size of a number.
> THEME: {theme}  OUTCOME: {outcome}  CANDIDATE: {definition_of_done}
> Reply VERDICT: PASS or FAIL / REASON: one sentence. If FAIL, name the element to cut.

## The AND rule (non-negotiable)

A candidate passes only if EVERY clause returns PASS. Never route on one clause.
EVIDENTIAL alone has a measured ~40% wobble on the preference case in isolation; it is
safe only because the other clauses carry it under the AND. Any code path that acts on
a single clause's verdict reintroduces that leak.

## The regenerate loop

1. Run all relevant isolated checks.
2. If all PASS → accept.
3. If any FAIL → return the candidate to its generator (Stage 4) WITH the failing
   clause and its one-sentence reason attached, and regenerate.
4. Cap at **N retries** (start N=2). If still failing after N, do not loop forever: drop
   the theme if others survive. If it was the only theme, do NOT halt and do NOT ask the
   user a question — ship it as `provisional=true`, log the failing clause and its reason
   to `refusals`, and show the reasoning for correction. Only Gate 1 (no outcome) halts;
   a referee that cannot perfect the last theme degrades to a provisional plan, it does
   not escalate to a question.

## Parsing (fix the known bug)

The model replies `VERDICT: PASS / REASON: ...` but frequently drops the literal
`REASON:` label, replying `PASS / ...`. The strict parser silently discarded ~30% of
these. Use a tolerant parser that extracts PASS/FAIL robustly, and separately log
strict-format adherence. Better: constrain the reply with structured output. A referee
that drops a third of its verdicts is worse than none.

## Stability note

Two clauses (C/MINIMAL, D/EVIDENTIAL) showed single-run instability of 40–50% on their
non-decisive cases. The AND rule masks this in production. If you ever need a
single-clause verdict to be reliable (you shouldn't), it isn't — run it 3× and take the
mode, or don't rely on it.
