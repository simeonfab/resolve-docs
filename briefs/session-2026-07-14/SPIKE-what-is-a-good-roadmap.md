# SPIKE — WHAT IS A GOOD ROADMAP, AND WHAT QUESTIONS PRODUCE ONE

**Status: Alpha-blocking research spike. Owner: Simeon. Precedes the elicitation redesign.**

## Why this spike exists

The referee — the machine deciding, alone, what counts as a real theme — has failed **five times**
(four prompt passes, a validation eval we can't reproduce, a weak model, a strong model). The
strong model failed 9/9 on a *coherent* reading of an *incoherent* criterion. The pattern is the
finding: **automated certification of theme-quality is the thing that doesn't work.**

**Decision taken (Simeon, 14 July): put a human in the loop.** The pipeline proposes; where it's
uncertain, it ASKS; the founder confirms. The asking is the product, not a failure state — people
enjoy being interviewed about their own work (the personality-quiz pattern: many questions, then a
reveal people are desperate to see). This dissolves the F33 blocker for Alpha and relocates the
hard problem from "judge the output" to "design the questions."

**But that only works if the questions are good.** And we cannot design good questions until we
know what "good roadmap" even means — to real people, not in the abstract. **This spike defines the
target before anyone builds the flow to hit it.**

## The two things this spike must produce

1. **A working definition of "good roadmap"** — grounded in real human judgement, not derived.
2. **A first-draft question set** — the questions whose answers would let the pipeline build that
   good roadmap. This is the input to the elicitation redesign.

## Method — reverse-engineer the criterion from a real choice

Do NOT ask people to define "good" — they give platitudes ("clear", "actionable"). Instead, make
them CHOOSE between two real roadmaps for their own situation and justify it. The justification is
the data; the criteria live in the "why", unspoken.

### The baseline question (ask Simeon's workplace contacts — real people he's close to)

> "Think about a project or goal you're actually working on right now — something real, that you'd
> genuinely like to make progress on.
>
> If I gave you two plans for it: **Plan A** is three broad directions to keep pushing on — no
> finish line, just areas that matter. **Plan B** is a checklist of ten concrete tasks you could
> tick off this week.
>
> Which one would you actually use — and what would make you trust it enough to act on it
> tomorrow?"

**Why this question:**
- "A project you're actually working on" forces a real judgement, not a hypothetical.
- Plan A vs Plan B IS the core product tension: themes (levers, no finish line) vs tasks
  (concrete, tickable). We learn whether people even want themes, or whether the checklist wins.
- "What would make you trust it enough to act tomorrow" is the payload — that sentence, in their
  words, is the criterion. Ten answers → the pattern is the spec.

**How to run it:**
- Ask out loud, let them ramble, capture the "why" VERBATIM (not just their A/B pick).
- Don't lead. If they say "both", make them pick one and defend it.
- ~10 people is enough to see a pattern. These are warm contacts — low cost, high signal.

### What to listen for (do not prompt these — see if they arise unprompted)
- Do they want direction (A) or a checklist (B) — or A *sequenced into* B?
- What makes them TRUST it? (specificity? that it reflects what they said? that it's short?)
- What makes them distrust it? (too generic? too obvious? too much? invented detail?)
- Does "no finish line" appeal, or does it read as vague? (This directly tests the F33 axis with
  real humans instead of a model.)
- How much detail is "enough" vs "overwhelming"?

## Outputs of the spike
1. **A definition of "good roadmap"** — 3-5 criteria, in users' own framing, evidenced by quotes.
   This REPLACES the broken referee criterion as the Alpha target.
2. **A first-draft question set** for the elicitation flow — the questions that gather what's
   needed to hit that definition.
3. **A decision on the theme-vs-task question:** do users want themes at all, and if so, do they
   want them broken into tasks? (Directly informs the Now/Next/Later sequencing work too.)

## What this unblocks
- The elicitation redesign (the new intake flow — Alpha).
- The referee's REDUCED Alpha role (propose-and-confirm, not unilateral hard-reject).
- A real, inspectable validation target to replace the missing 92% eval.

## Explicitly NOT in this spike
- Building the flow. Designing the reveal. The automated criterion (that's the background research
  track — mature it until it's trustable, Beta or later). The latency spike (separate, Beta).
