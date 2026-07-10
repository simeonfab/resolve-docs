---
status: superseded
superseded_by: "strategy/build-phases-and-commercial-model.md"
note: "Pre-Notion-era direction doc, paired with the intake-focus directive. Phase framing superseded 8 Jul."
---

# Resolve — Where We're Going (Roadmap Clarity)

Status: reference document for AI tools working on Resolve
Pairs with: `resolve-intake-focus-directive.md` (the current, narrower short-term rule)

This is the bigger picture. It does not change the short-term directive — Intake validation is still the only thing being built right now. This exists so any AI helping with Resolve work understands the destination without mistaking it for current scope.

## Current position

We are at the very start of Phase 1, and inside Phase 1 we are at the narrowest possible point: one route (Existing Messy Project), one prototype, one internal test completed, polish in progress, external testers not yet found.

Nothing below this point is approved to build. It exists so direction is clear, not so it can be started early.

## The corrected phase order

The original roadmap put commercial validation (Phase 5) after three phases of product-building (2, 3, 4). That's the wrong order — it risks building most of the product before learning whether anyone will pay for any of it. Validation should run early and in parallel, not at the end.

```text
Phase 1 — Resolve Intake                         [in progress, narrow]
Phase 2 — Service-Led Validation                 [moved earlier — runs alongside/after Phase 1]
Phase 3 — Project Shaping Routes (more journeys) [only after Phase 1+2 evidence]
Phase 4 — ResolvePM Light                        [only after shaped projects need continuity]
Phase 5 — ResolvePM Full / Operational Cockpit   [only after Light is insufficient for real users]
Phase 6 — Productisation                         [only after repeated, paid, service-led demand]
```

## Phase by phase — what each means and what unlocks it

### Phase 1 — Resolve Intake (current)
**What it is:** The entry point. A user brings in messy context and gets a clear understanding plus a path to execution, without needing to know what ResolveOS or ResolvePM are.
**Done now:** Existing Messy Project route, six-screen static prototype, one internal test.
**Unlocks Phase 2 when:** the route works for 2-3 external testers (see focus directive for exact criteria).

### Phase 2 — Service-Led Validation (moved up)
**What it is:** Manually deliver the value of Resolve as a service — you, personally, walking 2-5 real people/clients through intake-to-roadmap — to find out if anyone will pay for this before any more product gets built.
**Why it comes here, not later:** This is the cheapest, fastest way to learn whether the core idea has commercial legs. No more building should happen on instinct once this is running — decisions should follow what people actually pay for and ask for again.
**Tests:** Will users pay for guided setup? Which journey has the strongest pain? Do they value the output enough to share it or come back? What repeats across people?
**Unlocks Phase 3 when:** You have repeatable evidence of demand and a clear sense of which journey (route) is actually wanted, not just the one you happened to prototype first.

### Phase 3 — Project Shaping Routes (more journeys)
**What it is:** Once Existing Messy Project is proven, add the next-most-validated route — likely New Idea/Startup Concept or AI-Assisted Build/Prototype, based on what Phase 2 actually showed demand for, not the priority list as currently guessed.
**Unlocks Phase 4 when:** Shaped projects from these routes show a real need for ongoing structure (not just a one-time output).

### Phase 4 — ResolvePM Light
**What it is:** A lightweight workspace (roadmap, milestones, next actions, risks, continuation) for people who need more than a one-off intake output but don't yet need a full operational cockpit.
**Why it matters:** Stops every user being funnelled into ResolvePM Full prematurely — protects against the "ResolvePM dominates" risk already flagged in the risk register.
**Unlocks Phase 5 when:** Real users of Light hit its ceiling and ask for more (dashboards, work tracking, ongoing operational briefing).

### Phase 5 — ResolvePM Full / Operational Cockpit
**What it is:** The existing ResolvePM app/cockpit, positioned as the deep-end surface for people already running active, ongoing work — not the default destination for everyone.
**Unlocks Phase 6 when:** There's a repeatable, paid pattern across Phases 1-5 worth packaging as a real product.

### Phase 6 — Productisation
**What it is:** Turning the proven, repeated service pattern into an actual hosted product. This is explicitly last, and explicitly not guaranteed — the roadmap principle says repo/code convergence happens "last, if ever."

## The one rule that governs all of this

**Each phase is unlocked by evidence from the phase before it, not by confidence, time elapsed, or a good idea showing up.** If an AI tool working on this ever proposes starting Phase 3, 4, 5, or 6 work, the question to ask is: "What evidence from the phase before this said we were ready?" If there's no answer, it's not time yet.

## What this means for "where are we going"

Short version, in plain language: we're going from *one tested entry point* to *proof that people will actually pay for this in some form* to *more entry points, chosen by evidence not guesswork* to *light structure for people who need continuity* to *a full workspace only for people who outgrow the light version* and only then, maybe, *a real hosted product*.

Everything past Phase 2 is direction, not commitment. The plan can and should change once real evidence comes in — that's the point of sequencing it this way instead of building all seven layers from the layer model up front.
