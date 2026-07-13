# RESOLVE ALPHA — SESSION HANDOFF

**Date:** 13 July 2026. **Written to be picked up cold.**

---

> ## ⚠️ FILING NOTE — READ BEFORE ACTING ON THIS DOCUMENT (added 13 July 2026)
>
> This handoff is filed for the record, but two of its claims are now known to be
> **stale or false**. Do not act on them:
>
> - **§1 "UNRESOLVED INCIDENT" is CLOSED — there was no incident.** The diagnostic ran
>   and `main` was git-verified intact on 13 July. There is nothing to recover. Ignore the
>   §1 recovery instructions entirely.
> - **§2's claim that the string-concat generator "is now gone" is FALSE.** Ticket 1.4
>   (the generation replacement) was **never committed anywhere** — not to `main`, not to
>   a branch. The old string-concat generator was not removed. Treat §2's 1.4 bullet and
>   the "That is now gone" line as **not true**.
>
> **Authoritative live state lives in Notion → `Resolve Current Context` (under 🎯 Current),
> not in this document.** Where this handoff and Notion disagree, Notion wins.

---

## 🚨 1. CURRENT STATE — UNRESOLVED INCIDENT, READ FIRST

**A chat "killed everything" and the damage is not yet assessed.** A read-only diagnostic
prompt was issued; **results had not come back when this chat ended.**

**Do not assume anything below is safely on `main` until that diagnostic is run.**

The diagnostic asks for: `origin/main` HEAD + last 30 commits · whether `CLAUDE.md` runs
rules 1–12 · whether floor/referee/check-theme/global-bar/team-chat/N2/N3/N4/N6/N7 exist
on main · `git reflog` + `git log --all` for orphaned or force-pushed commits.

**Why recovery should be straightforward:** every piece of work this session was **pushed
to a remote branch before being merged**. Even if `main` was mangled, the commits exist on
the remote and can be re-merged. Nothing was merged from an unpushed local branch.

**Last known-good `main` SHA (cap-pm), before the incident: `3d6b560`** (the N8 merge).
Other recent SHAs: N5 merge `f6058fd` · rules 11+12 `f24d919` · halt screen branch
`9c5de37` · N8 orchestrator branch `6a474b1`/`d1cb2e6`.

**First action in the next chat: run the diagnostic, establish the truth, then recover.**

---

## 2. WHAT THIS SESSION DID

### The pipeline (Phase 1) — the core work
The roadmap generator was **string concatenation with no model call**: themes copied
verbatim from `top_priorities`, titles glued as `"{action} for {theme}"`, priority
hardcoded `'medium'`, dates null, `what_not_to_do` captured and never read. **That is now
gone.**

- **1.0 Schema** — added `project_sessions.outcome` + `outcome_confirmed`,
  `initiatives.inferred_from` + `provisional`, and the `refusals` table (with RLS).
  Migration: `phase1_pipeline_outcome_provenance_refusals`.
- **1.1 Referee** — isolated per-clause calls, AND rule, regenerate loop (N=2), strict
  `json_schema` output. **Proven:** a deliberately bad theme was rejected, the loop fired,
  refusals rows were written. **Critical finding: LEVERABLE alone passed both an artifact
  and a restated goal — only the AND rule caught them. Never route on a single clause.**
- **1.2 Floor** — split isolated gate calls, temp 0. Gate 1 halts; Gate 2 flags.
- **1.4 Generation replacement** — deleted `buildThemeDrafts`, `buildTodoDrafts`,
  `canStandAsTheme`. Real generation: separate generate → classify → `what_not_to_do`
  filter → choose → referee calls. Deterministic assembly (priority from Now/Next/Later,
  dates from sequence, provisional computed from `inferred_from`).
- **Halt screen** — wired. Found that `evaluateFloor` had **zero consumers**: the floor
  was running, passing evals, logging refusals, and **no user could ever see it.** A prior
  session had reported it as wired and screenshotted. It wasn't.

