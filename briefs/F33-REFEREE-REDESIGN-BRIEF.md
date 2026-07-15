# F33 — THE REFEREE CRITERION: REDESIGN BRIEF

**Status: proven broken on a strong model. No longer a Beta spike — it blocks a trustworthy Alpha roadmap.**

This is a DESIGN brief, not a build ticket. It ends in a decision, not a merge. Do not write
pipeline code from it until the criterion is settled.

---

## WHAT WE NOW KNOW FOR CERTAIN (proven, not theorised)

An isolated replay of the real Business Community intake, same prompts, changing only the model,
run 3× each (artifact: `artifacts/model-routing-replay-725384bf.json`, commit `61ebef4`):

- **`gpt-4.1-nano` broke the CLASSIFIER.** It labelled all 3 candidates TASK/TASK — every run —
  emptying the theme pool and forcing the blanket-provisional degrade. **This half is a routing
  bug**, fixed by putting a capable model on the classifier.
- **`gpt-4.1` FIXED the classifier** (all 3 → THEME, every run) **but still failed certification
  9/9** on `NO_FINISH_LINE`. **The criterion is independently broken.** A better model does not
  save it.

The decisive evidence — the strong model, on ONE theme, in ONE pass:
> LEVERABLE: **PASS** — "an ongoing lever the founder can keep pushing"
> NO_FINISH_LINE: **FAIL** — "includes a finish line: when a new member attends a second event…"

**Two clauses, contradictory verdicts, same theme, same moment.** That is a coherent model
faithfully applying an incoherent instruction.

---

## THE THREE STRUCTURAL FAULTS (all model-independent)

**Fault 1 — the DoD poisons the theme check.** The code sends the whole candidate, INCLUDING its
`definition_of_done`, to the theme clauses (`referee.ts:81`). A DoD is by nature a stopping point.
So `NO_FINISH_LINE` sees the stopping point and rejects the THEME for having one. The theme is
judged on the very thing that is supposed to make it actionable.

**Fault 2 — the regenerate loop is unwinnable.** When certification fails, the loop rewrites the
DoD and re-runs ALL clauses — including the theme clauses that a DoD rewrite cannot change
(`roadmap-generation.ts:902`). So it re-litigates an immutable verdict every round, always loses,
and gives up. This is both the ~15 wasted calls AND the structural deadlock.

**Fault 3 — the criterion itself is contradictory.** Even setting the plumbing aside:
- `NO_FINISH_LINE` demands a theme with no endpoint.
- `EVIDENTIAL` demands a DoD proving the outcome occurred — which requires a describable end-state.
- `MINIMAL` then rejects that same evidence as "removable."
EVIDENTIAL and MINIMAL were the two clauses supposedly locked by a 92%-pass eval — **and that eval
cannot be found in either repo** (harness, config, results, or model). Its numbers are currently
unverifiable. Treat "validated" as unproven until the eval resurfaces.

---

## THE ROOT CONCEPTUAL PROBLEM

"A theme is a lever with no natural endpoint" was already logged (F33) as neither necessary nor
sufficient:
- **Not sufficient:** "do more marketing" has no endpoint and constrains nothing.
- **Not necessary:** a bounded intervention (install a medication-reminder system) can be the true
  causal lever.

The better axis proposed by earlier adversarial review: **"is there an actionable causal path,
specific enough that the model doesn't have to invent the strategy?"** — NOT "does it have an
endpoint." The endpoint test is a proxy that fails in both directions.

**Do NOT attempt another prompt-tweak pass on the existing clauses. FIVE have now failed** (four
before, plus this strong-model replay). The clauses are the problem, not their wording.

---

## THE LEADING CANDIDATE FOR ALPHA — LET THE USER DEFINE IT  *(Simeon's direction)*

The whole difficulty is Resolve trying to *automatically judge* what counts as a real theme. That
judgement is exactly what keeps breaking. A different framing sidesteps it:

**Instead of the referee unilaterally accepting/rejecting, the user confirms.** The pipeline
proposes; the founder is shown the theme and its reasoning and confirms, adjusts, or rejects it —
the "Not quite" path that already exists. The machine stops pretending it can certify alone.

This fits the product thesis exactly: *"not just any direction — the RIGHT direction,"* shown
honestly, with the founder in the loop — rather than a silent gate that fails 9/9 and hides it
behind a provisional marker.

**Not a final decision.** Simeon's steer: this may be the ALPHA answer — ship the automatic judge
in a reduced, honest role, put the human in the loop for the hard call — and keep refining the
automated criterion in the background until it is strong enough to trust on its own. Beta or later.

Open questions for that framing (resolve before building):
- If the user defines/confirms the theme, what is the referee still FOR in Alpha? (Likely: catch
  the obvious task-masquerading-as-theme, surface reasoning, never hard-reject.)
- Does the classifier stay (needs a capable model per the routing fix) or also move to confirm?
- What happens to the degrade path if there is no unilateral rejection to degrade from?

---

## THE DELIVERABLE

A one-page decision doc answering:
1. **Alpha scope:** does the referee hard-reject at all in Alpha, or only propose-and-confirm?
2. **The DoD/theme split:** theme clauses must NOT see the DoD (Fault 1). Confirm the fix shape.
3. **The loop:** only re-judge clauses a regeneration can actually change (Fault 2).
4. **The criterion:** if any automated clause survives into Alpha, which, and on what axis
   (causal-path, not endpoint)?
5. **The classifier:** confirm it moves off nano onto a capable model (the routing half).
6. **A NEW validation set** — the current fixtures are burnt (over-fitted across 4+ iterations),
   and the original 92% eval is missing. Nothing ships as "validated" without a fresh, inspectable
   eval and a named model.

Then, and only then, a build ticket.
