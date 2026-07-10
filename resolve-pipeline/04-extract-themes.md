---
status: current
---

# Stage 4 — Extract Themes

**Type:** generator + isolated classifier. **Model call:** yes (two kinds).
**Purpose:** from a SUFFICIENT intake, produce the candidate themes and, for the chosen
one, a candidate definition of done. Classification is enforced by a SEPARATE call
because inline classification is ignored.

Only runs if Stage 3 returned SUFFICIENT.

## Part A — Generate candidate themes

From the surfaces, produce a small set of candidate themes (aim for 2–4). Each must be:

- a **lever** (pushable, no finish line),
- **distilled** from the intake, not copied,
- traceable (carry `inferred_from`).

Do not force a count. If only one real lever exists, return one. Padding to fill a
screen is the failure this pipeline removes.

Output per candidate: `{text, inferred_from}`.

## Part B — Classify (SEPARATE isolated call per surface)

Do NOT trust Part A or Stage 2 to have filtered correctly. For each candidate, run an
isolated classification call whose ONLY job is:

> Is this a THEME (a lever, no finish line), a TASK (observable finish line), an
> ARTIFACT (a thing built), or an OUTCOME (an end-state nobody pushes)? Reply with one
> label and one sentence.

Keep only those the isolated call labels THEME. Tasks are set aside into a task pool
(they may be real work, just not themes). Artifacts and outcomes are dropped.

Rationale: inline classification inside a generation prompt was ignored 130/130. A
dedicated call that does nothing but classify is the only version that fires.

## Part C — Apply what_not_to_do

For each surviving theme, check it against the user's stated refusals. Any theme a
refusal forbids is not eligible to be "Now" (it may still be Later). This is a hard
constraint, not advice — the user already said no.

## Part D — Choose one, and write its definition of done

Rank the eligible themes by which is the narrowest lever that still reaches the outcome.
Choose exactly one as the focus. If two seem equally central, that is a signal Part B
mis-split — two inseparable things are probably one thing; redo Part B.

For the chosen theme, write a candidate **definition of done** — a chosen stopping
point for a lever that has none. Aim for it to be:

- **EVIDENTIAL** — evidence the *outcome occurred* (the people named did the thing),
  not that the artifact was built, and not a proxy (speed, usage, satisfaction).
- **MINIMAL** — nothing removable while still evidencing the outcome.

You are *aiming* for these. You do not get to certify them — the referee (Stage 5)
does, in isolation. Expect some of your definitions of done to be rejected and
regenerated. That is the design working, not failing.

## Output contract

```
{
  "themes": [
    {"text": "...", "inferred_from": "...", "class_verdict": "THEME",
     "eligible_now": true | false}
  ],
  "task_pool": ["<surfaces classified TASK>", ...],
  "chosen_theme": "<the one focus theme>",
  "definition_of_done": "<candidate stopping point for the chosen theme>",
  "what_it_gives_up": "<what choosing this stopping point sets aside>",
  "refused": [{"candidate": "<dropped surface>", "why": "<task/artifact/outcome/forbidden>"}]
}
```

## Hard constraints

- **Exactly one chosen theme.** Scarcity forces the decision the schema can't.
- **Never certify your own definition of done.** It goes to the referee.
- **Carry the refusals forward.** Stage 6 needs them to show the cut.
