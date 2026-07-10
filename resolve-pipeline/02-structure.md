---
status: current
---

# Stage 2 — Structure

**Type:** generator. **Model call:** yes.
**Purpose:** turn the elicited mess into the intake schema the rest of the pipeline
consumes. This stage adds the one thing write-spec lacks entirely — the distinction
between a lever and a task — but it does NOT yet decide which surfaces are themes.
It structures; Stage 4 classifies under enforcement.

## Input

The Stage 1 output object (outcome, surfaces_raw, refusals, uncertainty, dream).

## What you produce

A structured intake record. For each surface, you attach the raw text and, critically,
**where it came from** — the span of the user's input it was drawn from. You do not
judge whether it is a good theme. You do capture enough for the referee to judge later.

## The lever concept (this is the part that is new vs write-spec)

You must understand, though not yet enforce, the difference:

- A **theme** is a *lever* — something the founder pushes and keeps pushing, with no
  finish line of its own. Someone has to decide when it is enough. "Make deck research
  more effective." You never complete it; you decide it's enough.
- A **task** is a specific thing that gets done, and the world tells you when. "Decide
  the product shape." "Publish the pricing page." Observable finish line.
- An **artifact** is a thing being built. "A searchable knowledge base." Not a lever.
- An **outcome** is an end-state nobody pushes on directly. "Get better at the game."
  It is what themes *serve*, not a theme itself.

For each surface, record your provisional read of which of these it is — but mark it
`provisional_class`, because Stage 4 will re-decide it in an isolated call. Do not
filter here. Do not drop anything. Carry everything forward with its provisional
class and its source span.

## Non-goals / refusals (better-structured than the user gave them)

Take the user's stated refusals and, for each, ensure it has a *reason*. A refusal
without a reason is not usable downstream. If the user stated a refusal without a
reason, either infer the reason from context and mark it inferred, or flag it as
missing-reason. Do not invent refusals the user did not express.

## Provisionality — derived, never asked

For each surface, record `inferred_from`: the exact span of user input it traces to.
Do NOT ask the model to rate its confidence. Provisionality is computed later, in
code, from how many independent sources a surface has and their quality. Your job is
only accurate attribution.

## Output contract

```
{
  "outcome": "<carried from Stage 1, confirmed>",
  "project": "<one-line neutral description of what this is>",
  "state": "<where the project actually is right now>",
  "surfaces": [
    {
      "text": "<the surface, distilled — NOT copied verbatim from a long sentence>",
      "provisional_class": "THEME | TASK | ARTIFACT | OUTCOME",
      "inferred_from": "<exact user-input span this came from>"
    }
  ],
  "what_not_to_do": [{"item": "...", "reason": "...", "reason_source": "stated | inferred"}],
  "uncertainty": ["..."]
}
```

## Hard constraints

- **Distil, do not copy.** A surface must be an extracted lever, not a sentence lifted
  whole from the intake. If your surface is a ≥8-word contiguous copy of a user
  sentence, you have not distilled it. (The current broken pipeline copies verbatim;
  do not reproduce that.)
- **Do not filter.** Everything the user raised goes forward with a provisional class.
  The floor and the referee decide what survives, not you.
- **Do not write definitions of done.** That is Stage 4→5.
