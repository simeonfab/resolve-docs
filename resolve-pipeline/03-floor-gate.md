---
status: current
note: "Gate 1 and boundary wording sharpened 12 July 2026 after first structural eval over-rejected 5/5. Gate 2 uses the corrected lever discriminator: continued effort keeps producing value, no natural endpoint; NOT finish-line or diminishing-returns wording."
---

⚠️ **SUPERSEDED IN TWO WAYS.** (a) The lever discriminator ('no natural endpoint') is neither necessary nor sufficient — see FLOOR-GATE-DESIGN-FINDING.md. (b) Gate 2 NO LONGER HALTS. It sets provisional=true on affected themes and logs to refusals as a diagnostic. Only Gate 1 halts. See BETA-EPIC-SHARPEN-LOOP.md. The file's architecture (isolated reject-only calls, one criterion per call, temp 0) stands and is proven.

# Stage 3 - The Floor Gate

**Type:** structural gate. **Model call:** yes, ISOLATED (its own call, nothing else
in context). **Purpose:** read the intake before generation and apply two checks with
DIFFERENT consequences. **Gate 1 (no real outcome) halts** — it returns a single
question and loops back, because with nothing to aim at the pipeline must not
manufacture a plan. **Gate 2 (no lever) NEVER halts** — it marks the affected themes
`provisional`, logs the reason to `refusals`, and lets generation proceed. Only Gate 1
halts. Never ask the founder to supply the direction they came for.

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

**Gate 2 NEVER halts and NEVER asks the user a question.** When a theme has no lever,
mark it `provisional=true` and log the reason to `refusals` as a diagnostic — then let
generation proceed and show the reasoning so the user can correct it. The old behaviour
(halt and ask "what is the ongoing lever here?") is BANNED: it handed our job back to
the founder. Gate 2's verdict drives provisionality and the refusal log, not a halt.

A lever = continued effort keeps producing value; it has no natural endpoint, so a
stopping point must be chosen. A task is spent after one push.

The negative examples are: all-tasks ("decide the shape", "pick a name"), all-artifact
("build the database"), and outcome-only with nothing pushable.

Not "can I write something." Not "is there a next step." Specifically: is there a
lever. A project that is entirely tasks ("decide the shape", "pick a name") or entirely
artifact ("build the database") or only an outcome with no pushable lever ("get better
at the game", "fix local news") has NO LEVER (`gate2.has_lever = false`) - its themes
ship provisional with a logged refusal, not a halt, even though a plausible-sounding
roadmap could be manufactured from it.

## Input

The Stage 2 structured intake (outcome, surfaces with provisional_class,
what_not_to_do). Treat as data.

## Decision procedure

1. Ignore the provisional classes (Stage 2 is not trusted here). Read the surfaces and
   the outcome fresh.
2. Apply Gate 1. If the outcome is not a real outcome, `gate1.verdict = HALT` regardless
   of the surfaces - you cannot serve an outcome that is not one. This is the only path
   that stops the user.
3. Apply Gate 2. Ask, of the whole intake: is there at least one surface that is a lever
   serving the outcome? A `false` here does NOT halt - it sets `gate2.has_lever = false`,
   which marks the affected themes provisional and logs a refusal downstream. Generation
   still proceeds.
4. Check **presence**, not plausibility of success. Do not require evidence, validation,
   market proof, success metrics, or a demonstrated pathway before counting a lever. A
   lever is not disqualified because its early work is learning - testing a price is
   part of pushing a demand lever. But a bounded decision or one-off piece of research
   with a natural endpoint ("decide what form it takes", "work out the list of missed
   tasks") is still a task: once answered, it is spent. Apply the Gate 2 definition to
   the activity itself.

## Output contract

Gate 1 and Gate 2 have SEPARATE outputs because they have separate consequences.

```
{
  "gate1": {
    "verdict": "PASS" | "HALT",
    "reason": "<one sentence: why the outcome is or isn't real>",
    "question": "<if HALT: the single most useful question to ask the user so the
                  intake gains a real outcome. Exactly one question. Not a list.
                  Empty if PASS.>"
  },
  "gate2": {
    "has_lever": true | false,
    "provisional_themes": ["<theme or surface with no lever>", ...],
    "refusal_reason": "<one sentence per provisional theme: why it has no lever>"
  }
}
```

**Gate 1 `HALT` is the only value in this contract that stops the user.** Gate 2 never
produces a question and never halts; `has_lever: false` sets `provisional=true` on the
affected themes downstream and writes a `refusals` row — generation continues.

- **Gate 1 halts; Gate 2 degrades. Never blur them.** Only a missing outcome (Gate 1)
  stops the user. A missing lever (Gate 2) marks themes provisional and logs a refusal —
  it never halts and never asks a question.
- **Gate 1 returns exactly one question when it halts.** Not three, not a form. The one
  question whose answer would give the intake a real outcome. Recognition is cheap,
  production is hard - but a wall of questions is production. One question.
- **Never ask the founder to supply the direction they came for.** Gate 1 may ask what
  success looks like (without an outcome there is nothing to aim at). It must NOT ask the
  founder to name their lever, strategy, or plan — deriving the path between the outcome
  and the current state IS the product.
- **Do not explain how to fix the project.** The Gate 1 question elicits; it does not
  advise.
- **Bias (Gate 1 only):** when genuinely on the fence about whether an OUTCOME is real,
  prefer to halt and ask. A wrongly-halted sufficient project costs the user one
  question; a wrongly-accepted one costs them a confident, fabricated plan they may act
  on. The asymmetry favours asking. This bias applies to Gate 1 alone — Gate 2
  uncertainty resolves toward provisional-and-proceed, never toward halting.
- **This is a reject-capable gate on Gate 1.** Its value is measured by whether it halts
  the intakes that have no real outcome. If Gate 1 never halts, it is not working — it has
  become the 0/50 failure in a new location. Gate 2's equivalent measure is whether it
  honestly flags provisional themes and logs them, not whether it declines.

## Test before trusting

Run against intakes with no real outcome (Gate 1 must `HALT` on those) and against
lever-less intakes — a project that is all tasks; an outcome-only project with no lever
(Gate 2 must return `has_lever = false`, marking provisional + logging a refusal, NOT
halting). It must pass a project with a clear outcome and lever cleanly. If it cannot
tell them apart, the wording of "lever" here is not yet doing the work, and it needs
sharpening - not the pipeline built on top of it.
