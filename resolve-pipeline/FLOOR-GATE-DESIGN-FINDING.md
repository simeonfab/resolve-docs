---
status: open-design-finding
opened: 2026-07-12
owner: Simeon
phase: BETA SPIKE — blocks nothing in Alpha; must be resolved before the refusal
  behaviour is sold as a differentiator
supersedes: the "lever = no natural endpoint" discriminator in 03-floor-gate.md and in
  RESOLVE-ALPHA-MASTER-PLAN Phase 1.2
note: "Written to be picked up COLD, months later, with no memory of this session.
  Everything needed to resume is in this file."
---

# The Floor Gate — open design finding

## TL;DR for whoever picks this up

The floor gate **works structurally** (split isolated calls, temp 0, reject-only,
presence-not-proof) — that part is proven and must not be re-litigated.

**The CRITERION it enforces is wrong.** The floor currently asks *"is there an ongoing
activity with no natural endpoint?"* It should ask *"is there an actionable causal path
specific enough that the model does not have to invent the strategy?"*

Do **not** try to fix this with another prompt pass. Four were tried. It is a product
design question, not a wording question. **Resume at §6.**

---

## 1. What the floor gate is (no prior context assumed)

Resolve turns a founder's messy project description ("intake") into a roadmap. The
**floor gate** decides one thing: **can this intake support a real roadmap at all?** If
it cannot, the product must **refuse** — return a single question — rather than
manufacture a confident, plausible plan the founder might act on.

This is the product's core differentiator ("structure and refusal"). It is the thing
that makes Resolve not-a-chatbot. It is also the highest-risk component in the system.

## 2. What is PROVEN and must not be re-derived

- **A model asked to generate will generate.** An "if insufficient, decline and ask"
  instruction placed *inside* a generation prompt fired **0 times out of 50**, including
  on projects deliberately built to have no lever. Related: classification criteria
  inside a generation prompt ignored 130/130; a model checking its own output passed it
  200/200.
  → **Enforcement only works as a separate, isolated, reject-only call.** Settled.
- **One criterion per call.** A single call asked to judge two gates **short-circuits**:
  it evaluates the first, finds a pass, and stops. Measured: 11/15 verdict-reasons cited
  Gate 1; cases that should fail Gate 2 returned SUFFICIENT with pure Gate-1 pass
  reasons. Verdicts flipped run-to-run on identical input (**zero stable verdicts across
  3 runs**).
  → **Split into isolated single-criterion calls, AND-ed in code.** After splitting +
  temperature 0: **15/15 identical across 3 runs.** Settled.