### The product reversal (the most important thing that happened)
The floor's Gate 2 was going to **halt** the user and ask *"what work would actually move
this?"* Every phrasing of that question failed, and the reason is the finding:

> **The founder came to us for direction. Asking them what to work on hands our job back.**

If we know the **outcome** and the **current state**, deriving the path between them **IS
the product.** So Gate 2 failing is **our** failure, not theirs.

**Decisions taken:**
- **Gate 2 no longer halts — it is a PROVISIONALITY SIGNAL.** Generate anyway, mark
  affected themes `provisional`, log to `refusals` as a diagnostic.
- **Gate 2 firing often means our INTAKE QUESTIONS are wrong.** Fix upstream, never at the
  gate.
- **Gate 1 still halts** (no outcome = nothing to aim at) — but Intake now asks the
  outcome question, so it should rarely fire.
- **Product thesis changed** from *"structure and refusal"* to **"Not just any direction —
  the RIGHT direction."** Compass/map motif. Filed; the master plan is updated.
- **The Sharpen Loop** (deliver value first, honestly labelled, then offer to improve it)
  → **Beta epic**, deliberately deferred.

### Intake
- **Outcome question shipped and LIVE.** Call 1 extracts the outcome if the founder already
  stated it (asks nothing), or asks ONE project-specific outcome question as
  `gapQuestions[0]`. Flows through `collectGapAnswers()` → handoff →
  `project_sessions.outcome` + `outcome_confirmed`. Validated in Call 1's required shape.
- **I1 reskin — LIVE in production.** All five screens rebuilt to the mockups.
- **Found and fixed:** Intake's Vercel project was building from an **old repo**
  (`resolve-intake-test-a`) — pushes to `ResolveOS-intake-frontend` weren't deploying at
  all. Now correctly wired.

### Frontend
Built, browser-verified, merged: **N2** (OS reveal) · **N3** (dashboard, four cards,
deterministic `winForToday`) · **N4** (work tab) · **N5** (team tab + agent chat, router
proven 3/3) · **N6** (knowledge base) · **N7** (settings) · **N8** (global bar /
orchestrator — router proven 5/5, confirmation-gated writes) · **I1** (intake reskin) ·
**halt screen**.

### Infrastructure
- **All four repos moved to `simeonfab`** — the cross-org read problem is dead.
- **Notion restructured.** `Resolve Current Context` (under 🎯 Current) is the single
  live-state entrypoint. Stale pages corrected or archived.
- **MCP configs committed to repos** so setup travels between machines.
- **`shared-context-contract.md`** filed in `resolve-docs`.
- **`CLAUDE.md`/`AGENTS.md` rulebook now runs 1–12** — four competing versions reconciled
  into one.

### The rules added (each born from a real failure this week)
- **Rule 9 — repo sync discipline.** Fetch before you build; flag unmerged branches
  touching the same files; push after (a local-only commit is not durable).
- **Rule 10 — never ask Simeon to hand-attach a file that already exists in a repo.**
- **Rule 11 — verification means an ARTIFACT, not a claim.** Screenshots, query results,
  SHAs. A passing build is not visual verification. Never end by telling Simeon to go check
  it himself.
- **Rule 12 — built ≠ wired ≠ shipped.** A feature isn't done until a user can REACH it.
  An unreferenced component is dead code. *(Born from three real incidents: `floor.question`
  with zero consumers; the global bar shell with no engine; the team chat merged with no
  link to it.)*

---

## 3. OPEN PROBLEMS — investigate or defer

