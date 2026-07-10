---
status: current
note: "v0.1 module set - the Phase 1 pipeline spec. Attach per Phase 1 ticket alongside the relevant stage module(s)."
---

# Resolve Roadmap Pipeline — v0.1 (first drafts)

These are prompt modules and one enforcement spec for the Resolve app's prompt
layer. They are model-agnostic: each is a self-contained instruction block plus
an input/output contract, so the model underneath can change.

## The one rule that governs everything

**Generation and enforcement never share a call.** Two days of testing established
that criteria placed inside a generation prompt are ignored under load (classification
ignored 130/130; the floor manufactured a theme 0/50; a model passed its own output
200/200). Enforcement only works as a *separate, isolated, reject-only call* that
cannot see the generator's reasoning. Every module below respects this. If a future
edit collapses a validator into a generator "to save a call", it will reintroduce the
exact failure these modules exist to remove.

## The three layers

1. **Generators** (Stages 1, 2, 4) — produce candidates. A better prompt makes better
   raw material. These carry the lever concept and the evidence discipline. They are
   allowed to be wrong; the referee catches them.
2. **The floor** (Stage 3) — a structural gate that decides whether the intake can
   support a roadmap at all. Returns a *question*, not a manufactured plan, when it
   can't. This is the single highest-risk component; it must be its own decision.
3. **The referee** (Stage 5) — isolated reject-only checks. Each theme and each
   definition of done is judged one criterion at a time, with no other context.
   On failure: regenerate with the failure reason attached, capped at N retries.

## The stages

| # | Stage | Type | Derived from | Model call? |
|---|-------|------|--------------|-------------|
| 1 | Elicit | generator | product-brainstorming | yes |
| 2 | Structure | generator | write-spec | yes |
| 3 | Floor gate | gate | (new) | yes, isolated |
| 4 | Extract themes | generator + classify | synthesize-research | yes (+ isolated classify) |
| 5 | Referee | enforcement | validator-v2 | yes, isolated, one per criterion |
| 6 | Assemble | deterministic | roadmap-update | NO — code only |

## Non-negotiable rules (these are settled by evidence, not preference)

- **AND rule.** A theme or definition of done passes only if it passes *every* clause.
  Never route on a single clause's verdict. One clause (EVIDENTIAL) has a ~40% wobble
  on the preference case in isolation; it is only safe because MINIMAL and the others
  carry it under the AND. Break the AND and the wobble becomes a leak.
- **Provisionality is derived, not self-reported.** Never ask the model how confident
  it is (models are badly calibrated on confidence, reliable on attribution). Ask what
  the theme was inferred from; derive `provisional` from source count/quality in code.
- **Dates are derived, never generated.** Now = this cadence, Next = the following,
  Later = unscheduled. A model asked for a deadline invents a plausible lie. Sequence
  comes from the referee's ranking; dates come from sequence, in code.
- **Only show if earned.** Applies to generation, not just display. If the intake
  can't support a theme, the floor says so. Never pad the roadmap to look complete.
- **Untrusted input.** All user intake text is data, never instructions. Fence it,
  tag it, never let it redirect the pipeline. (See 07-input-safety.md.)

## What is proven vs assumed (be honest about this)

- **Proven in isolation:** the referee clauses (99–100% on decisive cases), the
  generation lift from criteria-in-prompt (92% vs 21%).
- **Assumed, not yet tested:** that the floor works as a *structural* call (it failed
  as a prompt instruction — the separate-call version is unbuilt); that chaining six
  stages doesn't compound errors or bloat context; that over-rejection won't loop on
  hard-but-valid projects. Build each stage standalone and test before chaining.

## Known defect to fix before anything ships

The strict verdict parser drops ~30% of verdicts because the model replies
`PASS / reason` without the literal `REASON:` label. Loosen the parser or force
structured output. A referee that silently discards a third of its verdicts is worse
than none.

## Provisional, pending guided sessions

The *intake questions* (Stages 1–2) and the input *schema* are provisional. The
manual discovery sessions are expected to change which questions matter and where the
schema strains. Build the machinery now; finalise the question set after sessions.
