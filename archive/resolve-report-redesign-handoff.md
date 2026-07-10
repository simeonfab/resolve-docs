---
status: historical
note: "Mid-process handoff; the thread continued into output-templates and the Step 1.5 pipeline work."
---

# Resolve Intake — Project Report Redesign: Handoff Document

Status: mid-process handoff, Claude session ending due to usage limits. This document carries full context for continuing this discussion elsewhere (ChatGPT, a fresh Claude session, or directly with Codex once the next step is done).

Suggested next destination: Strategic Product Director framing in your ResolveOS ChatGPT project — this is a product-definition discussion (what should the output contain, what should we ask for to get it), not an implementation task yet. Codex should only be brought in once the question-mapping step below is complete and turned into a concrete spec.

---

## The problem that triggered this work

The Resolve Intake "project plan" report (the downloadable output on Screen 4) was judged too thin and not genuinely useful compared to what real ResolveOS produces when adopting a project. There was also a structural process problem identified: screens and questions had been designed first, with the report built to fit whatever data those screens happened to collect — backwards. A placeholder/inconsistency bug was also flagged (e.g. roadmap phase labelled "Define" even when nothing is actually being defined) as a symptom of this same root issue — pieces being templated rather than fully reasoned from real input.

## The corrected process (agreed and sanity-checked)

Work backwards, not forwards:
1. Study a real, known-good ResolveOS output to understand what "small input, rich output" actually looks like when done well
2. Define the Intake report to genuinely match that bar
3. Map backwards from the defined report to the minimum information actually needed from the user
4. Only then adjust/redesign the existing screens and questions to serve that — not the other way around

Why this is the right process, not just a preference: it directly targets the actual bug already found (gap-question plumbing bug — questions were being generated without a fixed target to fill, causing repeated/mislabeled questions). Defining the output schema first forces every question to justify itself against "does this fill a real field," which structurally prevents that class of bug. It's also the only way to actually test the core product promise ("small input, rich output, like real ResolveOS") rather than just assuming the screens are fine.

One acknowledged risk, not yet resolved: working backward from a rich ResolveOS output (which has live GitHub/Notion access) risks defining report fields that need more input than "first page or two" can realistically supply from a non-technical Intake user with no repo. Some fields may need to be cut, marked as inferred/assumed, or accepted as less rich than the connected-ResolveOS version. This needs to be confronted directly during the question-mapping step, not discovered as a surprise.

## The real ResolveOS output that was analysed

A real "Existing Project Adoption" report for ResolveYGO (a different Resolve project, repo+Notion connected) was pasted and broken down section by section. Full original text is preserved separately if needed, but the analysis conclusion is what matters going forward:

**High-value, transferable to Intake:**
- Project Setup Report block (identity, objective, phase, highest-leverage activity, top 3 actions each with real reasoning)
- Readiness table (status + one honest sentence per area)
- "What should not change" / what-not-to-do-yet list

**Zero value for Intake users — process/audit metadata specific to having a real repo:**
- File paths, commit hashes, branch names, "context loaded" logging
- GitHub/Notion inventory listings
- "Recommended operating model" (which system owns what) — this only exists because ResolveYGO has two competing source-of-truth systems (GitHub + Notion). Intake users have exactly one input source (what they typed), so this entire layer doesn't transfer and should not be force-fitted in.

**One pattern worth stealing and generalising:**
- "Source-of-truth gaps discovered" in the original report meant contradictions between GitHub and Notion. The underlying pattern (catching internal inconsistency) generalises well: a messy project description very often self-contradicts (states one priority early, a different one later; mentions a deadline that conflicts with stated scope). This became "Things that don't quite add up" in the new report shape — only included if a genuine contradiction is actually present in the input, silent otherwise.

Conclusion reached: roughly 60% of the real ResolveOS report is repo/Notion-specific infrastructure that doesn't transfer. The lesson is not "make the Intake report longer to match," it's "match the sharpness and justification quality of the parts that do generalise."

## Final locked report shape (10 sections, fully agreed)