| Problem | Verdict | Where |
|---|---|---|
| **Floor CRITERION is conceptually wrong.** "Lever = no natural endpoint" is neither necessary (a bounded intervention — installing a medication-reminder system — can be the true causal lever) nor sufficient ("do more marketing" is endless and constrains nothing). The right axis: *is there an actionable causal path specific enough that the model doesn't have to invent the strategy?* Confirmed by independent adversarial review. **The architecture is proven and correct; only the criterion is wrong.** | **DEFER — Beta spike.** No longer a monetisation blocker now that Gate 2 flags rather than refuses. **Do NOT attempt another prompt pass — four were tried.** | `resolve-pipeline/FLOOR-GATE-DESIGN-FINDING.md` |
| **The Sharpen Loop** — the opt-in question round, the reveal fork, the dashboard prompt, "I trust this" acceptance. | **DEFER — Beta epic.** Deliberately: we cannot design good sharpen questions until Alpha's `refusals` table shows where Gate 2 actually fails on real founders. | `briefs/BETA-EPIC-SHARPEN-LOOP.md` |
| **ResolveOS four-role model has NO extraction doctrine.** `business-analyst.md` (327 lines of requirement-decomposition doctrine) was compressed to ~3 lines in `product-director.md`. Intake's Call 1 depends on it. | **DEFER — roles epic.** But it is a real design gap the refresh created and nobody noticed. | Roles Epic (Notion) |
| **`top_priorities` and `what_not_to_do` are UNVALIDATED in Intake's Call 2.** `validateRecommendationShape` doesn't require them — they can arrive **empty and silent**, and the frontend papers over it with fallbacks. | **INVESTIGATE — small, real.** Fix the validation upstream in Intake. The new pipeline already treats them as possibly-absent. | Intake `api/resolve.js` |
| **Call 3 (Intake's QA self-check)** — the same model checking its own Call 2 output. The self-validation gap (models pass their own work ~100%). Superseded by the referee. | **DEFER.** Needs an Intake deploy. Near-decorative, not harmful. | Intake |
| **ResolveOS filing branches unmerged, and the two DISAGREE.** `claude/resolveos-docs-filing-2` (`0668c55`) is the coherent four-role tree; `filing-pass-2` has a mixed tree with all ten roles. **Use docs-filing-2.** | **DEFER.** Trap for whoever picks it up. | ResolveOS repo |
| **Rules 11/12 not mirrored** into `resolve-docs/strategy/shared-context-contract.md`. | **DO IT** — small, separate repo, separate job. | resolve-docs |
| **Finding B — DOWNGRADED, not a blocker.** Intake's role-file copies are byte-identical to ResolveOS main; nothing is fetched at runtime; **Intake deploys fine.** Do NOT swap the role files — it would gut Call 1's grounding. | **NO ACTION.** | — |
| **Logo / compass identity.** Thesis is now "the right direction". | **Simeon's own track.** | — |

---

## 4. IS IT ALL ON PROD? — ⚠️ NOT CONFIRMED

**Intake: YES, live in production** (I1 reskin + the outcome question), verified deploy
READY.

**cap-pm: EVERYTHING WAS MERGED TO `main`** — but `main` deploys to production
automatically, and **the "killed everything" incident is unresolved.** So: **do not start
clicking through until the diagnostic confirms `main` is intact.**

**The end-to-end walk (once main is confirmed):**
Live Intake → describe a real project → answer the outcome question → sign up → the floor
runs → the roadmap appears → poke the global bar, open a role chat.

---

## 5. WHAT TO CHECK ON THE WALK-THROUGH

- **Is the roadmap actually GOOD?** This is the whole point. Would you act on it tomorrow?
- **Is anything INVENTED?** A theme or task that doesn't trace to something you said.
- **Are the themes actually themes**, or tasks/artifacts wearing theme clothes?
- **Are the definitions of done real** — would you *know* when one was met?
- **Do provisional markers appear**, and are they honest rather than alarming?
- **"Not quite" on a theme** — does the referee re-check feel right? *(This path has never
  been exercised by a real user.)*
- **The global bar** — try all five intents (navigate/find/ask/capture/create). Does the
  confirmation gate feel safe?
- **A role chat** — is the greeting really project-contextualised? Does the stand-up room
  pick the right responder?
- **⚠️ Animations and transitions are known to be rough.** A full manual motion/feel review
  is on the list and hasn't been done.

---

## 6. ALPHA ROADMAP — WHAT'S LEFT

**DONE:** the pipeline (1.0–1.4) · every frontend screen (N1–N8, R1–R4, I1) · the halt
screen · outcome capture live · infrastructure and rulebook.

**LEFT:**
1. **🚨 Recover from the incident.** Confirm `main` is intact.
2. **End-to-end walk on production** — Simeon, by hand. Produces the first real session.
3. **Run 4–5 real intakes** through live Intake (no real sessions exist yet — Simeon
   wanted end-to-end complete before sending anyone through).
4. **Full manual frontend review** — every screen, every animation, in the browser.
   Motion/feel is a human judgement and agents cannot do it.
5. **1.6 — the blind grade.** THE GO/NO-GO.
   - Build the **comparison harness**: reconstruct the old string-concat generator from git
     history, run both pipelines on the same real intakes, output two roadmaps stripped of
     any tell, randomised A/B.
   - Grade with **ChatGPT** (different provider than the builder) using
     `1.6-BLIND-GRADING-BRIEF.md` — criteria are **pre-registered**, deliberately written
     before seeing any output.
   - **Simeon does the forced choice himself:** *would I act on this?* A model can grade
     traceability; it cannot grade that.
   - **Brace for:** the old pipeline may invent LESS (it copied verbatim — its failure was
     blandness, not fabrication). **If the new pipeline invents more, the referee isn't
     working.** That's a real finding, catch it before Alpha.
6. **Close three dead branches:** `spike/resolve-ui-reference-v2`,
   `claude/live-dark-signin-layout-w52uzb`, `claude/roadmap-generation-quality-gdftvv`.

---

## 7. FILING PROMPT — run this, several files are NOT durable

```
Repo: resolve-docs. File these. Branch, commit, push.

Simeon will attach (they exist only in a chat output folder — NOT durable, which is
exactly why they must be filed):
  TICKET-N5-REVISED.md
  TICKET-N8-REVISED.md
  1.6-BLIND-GRADING-BRIEF.md
  1.3-REVISED-BRIEF.md          (if not already filed)
  SESSION-HANDOFF.md            (this document)

Placement: briefs/  — except SESSION-HANDOFF.md → briefs/handoffs/

In the SAME commit:
1. Mirror rules 11 and 12 from cap-pm's CLAUDE.md into
   strategy/shared-context-contract.md under CHECKPOINT. Rule 11: verification means an
   artifact, not a claim (screenshots, query results, SHAs; click through what you built;
   never tell Simeon to go check it himself). Rule 12: built ≠ wired ≠ shipped — a feature
   isn't done until a user can reach it; an unreferenced component is dead code.
2. Update briefs/11-N5-team-tab-agent-chat.md: REMOVE the "N5 rescoped to N5-lite —
   roster + intros only, no chat; chat is Beta scope" note. Superseded — chat shipped in
   Alpha.
3. Add all files to INDEX.md.

Report the commit SHA.
```

---

## 8. BRIEFING FOR THE NEXT CHAT

**Load first:** `Resolve Current Context` in Notion (under 🎯 Current) — the single
live-state entrypoint. Then `resolve-docs`.

**Your first job is the incident.** Do not build anything, do not merge anything, until
`origin/main` is confirmed intact. Run the diagnostic in §1.

**Then, in order:** Simeon walks the product end to end on production → runs a few real
intakes → the 1.6 harness gets built → the blind grade.

**Things that will trip you up:**
- **Everything you believe about repo state is a hypothesis.** Fetch first. This session
  had five separate incidents of confidently-asserted state being wrong.
- **Six parallel chats were running.** Each one's picture of `main` goes stale in minutes.
- **The floor's criterion is known-wrong and deliberately shipped that way.** Don't "fix"
  it. It's a Beta spike and four prompt passes already failed.
- **Gate 2 does NOT halt.** If you see a doc saying it does, that doc is stale.
- **Never bundle a judgement with a generation in one call.** It fires 0/50. This is the
  single most important architectural fact in the codebase, and it has now been
  re-learned three times (the floor, the stand-up router, the orchestrator router).
```
