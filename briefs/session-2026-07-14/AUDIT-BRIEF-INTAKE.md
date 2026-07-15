# AUDIT BRIEF — INTAKE (FRONTEND + BACKEND)

**Run this brief TWICE, independently: once with Codex, once with Claude Code.**
Do not show either one the other's output. Where they disagree, the disagreement is the
finding — that is how the ResolvePM route audit found its real root cause.

---

## THIS IS AN AUDIT. DO NOT FIX ANYTHING.

**No code changes. No commits to source files. No deploys. No "while I was in there".**
You are producing a report. Somebody else fixes it, later, from a plan built on your report.

If you find something dangerous, **write it at the top in bold**. Do not fix it.

The one exception: you may commit **the report itself** to a branch, and you should.

---

## REPO

`ResolveOS-intake-frontend` (org `simeonfab`).
Production deploy: `4e6c16e`, live, building from `main`.
⚠️ The Vercel project is confusingly named **`resolve-intake-test-a`** — that is the *correct*
project despite the name. An earlier bug where it built from the wrong repo is **fixed**.

**Reference docs:** `resolve-docs` (org `simeonfab`). Clone it if it is not in scope.
**START AT `INDEX.md`** — it lists every brief and mockup with its path. Do not go hunting.
- Intake screen brief: `briefs/01-I1-intake-reskin.md`
- Mockups: the `ResolvePM_*_dc.html` files. INDEX gives their location.
- ⚠️ A filing branch (`claude/resolve-docs-filing-updates-d114cl`, `e350b16`) is **pushed but
  unmerged** and corrects `INDEX.md`. If main's INDEX looks stale, check it.

**If a spec you need is not in the repo: STOP and report the missing file.** Do not
reconstruct it from memory. Do not ask Simeon to attach it — if it exists, it exists in a
repo, and finding it is your job.

---

## PART 1 — FRONTEND AUDIT

**The standard, in Simeon's words:**
> *"The mockups I've posted should be what's there — and each button and action should have
> interactions that are actually working."*

Every screen of the intake flow. For each:

1. **Does it match the mockup?** Structure, hierarchy, spacing, type, colour. Report
   **MATCHES** or **DIVERGES — and say exactly how.** Screenshot each screen.
2. **Enumerate EVERY interactive element** — every button, input, link, toggle, chip.
   For each: what it is meant to do, what it *actually* does, and whether you **clicked it**.
   An unclicked button is an unaudited button.
3. **Every state:** loading, empty, error, success. Which exist? Which were never built?
4. **Anything on screen that is NOT in the spec.** Debug tools, test controls, leftovers from
   an earlier build. (ResolvePM had a live "Reset onboarding" button in Settings. Assume the
   same class of thing is here until you prove otherwise.)
5. **The I1 reskin is reported live but has NEVER been compared to its mockups.** This is the
   first real look.

Deliverable: a table — screen × element × expected × actual × verdict, with screenshots.

---

## PART 2 — BACKEND AUDIT

### 2.1 The call chain
Map every model call Intake makes, end to end. For each: what it is given, what it returns,
what validates it, and what happens when validation fails.

**Check specifically: is any call bundling a JUDGEMENT with a GENERATION?**
That fires 0/50 and it is the single most important architectural fact in this codebase. It
has been re-learned three times (the floor, the stand-up router, the orchestrator router).
Report every place generation and enforcement share a call.

### 2.2 Known findings — CONFIRM OR REFUTE, do not take on trust

**F25 — the outcome question accepts half an answer (HIGHEST LEVERAGE).**
Real session `cee60b5e`. Call 1 asked: *"If YGOBrain works, how will Yu-Gi-Oh! players be
better off, **and how would you know?**"* The founder answered: *"they would be able to make
better decks easier and keep track of the meta."* That gives the **beneficiary** and skips the
**observable change** entirely. Nothing required the second half. It was written to
`project_sessions.outcome` with **`outcome_confirmed = TRUE`** — so downstream it looks *more*
trustworthy than an inferred outcome, while missing the exact part that makes it usable.
- Confirm the mechanism in code. Where *could* the answer have been validated, and isn't?
- What else is validated by **presence** rather than by **content**? Audit all of it.

**F26 — fail-silent handoff.** `buildResolvePmHandoffPayload` (`index.html:~1787-88`):
`top_priorities: recommendationData?.topPriorities || []`. `validateRecommendationShape`
*does* require both fields (`resolve.js:215`, `:216-228`), so the `|| []` fires **only when
the entire `recommendationData` object is missing** — i.e. total failure gets handed off as an
empty-but-valid-looking payload. Confirm. Then find **every other silent fallback** in the
handoff path.

**F27 — Call 3 is a self-check.** The same model validating its own Call 2 output. Confirm it
still exists and report what it actually does today.

### 2.3 The handoff contract
Trace every field from Call 1 → gap questions → `collectGapAnswers()` → the handoff payload →
what ResolvePM inserts into `project_sessions`. **Report the exact key at every hop.** Flag
any rename, drop, or silent default. *(The `outcome` field is known-good — it works end to
end. Audit the rest with the same rigour.)*

### 2.4 Data reality
Query Supabase (`urmwilgdevgkdnntojnl`, **read-only**). Look at real `project_sessions` rows
— especially `cee60b5e`, the only one produced by the live outcome question. What is actually
written, what is null, what is silently empty?

---

## OUTPUT

One report. Every finding numbered, each with:
- **What** it is
- **Evidence** — file, line, query result, or screenshot. Not an assertion.
- **Severity** — critical / major / minor / cosmetic
- **Root cause** if you know it; **"unknown"** if you don't. **Do not guess.**
- **Proposed fix** in one line. Do not implement it.

Commit the report to a branch and push it. Report the SHA.

**Then stop.** The fix plan is built from both audits together, not from yours alone.