1. **Project identity / objective / phase** — fully unpacked, NOT compact. Grounded in what the user actually said as the base layer; only goes beyond that if genuinely necessary, and if so it must be clearly flagged as inference, never stated as fact. Length is not the constraint here, accuracy is.

2. **Single highest-leverage next action** — already exists in the current build, carries forward unchanged.

3. **Top 3 priorities** — already exists, carries forward unchanged.

4. **Longer-term speculative roadmap (NEW)** — distinct from the existing near-term 3-phase "what happens after this" sequence on Screen 3; do not merge these. This section answers "paint a picture of step 10" but resolves the tension with the no-invention rule by being explicitly labelled as reasoned extrapolation, not fact — e.g. framed as "If the immediate priorities go well, here's a plausible longer-term direction," never stated as a claim about the present.

5. **Readiness table** — re-scoped to categories that make sense WITHOUT a repo (e.g. direction clarity / planning maturity / readiness to act / evidence so far) — not the original's "discovery readiness" / "implementation readiness" labels, which assume code/repo state exists.

6. **What not to do yet** — confirmed, carries the same spirit as the original's "what should not change."

7. **Risks / things to be aware of (NEW, reframed)** — this went through a real revision. Originally proposed as "what's at stake if nothing changes," which was explicitly rejected as too emotionally loaded / consequence-framed and at risk of inventing drama. Reframed as observational "risks / areas to be aware of" — same underlying value (user should know what could go wrong) but framed as observation, not warning. Must be strictly grounded only in what the input actually supports — same no-invention discipline as everything else.

8. **Things that don't quite add up (NEW)** — only appears if a genuine contradiction exists in the user's actual input; says nothing if there isn't one. See "pattern worth stealing" above for origin.

9. **Sharing paragraph (NEW)** — one short, clean paragraph specifically written so the user could copy-paste it to explain the project to someone else (a partner, investor, family member). This is NOT a recap for the project owner — it must be written for the person being shown it. Explicit rationale given: this targets the social/communication blocker directly (talking about the project with someone else is often the actual blocker to progress), which is a distinct value from project clarity itself, and ties to sales/marketing being identified as a currently-missing but important future area of ResolveOS more broadly.

10. **How to use this (NEW) — likely the most important section, not a minor closing add-on.** This is the literal bridge between "Resolve gave me clarity" and "I'm now doing something with it elsewhere." States explicitly: do not end the report on this — note it gives practical direction on what to physically do with the document next, including the ability to paste relevant parts of it into another AI tool (ChatGPT/Claude) and keep working there. This section is effectively the test of the whole Intake product thesis: someone uses Intake, the output is good enough that they immediately take it elsewhere and start working — that IS the success condition for this entire test phase, stated explicitly by the project owner.

No closing question anywhere in the report (decided in an earlier session) — this is a final, immediately usable handoff document, not the start of another interaction loop with Resolve itself.

## What has NOT been done yet — the actual next step

The report shape above is fully locked. What has NOT yet happened, and is the next piece of work:

**Map the 10 sections backwards to exactly what minimum information needs to come from the user to genuinely fill all of them — without overloading the early input screens.**

Standing principle to respect during this mapping (reconfirmed multiple times across sessions): real user-inputted information should only happen early in the journey (first page or two). After that point, Resolve should be doing the reasoning/generation work, not continuing to extract more raw input from the user. This is a hard constraint on the mapping exercise, not a soft preference — if a report section seems to need information that can only come from extensive further questioning, that's a sign the section needs to lean more on Resolve's own reasoning/inference (clearly flagged as such) rather than on more user input.

This mapping has not started. This is the literal next step when work resumes.

## Known existing technical context relevant to this work

