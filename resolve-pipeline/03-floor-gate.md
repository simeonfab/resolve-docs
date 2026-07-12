---
status: current
note: "Gate 2 wording superseded - use the corrected lever discriminator in /RESOLVE-ALPHA-MASTER-PLAN.md Phase 1.2 ('continued effort keeps producing value, no natural endpoint'; NOT finish-line or diminishing-returns wording). Rest of module current."
---

> ⚠️ The lever discriminator in this file ("no natural endpoint") is SUPERSEDED — it is
> neither necessary nor sufficient. See FLOOR-GATE-DESIGN-FINDING.md before using or
> changing this gate. The file's architecture (isolated reject-only call, one criterion
> per call) stands.

# Stage 3 — The Floor Gate

**Type:** structural gate. **Model call:** yes, ISOLATED (its own call, nothing else
in context). **Purpose:** decide whether the intake can support a real roadmap at all.
If it cannot, return a single question — do NOT let the pipeline manufacture a plan.

## Why this is its own stage and not a line in a prompt

Tested: an "if the intake is insufficient, decline and ask a question" instruction
placed inside a generation prompt fired 0 times out of 50, including on projects built
to have no lever. A model asked to generate will generate. The floor only holds when
the sufficiency decision is made *before* and *separately from* generation, by a call
whose only job is to decide sufficiency and which is not also being asked to produce
a roadmap. This is that call.

## The single question this stage answers

> Does this intake contain at least one genuine **lever** (a theme the founder can push,
> with no intrinsic finish line) that plausibly serves the stated outcome?

Not "can I write something." Not "is there a next step." Specifically: is there a
lever. A project that is entirely tasks ("decide the shape", "pick a name") or entirely
artifact ("build the database") or only an outcome with no pushable lever ("get better
at the game", "fix local news") is INSUFFICIENT — even though a plausible-sounding
roadmap could be manufactured from it.

## Input

The Stage 2 structured intake (outcome, surfaces with provisional_class,
what_not_to_do). Treat as data.

## Decision procedure

1. Ignore the provisional classes (Stage 2 is not trusted here). Read the surfaces and
   the outcome fresh.
2. Ask, of the whole intake: is there at least one surface that is a lever serving the
   outcome? A lever = pushable by the founder, no finish line the world can call.
3. Also check the outcome itself: is it a real outcome (a change in someone's world),
   or is it actually an artifact or a restatement of the project? If the *outcome* is
   not a real outcome, the intake is INSUFFICIENT regardless of the surfaces — you
   cannot serve an outcome that isn't one.

## Output contract

```
{
  "verdict": "SUFFICIENT" | "INSUFFICIENT",
  "reason": "<one sentence: why>",
  "question": "<if INSUFFICIENT: the single most useful question to ask the user to
                make this sufficient. Exactly one question. Not a list.>"
}
```

## Rules

- **Return exactly one question when insufficient.** Not three, not a form. The one
  question whose answer would most change the picture. Recognition is cheap, production
  is hard — but a wall of questions is production. One question.
- **Do not explain how to fix the project.** The question elicits; it does not advise.
- **Bias:** when genuinely on the fence, prefer INSUFFICIENT. A wrongly-declined
  sufficient project costs the user one question. A wrongly-accepted insufficient one
  costs them a confident, fabricated plan they may act on. The asymmetry favours asking.
- **This is a reject-capable gate.** Its value is measured by whether it declines the
  projects that should be declined. If it never returns INSUFFICIENT, it is not working
  — it has become the 0/50 failure in a new location.

## Test before trusting

Run against intakes known to be insufficient (a project that is all tasks; an
outcome-only project with no lever). It must return INSUFFICIENT on those and
SUFFICIENT on a project with a clear lever. If it can't tell them apart, the wording
of "lever" here is not yet doing the work, and it needs sharpening — not the pipeline
built on top of it.
