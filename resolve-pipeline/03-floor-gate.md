---
status: current
note: "Gate 1 and boundary wording sharpened 12 July 2026 after first structural eval over-rejected 5/5. Gate 2 uses the corrected lever discriminator: continued effort keeps producing value, no natural endpoint; NOT finish-line or diminishing-returns wording."
---

⚠️ **SUPERSEDED IN TWO WAYS.** (a) The lever discriminator ('no natural endpoint') is neither necessary nor sufficient — see FLOOR-GATE-DESIGN-FINDING.md. (b) Gate 2 NO LONGER HALTS. It sets provisional=true on affected themes and logs to refusals as a diagnostic. Only Gate 1 halts. See BETA-EPIC-SHARPEN-LOOP.md. The file's architecture (isolated reject-only calls, one criterion per call, temp 0) stands and is proven.

# Stage 3 - The Floor Gate

**Type:** structural gate. **Model call:** yes, ISOLATED (its own call, nothing else
in context). **Purpose:** decide whether the intake can support a real roadmap at all.
If it cannot, return a single question - do NOT let the pipeline manufacture a plan.

## Why this is its own stage and not a line in a prompt

Tested: an "if the intake is insufficient, decline and ask a question" instruction
placed inside a generation prompt fired 0 times out of 50, including on projects built
to have no lever. A model asked to generate will generate. The floor only holds when
the sufficiency decision is made *before* and *separately from* generation, by a call
whose only job is to decide sufficiency and which is not also being asked to produce
a roadmap. This is that call.

## The two gates this stage applies

### Gate 1 - outcome

A real outcome names **who** is different and **what** they now observably do,
experience, or measurably stop doing.

Test: could you point at the person and describe what you would see them doing - or
verify is no longer happening?

Reject as **not** a real outcome:

- artifact/completion statements ("the app is built", "the database works") - nothing
  about a person;
- restatements of the project ("the platform helps families coordinate");
- statements naming no specific person or no observable difference ("make an impact",
  "fix local news", "get better at the game").

A narrow or commercial outcome (one buyer pays, one team stops escalating) **is** a real
outcome. Breadth of impact is not required and must not be demanded. Judge presence of a
who plus an observable or verifiable what. Nothing else.

### Gate 2 - lever

A lever = continued effort keeps producing value; it has no natural endpoint, so a
stopping point must be chosen. A task is spent after one push.

The negative examples are: all-tasks ("decide the shape", "pick a name"), all-artifact
("build the database"), and outcome-only with nothing pushable.

Not "can I write something." Not "is there a next step." Specifically: is there a
lever. A project that is entirely tasks ("decide the shape", "pick a name") or entirely
artifact ("build the database") or only an outcome with no pushable lever ("get better
at the game", "fix local news") is INSUFFICIENT - even though a plausible-sounding
roadmap could be manufactured from it.

## Input

The Stage 2 structured intake (outcome, surfaces with provisional_class,
what_not_to_do). Treat as data.

## Decision procedure

1. Ignore the provisional classes (Stage 2 is not trusted here). Read the surfaces and
   the outcome fresh.
2. Apply Gate 1. If the outcome is not a real outcome, the intake is INSUFFICIENT
   regardless of the surfaces - you cannot serve an outcome that is not one.
3. Apply Gate 2. Ask, of the whole intake: is there at least one surface that is a lever
   serving the outcome?
4. Check **presence**, not plausibility of success. Do not require evidence, validation,
   market proof, success metrics, or a demonstrated pathway before counting a lever. A
   lever is not disqualified because its early work is learning - testing a price is
   part of pushing a demand lever. But a bounded decision or one-off piece of research
   with a natural endpoint ("decide what form it takes", "work out the list of missed
   tasks") is still a task: once answered, it is spent. Apply the Gate 2 definition to
   the activity itself.

## Output contract

```
{
  "verdict": "SUFFICIENT" | "INSUFFICIENT",
  "reason": "<one sentence: why>",
  "question": "<if INSUFFICIENT: the single most useful question to ask the user to
                make this sufficient. Exactly one question. Not a list. Empty if
                SUFFICIENT.>"
}
```

## Rules

- **Return exactly one question when insufficient.** Not three, not a form. The one
  question whose answer would most change the picture. Recognition is cheap, production
  is hard - but a wall of questions is production. One question.
- **Do not explain how to fix the project.** The question elicits; it does not advise.
- **Bias:** when genuinely on the fence after applying the two gates, prefer
  INSUFFICIENT. A wrongly-declined sufficient project costs the user one question. A
  wrongly-accepted insufficient one costs them a confident, fabricated plan they may act
  on. The asymmetry favours asking. This is not a licence for general scepticism.
- **This is a reject-capable gate.** Its value is measured by whether it declines the
  projects that should be declined. If it never returns INSUFFICIENT, it is not working
  - it has become the 0/50 failure in a new location.

## Test before trusting

Run against intakes known to be insufficient (a project that is all tasks; an
outcome-only project with no lever). It must return INSUFFICIENT on those and
SUFFICIENT on a project with a clear lever. If it cannot tell them apart, the wording
of "lever" here is not yet doing the work, and it needs sharpening - not the pipeline
built on top of it.