- The live app pipeline currently has 3 (soon to be 4, including the project-plan call) OpenAI API calls, each grounded in real ResolveOS role/governance file text pasted into system prompts (Business Analyst for understanding extraction, Strategic Product Director for recommendation, automatic QA self-check pass). This pattern should extend to whatever new/expanded report-generation call is needed once the question-mapping is done.
- A real, already-diagnosed bug exists in the current build: the two gap-question answer fields are hardcoded as "successDefinition" and "deadline" regardless of what the AI actually asked, causing mislabeled answers and repeated questions. A fix for this was already speced in a previous Codex prompt (pass actual question+answer pairs through instead of hardcoded field names) — confirm whether this fix has been pushed/deployed yet before assuming it's resolved.
- The project's standing principle: never invent facts; assumptions must always be explicitly labelled as assumptions, not stated as fact. This applies to every new report section above, especially the speculative roadmap (#4) and risks (#7).
- Code changes in this project are made via Codex (which has GitHub push access), not by Claude directly — Claude produces specs/prompts, the user pastes them into Codex, Codex implements and pushes. Standing principles already given to Codex in prior prompts: modular/extensible code, don't touch anything not explicitly listed, ask for clarification rather than guess if context is missing.
- Repo: simeonfab/ResolveOS-intake-frontend — public/index.html (frontend) and api/resolve.js (serverless function, OpenAI pipeline).

## ROUND 2 UPDATES (continued same session)

### "Only show if earned" — elevated to a structural rule, not a one-off caveat

This is the most important refinement from this round. It applies to the ENTIRE report, not just Risks and the now-cut "Things that don't add up" section:
- Top priorities: show as many as genuinely exist (could be 1, could be 3) — never pad to a fixed count
- Risks: only renders if real risks are actually identifiable from the input — if none surface, the section is omitted entirely, not filled with thin/invented content
- This same discipline applies to every other section — nothing should appear "for show." If a section isn't genuinely earned by the actual input, it should not render.

### "Things that don't quite add up" — CUT (confirmed, finalised)

Agreed redundant once Risks exists properly as its own honestly-gated section. Removed from the report shape.

### Final two screen-2 questions — RESOLVED

- Question 1: **"What are you most worried about with this project?"** — confirmed, needed.
- Question 2: **DROPPED.** The candidate ("Who would you most want to share this with?") was considered but rejected. Reasoning: the sharing paragraph (section 9) can be written well as "something you could show someone outside the project to explain it" without knowing the specific named audience — knowing the audience would only make it marginally more personalised, not meaningfully better. Per the "don't add friction unless genuinely earned" principle, that's not a strong enough justification for a second question the project owner was already uneasy about. Net result: **Screen 2 has exactly ONE gap-question slot going forward, not two.** This is a meaningful structural simplification from the current build (which has two slots) — confirm this is reflected correctly when the Codex spec is written; the second input box/slot should be removed from Screen 2 entirely, not just left empty.

### Updated final report shape (10 sections -> 9 sections after the cut)

1. Project identity/objective/phase — unpacked, grounded, NOT compact
2. Highest-leverage next action
3. Top priorities — as many as genuinely exist, never padded to a fixed count
4. Speculative long-term roadmap — explicitly labelled as extrapolation, not fact
5. Readiness table — re-scoped categories (no repo-assuming language)
6. What not to do yet
7. Risks — only renders if real ones exist; never invented to fill the section
8. Sharing paragraph — written for the person being shown it; audience-agnostic ("someone outside the project"), since the audience-naming question was cut
9. How to use this — the practical bridge to acting elsewhere; the actual success condition for this whole test phase

