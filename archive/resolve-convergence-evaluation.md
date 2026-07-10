---
status: historical
note: "Evaluation of the retired Resolve-Planning repo (pre-Notion migration). Advice absorbed into subsequent planning."
---

# Resolve Convergence — Evaluation Report

Based on a full read of the Resolve-Planning repo (README, SOURCE_OF_TRUTH, current plan/decisions/todo, strategy docs, layer model, runtime surface doc, validation evidence, wife-test feedback, history, operating-model constraints, route priorities).

---

## 1. Are the individual ideas good?

**ResolveOS — yes, this is the strongest piece.** A reusable framework for context continuity, source-of-truth discipline, and project state across AI sessions. Verified earlier against BMAD-METHOD and GitHub Spec-Kit: real competitors exist, but your specific angle (conversational, no-CLI, cross-tool source-of-truth arbitration) is a genuine, defensible niche. This idea stands on its own.

**ResolvePM — plausible, but unproven as described here.** It already has real app code and Supabase-backed workflows, which is more concrete than most of what's in this repo. But its value here is asserted ("operational workspace/product cockpit"), not evidenced — no tester feedback, no validation log entry exists for ResolvePM specifically. It's a real thing, but its place in this report is "exists," not "proven."

**Resolve Intake / Issue 16 — good instinct, weak execution so far.** The reframe from "generic hosted wrapper" to "guided intake and routing layer" is a real improvement — wrappers are commodity, routing-by-actual-need is more defensible. But the one test you've run (your wife, on a static six-screen mock) surfaced fairly fundamental usability gaps: unclear title, unclear purpose, inconsistent terminology, confusing journey separation. These aren't edge-case nitpicks — they're "does the user understand what this is" problems. That's a normal, expected outcome of a first test, not a red flag on the idea — but it means Intake is at "early sketch," not "validated layer."

**Orchestration layer (ResolveOS Runtime Surface) — conceptually sound, currently the riskiest piece.** The idea — give the user a way to keep talking to ResolveOS, in-role, after intake, without forcing them into a full PM cockpit — is coherent and addresses a real gap (otherwise Intake hands off to either nothing or to ResolvePM Full with no middle ground). But it's the least concrete thing in the repo: no prototype, no test, explicitly blocked from implementation, and described almost entirely as relationships between other layers rather than as a thing with defined behaviour. It currently exists as an idea about an idea.

**Verdict on individual ideas: three are good (ResolveOS strong, Intake promising, Runtime Surface coherent-but-abstract), one is unproven-but-plausible (ResolvePM).** None of them are bad ideas. The concern isn't idea quality — it's what happens when you stack four of them at once.

---

## 2. Did merging them make sense? Is there now a clearer value proposition?

**Be precise about what actually happened:** this isn't a 2-way merge of "ResolveOS + a frontend wrapper," as you described it conversationally. The repo defines **seven layers** (Resolve, Resolve Intake, ResolveOS, ResolveOS Runtime Surface, Resolve Workspace/ResolvePM Light, ResolvePM Full, AI Project OS Sprint) plus an eighth category (External Execution Tools), spanning **five separate repositories** (Resolve-Planning, ResolveOS, ResolveOS-beta, cap-pm-cockpit-alaria, ResolveOS-frontend-test). That is a materially larger scope than "I merged two things."

**Is there a clearer value proposition as a result?** Conceptually, yes — there's now a single sentence ("Resolve helps people turn ambiguous project and work context into clear operational direction") and a layer diagram that explains how the pieces relate. That's real progress over having two disconnected things with overlapping unclear purposes.

**But clarity-on-paper and clarity-to-a-user are different things, and you have evidence pointing at the gap.** Your one test — on the simplest, most concrete artifact in this entire repo (a six-screen static mock) — already found the title and purpose unclear, and found that terminology ("idea" vs "project" vs other words) was inconsistent enough to confuse a real person. If a six-screen mock isn't yet legible to one tester, a seven-layer product family with two named "PM" variants (Light/Full) and a "Runtime Surface" that's explicitly not yet a real thing is several orders more complex to communicate. The clarity you've gained is internal — it's clarity for you, as the builder, about how the pieces fit together. It is not yet evidence that an end user will perceive a clearer product than before.