- **Presence, not proof.** The first working eval returned **5/5 INSUFFICIENT** —
  rejecting even the clear-lever controls — because the gate demanded *evidence the
  project would succeed* ("no validated demand", "doesn't specify how success is
  assessed"). A gate that demands proof rejects everything.
  → **Check presence of a path, never plausibility of success. Learning activities
  (testing a price, validating demand) ARE valid — pushing them is how the answer gets
  found.** Settled.

**Keep all three. The architecture is sound. Only the criterion is wrong.**

## 3. The criterion that is wrong

Current (v1) definition — **DO NOT REUSE**:

> A lever = an activity the founder can push, with **no natural endpoint** — continued
> effort keeps producing value, so a stopping point must be chosen. A task is spent
> after one push.

Fail taxonomy built on it (TASK / ARTIFACT / RESTATED GOAL) was claimed to be
exhaustive. **It is not**, because *endpoint* is not the right axis.

### Why "no natural endpoint" fails — it is neither necessary nor sufficient

**Not necessary** — bounded interventions can be the *main causal lever* of a
legitimate project:
- Install an automated medication-reminder system so a family stops missing doses.
- Introduce pre-orders to find out whether buyers will pay £30.
- Replace a manual process with an automated workflow. Change the pricing model.
- Fit a wheelchair ramp so someone can enter the building independently.

These have natural endpoints. Some produce artifacts. They still create durable
outcomes and support genuine roadmaps. **v1 rejects all of them.**

**Not sufficient** — endless activities can be strategically empty:
- "Keep posting on social media." "Do more marketing." "Improve the product."
- "Keep adding features." "Work on distribution."

These are pushable and endless, and **v1 passes them** — yet the model would still have
to invent the entire strategy, which is exactly the failure the floor exists to prevent.

### Additional non-lever shapes v1's taxonomy misses entirely
- **EXTERNALLY CONTROLLED** — an activity, but not the founder's: "retailers stock the
  product", "the regulator approves", "coaches recommend us".
- **VAGUE ACTIVITY SHELL** — pushable, endless, constrains nothing: "do marketing",
  "engage the community", "work on retention".
- **MOTION WITHOUT ADVANCEMENT** — ongoing and relevant but preserves rather than
  creates: "keep the website running", "answer support emails".

Also: **the v1 categories overlap and are not mutually exclusive.** "Build a brand
presence" is simultaneously readable as activity, artifact, outcome, or vague theme. A
list of labels is not a closed decision function when the labels are not disjoint.

## 4. The reframe (this is the finding)

> **A roadmap does not require an endless activity. It requires an ACTIONABLE CAUSAL
> PATH that is sufficiently specified not to be invented.**

This is the right axis because it is *the same axis as the floor's purpose*: the floor
exists because models manufacture plans from thin input. So the gate should test exactly
the property whose absence forces manufacturing — **is the causal path present in the
intake, or would the model have to supply it?**

A valid lever may be **finite or continuing**; it may **produce an artifact or operate a
process**. What matters is that the intake states or directly implies *how* the work
changes the outcome.

Re-reading the fixtures under the reframe: **Q4 (heritage app) should fail not because
"build the lesson library" is an artifact**, but because the intake never says how
building those components produces *conversations between grandchildren and
grandparents*. The behavioural mechanism is missing — the model would have to invent it.
That is a much better reason, and it generalises.

## 5. The open risk in the reframe — DO NOT SKIP THIS

The proposed replacement criteria include "specific enough to constrain the roadmap"
and "roadmap depth". **These smuggle a QUALITY judgement into a STRUCTURAL gate.**

"Sufficiently specified" is a **slider, not a boundary**. A model asked *"is this
specific enough?"* will always find that it isn't — which is **precisely the mechanism
that produced the 5/5-reject disaster** (§2, presence-not-proof). The reviewer's own
critique of the proof-demand failure applies to the reviewer's own proposal.

**The unsolved design problem is therefore:** how to test for a present causal path
**as a boundary condition** (present / absent) rather than **as a quality slider**
(specific enough / not specific enough). Solve that and the floor is solved.

## 6. RESUME HERE — what the Beta spike must do

1. **Do not touch the architecture.** Split isolated calls, one criterion per call, temp
   0, reject-only, presence-not-proof. Proven. Keep.
2. **Answer the product question first — it is not a prompt question:**
   **Should Resolve refuse the medication-reminder project?** (Real outcome; single
   bounded intervention; clearly causal.) If NO → v1's whole lever concept is wrong and
   the reframe is confirmed. If YES → Resolve is deliberately only for
   continuous-improvement products, which is a **positioning decision** and must be
   stated as one. **This is Simeon's call, not an agent's.**
3. **Design the causal-path test as a boundary, not a slider** (§5). Candidate framing:
   *"Does the intake state or directly imply HOW this work changes the outcome — or
   would you have to supply that link yourself?"* The second clause is the boundary: it
   asks the model to notice it is about to invent, which is a factual question about the
   input, not a quality judgement about the project.
4. **Build a NEW validation set — the current fixtures are burnt.** They were written by
   the same person who wrote the criteria and have been iterated against four times.
   Q1/Q4/Q5 now encode assumptions about "implied mechanism". Only Q3 and the
   restated-goals probe are clean. The new set must include: bounded-but-causal
   interventions (medication reminder, pricing change, pre-orders); endless-but-empty
   activities ("do more marketing"); externally-controlled surfaces; and **real intakes
   from Phase S sessions** — written by people who never saw the prompt.
5. **Stability must be tested properly.** Three runs of identical text at temp 0 proves
   the inference path is deterministic — **it does not prove the concept is robust.**
   Real test: paraphrases, surface reordering, irrelevant detail, synonym swaps, and
   **minimal pairs** where one clause flips the correct verdict (e.g. "become known in
   the local scene" vs "publish weekly matchup analysis to local players" — the second
   introduces an actual activity).
6. **Fix Gate 1 too.** It currently rejects "people waste less food" for lacking a
   specific "who". That conflates *"is there a real outcome?"* with *"is the audience
   targeted enough?"* — two different questions. Gate 1 must answer only the first.
   Broad consumer outcomes are real outcomes.

## 7. Why this is a Beta item, not an Alpha blocker

Alpha ships the pipeline with the floor's **architecture** in place and a **v1
criterion** that is known-imperfect: it over-rejects bounded interventions and
under-rejects vague perpetual activity. Every refusal is logged to the `refusals` table
(already built), so Alpha **generates the evidence** this spike needs — real intakes,
real verdicts, real disagreements.

**But the refusal behaviour must NOT be sold as the differentiator until this is
fixed.** Refusing a legitimate founder's legitimate project — telling someone whose
medication-reminder project is perfectly sound that Resolve won't help them — is worse
than not refusing at all. **That is the monetisation gate: "structure and refusal" is
only a paid-tier claim once the floor refuses the right things.**

## 8. Provenance — where this came from

- Four eval iterations on 12 July 2026 (5 fixtures, gpt-4o): 5/5-reject → wording
  sharpened → 5/5 correct on one run → **3× rerun gave ZERO stable verdicts** → split
  gates + temp 0 → 15/15 stable, but the all-artifact case stably PASSED.
- Rather than patch a third time, the complete Gate 2 function was derived — which
  surfaced a fourth, untested hole (RESTATED GOAL: "win more", "get traction" — a result
  with no activity in it, which v1 would pass).
- The full design was then sent for **independent adversarial review by a different
  model**, which correctly identified that the endpoint axis is wrong. Its critique is
  the basis of §3–§5. Its own proposal has the slider problem in §5 — take the reframe,
  not the criteria, as written.
- Original eval JSONLs from the pre-Alpha referee testing **do not exist in any repo**
  and could not be located. Do not go looking.