(Note: original numbering had 10 items including "things that don't add up" at position 8 — that's now cut, so what was 9/10 are now 8/9.)

### THE BIG FINDING — guardrail/implementation gap diagnosed (critical, drives next major piece of work)

The project owner correctly identified that the current live-app implementation is NOT properly "running ResolveOS" — it's a thinner approximation. Specifically:

**Current state (the problem):** Each API call's system prompt contains PARAPHRASED/EXTRACTED fragments of ResolveOS role and governance file text, written once during a Claude conversation and hardcoded as static strings into api/resolve.js. This is meaningfully weaker than how ResolveOS is actually used everywhere else (manually pasting full real files into a ChatGPT project).

**Why this matters concretely:** a single extracted sentence in a system prompt is much easier for the model to drift from over a generation than a full governance file, which usually reinforces a rule from multiple angles (the rule, the reasoning, edge cases, related rules). This directly explains why "don't invent / don't pad" hasn't been reliably enforced in testing so far — the enforcement mechanism was always thin.

**THE AGREED FIX (Fix A) — to be implemented BEFORE any Codex prompt is written, planned properly first:**

Store the actual, real ResolveOS role/governance .md files in the repo itself, and have each API call read the FULL VERBATIM file content at request time, injecting complete real files into the system prompt — not paraphrased fragments. This is the automated version of the same mechanism the project owner already uses manually (paste full files into a ChatGPT project, work from real context).

PROPOSED call -> file mapping (not yet implemented, needs review before Codex prompt is written):
- **Call 1** (understanding extraction): full `02-roles/business-analyst.md`
- **Call 2** (recommendation): full `02-roles/strategic-product-director.md` + full `06-governance/project-readiness.md` (assumption/evidence/confidence model lives here)
- **Call 3** (QA self-check): full `06-governance/project-readiness.md` + this call becomes the enforcement point for the new structural "only show if earned" rule across the WHOLE report, not just risks — turning it into something actively checked, not just hoped for via prompt wording
- **Call 4** (expanded project plan report covering all 9 sections above): likely needs Business Analyst + Strategic Product Director + project-readiness.md combined, since it spans multiple report sections with different reasoning needs

Source files confirmed still accessible (local repo path used during this session: /home/claude/resolveos/02-roles/ and /home/claude/resolveos/06-governance/ — contains business-analyst.md, strategic-product-director.md, project-readiness.md, plus others not yet mapped to a call: implementation-engineer.md, product-manager.md, qa-tester.md, technical-strategy-lead.md, architecture-decisions.md, codex-working-rules.md, decision-maker-reporting.md, duplication-control.md, extraction-migration-guardrails.md, source-of-truth-rules.md, update-process.md).

**Fix B (logged as a deliberate follow-up, NOT this round):** A more complete architectural version of the same idea, using ResolveOS's actual `03-skills/` structure — breaking relevant behaviour into discrete named skill files and attaching the specific right skill to the specific right call, rather than one undifferentiated role file per call. Bigger scope, deserves its own dedicated round once Fix A proves the basic full-file-injection mechanism works in practice.

**Process instruction for whoever picks this up:** Fix A's actual technical plan needs to be walked through and reviewed BEFORE a Codex prompt is generated — this was explicitly requested, do not skip straight to writing the Codex prompt. The plan above is a starting proposal, not yet finalised/approved in detail (e.g. exact mechanism for how a Vercel serverless function reads bundled markdown files at request time still needs to be confirmed/specified).

## ROUND 3 UPDATE — Report-as-source-of-truth architecture (important refinement)

**The insight:** the project owner wants ONE underlying generated body of project understanding, with Screens 2, 3, and 4 functioning as progressive VIEWS into slices of that same material — not separately-generated content that happens to roughly agree. The final downloadable report should be the complete version of the exact same material already shown piece-by-piece across the screens, not a fresh independent regeneration that risks subtly disagreeing with what the user already saw.

**Current gap this closes:** as planned, Call 4 (the project plan) would still freshly regenerate even the sections that overlap with Calls 1/2's existing output (project identity, highest-leverage action, top priorities, what-not-to-do-yet) — risking the AI phrasing the same underlying facts slightly differently the second time, even from the same source material.

**The fix (cleaner to build, not more complex):** Call 4 should REUSE the actual already-generated content from Calls 1 and 2 VERBATIM for the overlapping report sections, and only freshly GENERATE the sections that have no screen equivalent yet:

REUSED VERBATIM from Calls 1/2 (no fresh generation):
- Project identity / objective / phase (section 1) — from Call 1's understanding output
- Highest-leverage next action (section 2) — from Call 2's recommendation output
- Top priorities (section 3) — from Call 2's recommendation output
- What not to do yet (section 6) — from Call 2's recommendation output (or wherever this currently lives)

FRESHLY GENERATED in Call 4 (no screen equivalent exists yet):
- Speculative long-term roadmap (section 4)
- Readiness table (section 5)
- Risks (section 7) — informed by the new "what are you most worried about" answer
- Sharing paragraph (section 8)
- How to use this (section 9)

This means Call 4 becomes an ASSEMBLY function (combine real prior outputs + generate the genuinely new sections) rather than five-plus sections independently regenerated and hoped to align. This is simpler to build correctly, not more complex, and directly removes the consistency-risk the project owner flagged.

The "nothing to see here yet, wait a minute" UX treatment for sections with no genuine content (e.g. Risks when none are found) is logged as a future dedicated UX question, not solved now — current behaviour stays: omit cleanly, no placeholder.

## IMPLEMENTATION PLAN FOR CODEX (laid out, not yet turned into a final prompt - see note below)

This is the full scope of what needs to be built. Whoever generates the actual Codex prompt(s) from this plan should embed the current real file content (public/index.html, api/resolve.js) as the starting point, same pattern as all previous Codex prompts in this project.

### Part 1 — Fix A: Real full-file injection (replaces paraphrased fragments)

1. Add the actual ResolveOS markdown files to the repo (a new folder, e.g. /resolveos-context/roles/ and /resolveos-context/governance/), containing the FULL VERBATIM text of:
   - business-analyst.md
   - strategic-product-director.md
   - project-readiness.md
2. Update api/resolve.js so each call reads the relevant file(s) at request time (via Node's fs module, reading from the bundled deployment files - confirm this works correctly in Vercel's serverless function environment, since file bundling/inclusion needs to be configured correctly for the function to access them at runtime) and injects the FULL file content into the system prompt, replacing the currently-hardcoded paraphrased fragments.
3. Call -> file mapping:
   - Call 1 (understanding): full business-analyst.md
   - Call 2 (recommendation): full strategic-product-director.md + full project-readiness.md
   - Call 3 (QA self-check): full project-readiness.md
   - Call 4 (project plan, new): business-analyst.md + strategic-product-director.md + project-readiness.md (all three, since it spans multiple reasoning needs)
4. Watch for prompt-length/cost implications of injecting multiple full files into Call 4 specifically - flag if this becomes a real practical concern, don't just silently accept a much slower/more expensive call without surfacing it.

### Part 2 — "Only show if earned" structural rule (enforced in Call 3)

1. Call 3 (QA self-check) needs an explicit, real enforcement instruction: check every section of whatever it's validating for padding or invented content not genuinely supported by the source input. This applies across the whole report now, not just risks.
2. Specifically for sections with a variable/optional nature (top priorities - could be 1 or could be 3; risks - could be zero), Call 3 should verify the count/presence is genuinely earned, not padded to look complete.

### Part 3 — Screen 2 simplification (single gap-question, not two)

1. Remove the second gap-question input slot from Screen 2 entirely (not just leave it empty/unused).
2. The single remaining question should be specifically: "What are you most worried about with this project?" - replacing whatever the current dynamic gap-question generation produces for slot 1, OR confirm whether this should be a FIXED question every time (not AI-generated per session) given it's now explicitly defined. (This needs deciding before the Codex prompt is written - see open question below.)
3. Update the existing gap-question plumbing fix (paired question+answer structure, already speced in an earlier prompt) to work with a single question instead of two.

### Part 4 — Call 4 restructure as assembly function (the 9-section report)

1. Call 4 (runProjectPlanCall or equivalent) takes confirmedUnderstanding (Call 1 output) + recommendationData (Call 2 output, post-QA-check) + the single gap answer, and:
   - REUSES verbatim: project identity/objective/phase, highest-leverage action, top priorities, what-not-to-do-yet (pulled directly from the existing Call 1/2 outputs, not regenerated)
   - GENERATES fresh: speculative roadmap, readiness table, risks (only if genuinely present), sharing paragraph, how-to-use-this
2. Schema/validation updated to match the 9-section shape (down from the previously-speced 10, since "things that don't add up" is cut).
3. Risks section: validation must allow for a genuinely empty/absent result (not force a minimum array length the way other sections do) - this is a deliberate exception to the usual "must have at least N entries" pattern used elsewhere, specifically because of the "only if earned" rule.
4. Top priorities: similarly, validation should allow a variable count (e.g. 1-3) rather than requiring exactly 3.

### Open question to resolve before writing the final Codex prompt

Should "What are you most worried about with this project?" be a FIXED, hardcoded question every session, or should it remain part of the AI-generated dynamic gap-question system (just reduced to one slot)? Leaning toward FIXED, since it was explicitly defined as a specific, deliberate question rather than something that should vary - but this wasn't explicitly settled and should be confirmed.

## ROUND 4 UPDATE — "What are you most worried about" question: DROPPED (final resolution)

After extended back-and-forth, this is resolved cleanly:

**Decision: the dedicated "what are you most worried about" question is DROPPED entirely. It does not get added to Screen 2.**

**Reasoning (the deciding insight):** the actual goal of this test phase is that the user finishes the flow feeling clarity and confidence - "yes, I'm about to do something good" - not anxiety or being handed problems they hadn't already surfaced themselves. A direct question that fishes for a risk/worry runs directly against that goal. There was a real, valid tension the project owner correctly identified between two different principles in play - "if the user comes to me with a problem, I should help surface more" (a later-stage, deeper-relationship principle) versus "this test phase should end on clarity and confidence, not surfaced anxiety" (the correct principle for THIS stage) - and the second one wins for this phase. This is logged as a real distinction worth remembering: surfacing additional problems proactively may become appropriate at a LATER stage of the product (after trust is established, in an ongoing relationship), but is wrong for a first-touch clarity test.

**What this means for Risks (report section 7):** it stays in the report shape, but becomes PURELY INFERRED - derived only from whatever is already genuinely present/implied in the existing input and understanding (e.g. a stated deadline conflicting with stated scope, a dependency mentioned in passing), never asked for via a direct question. Same "only if earned" rule as already established elsewhere - if nothing genuinely surfaces, the section is omitted, never invented or forced.

**What this means for Screen 2:** NO new question is added. The existing dynamic gap-question system (already present in the current build, already has the question/answer plumbing-bug fix pending from an earlier session) stays exactly as it was. This makes the previously-open "fixed vs dynamic wording" question MOOT - there is no new dedicated question to decide that about.

This SUPERSEDES the "Screen 2 simplification (single gap-question, not two)" item under "Part 3" of the implementation plan below - Screen 2's existing two-slot dynamic gap-question system is UNCHANGED by this report-redesign work. The only Screen 2-related work item that remains relevant is the already-known plumbing bug fix (paired question+answer structure replacing the hardcoded successDefinition/deadline field names) - this was speced in an earlier session and may or may not already be deployed; confirm status before assuming it's done.

## Open question from Round 3 - NOW RESOLVED

The "fixed vs dynamic worry-question" open question is resolved by the above: it's moot, since the question itself was dropped. No further decision needed on this point.

## STATUS UPDATE — Codex prompt generated and sent

The final Codex implementation prompt covering all three parts (real file injection,
"only show if earned" enforcement, Call 4 restructured as an assembly function) has
been generated and handed to Codex for implementation. This included the three real
ResolveOS source files (business-analyst.md, strategic-product-director.md,
project-readiness.md) embedded in full as new files for Codex to add to the repo at
/resolveos-context/roles/ and /resolveos-context/governance/.

All design/planning decisions in this document are final and were included in that
prompt. Nothing further needs deciding on this report-redesign effort - what remains
is verifying Codex's implementation once it's pushed (check the commit hash, review
any assumptions/flags Codex reports back, confirm the gap-question plumbing bug fix
status it reports, and test the live deployment).

## Immediate next action when this resumes (SUPERSEDES previous version - genuinely final)

1. Review whatever Codex reports back (commit hash, any flagged assumptions, schema
   changes it made to Call 2 to support reuse in Call 4, confirmed status of the
   gap-question plumbing bug fix).
2. Test the live deployment - specifically check: do the screens still work correctly,
   does the project plan report now contain genuinely richer/grounded content, does
   the Risks section correctly omit itself when nothing genuine surfaces, does the
   report's overlapping sections (identity, top priorities, etc) actually match what
   was shown earlier on screens 2/3 rather than subtly differing.
3. If Codex flagged a prompt-length/cost concern for Call 4 (multiple full files
   injected), decide whether that's acceptable or needs addressing.
4. Fix B (skills-based architecture) remains logged as a separate future piece of
   work, not yet started.