**Does it compete more strongly against others?** Marginally, in narrative — "Resolve is an AI orchestration system with workspace surfaces" is a more complete pitch than "ResolveOS, a markdown framework" was alone. But against BMAD-METHOD and GitHub Spec-Kit specifically, breadth doesn't help you — they already do multi-phase, multi-surface, multi-tool orchestration, with actual shipped product behind it (not "Later — do not build yet" qualifiers attached to most of the surfaces). Adding more named layers makes your pitch longer without yet making it stronger, until each layer has its own evidence.

---

## 3. Reality check: how much can you actually do yourself, and the time-cost question

You said you think you went from something potentially saleable within a month to something that might now take up to a year. Based on what's actually in this repo, that's a fair and accurate read, not a self-critical exaggeration.

Concretely, here's what "the year-or-more version" now contains, that the "month version" didn't:
- A routing layer (Intake) that needs its own design, its own testing, and — per your own `route-priorities.md` — six more routes beyond the one currently prototyped, each needing separate validation.
- A second workspace tier (ResolvePM Light) that doesn't exist yet, sitting between Intake and ResolvePM Full.
- An entire orchestration surface (Runtime Surface) that is explicitly defined as not yet validated, not yet prototyped, and not yet implementation-approved.
- Five repositories to keep mutually consistent instead of one or two.

To your credit, the document set itself is unusually disciplined about not letting this turn into a build spiral — `do-not-build-yet.md` and `implementation-readiness.md` explicitly block backend, accounts, billing, persistence, ResolvePM/ResolveOS rewrites, and repo merges, and gate future implementation behind acceptance criteria and evidence. That governance is good practice, and it's consistent with the self-hosting governance model we built earlier — you're applying your own "don't let it run away" instincts here, and it shows. This significantly reduces the risk of silent scope explosion, but it does not reduce the scope that's already been defined on paper.

**Can you do this yourself?** You can clearly do the planning, documentation, governance, and prototype-copy work — you've already demonstrated that here, and it's high quality. What you cannot realistically do solo, at pace, alongside a full-time Senior Product Owner role and twin one-year-olds, is: design and usability-test seven layers, build and validate a router, stand up a "Light" workspace tier, and eventually build a real orchestration runtime — all before you've validated whether *anyone outside your household* wants any of it.

**The honest read on "did it make sense to merge":** Strategically, recognising that ResolveOS, ResolvePM, and Intake are related rather than competing was probably correct — they do address adjacent needs, and keeping them artificially separate might have caused its own confusion later. But you've designed the *full* converged architecture before validating even the *simplest* unconverged piece (the six-card Intake flow had one test, with usability problems still open). The merge wasn perform — the sequencing did. You went from "validate route A" to "architect seven layers across five repos," without route A being validated first.

---

## 4. What I'd actually do from here

1. **Freeze the layer model as a reference document, not a build target.** It's useful as a north star for how things might eventually relate. It is not a justified set of things to build next.
2. **Get back to exactly one open question: does the Existing Messy Project route work for a stranger, not your wife.** Everything else in this repo — Runtime Surface, ResolvePM Light, six more routes — should stay in "do not build yet" until that single route clears a second, external test.
3. **Resist adding the Runtime Surface or Light/Full split until Intake alone is validated.** Right now you have zero external evidence for any layer beyond the one six-screen mock. Adding more layers before that evidence exists is the same risk you flagged with ResolveOS governance: the system (in this case, you) generating its own justification for more structure without outside checking.
4. **If you want the "year" estimate to come back down toward "weeks," the lever is scope, not effort.** You can't out-work a 7-layer, 5-repo architecture solo on top of your existing constraints — but you can cut it back down to "Intake, one route, validated externally" and ship that, with everything else staying exactly where `do-not-build-yet.md` already puts it: written down, not built.
