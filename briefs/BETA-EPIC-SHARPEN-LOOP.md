---
status: beta-epic
opened: 2026-07-12
owner: Simeon
phase: BETA — deliberately OUT of Alpha scope
blocked_by: Alpha shipping and generating real `refusals` data
note: "Written to be picked up COLD. Contains the full reasoning, not just the conclusion.
  The reasoning is the valuable part — it took a long session to get here and it reversed
  the product's core framing."
---

# The Sharpen Loop — Beta epic

## The one-line change

**Old framing:** "structure and refusal" — Resolve refuses to build a roadmap when the
input can't support one.

**New framing:** **"Not just any direction — the RIGHT direction."** Resolve *always*
gives direction, shows honestly what it's confident in versus what it inferred, and
**offers to sharpen it.**

The compass/map motif follows from this and should drive the brand.

---

## 1. How we got here (the reasoning — do not skip)

The floor gate had two gates. Gate 1: is there a real outcome? Gate 2: is there a lever —
ongoing work that keeps paying off, connecting the project to the outcome?

Gate 2 failing was going to **halt** the user and ask them a question. We spent a long
time trying to write that question. Every version failed, and the reason it failed is the
finding:

> **The founder came to us for direction. Asking them "so what work would actually move
> this?" hands our job straight back to them.**

Every phrasing of the Gate 2 question was some version of *"what should you be working
on?"* — which is precisely what they hired the product to answer. Dressing it up in
better words didn't fix it. The question was structurally wrong.

### The realisation that dissolved the problem

Gate 1's question works — *"If this works, what's different in three months that isn't
true today?"* — because it asks the founder to **DESCRIBE** something, not **ANALYSE**
something. It's an act of imagination, not product management. Anyone can answer it.

So: if we know the **outcome** (Gate 1) and we know the **current state** (from intake),
then **deriving the mechanism that closes the gap between them IS THE PRODUCT.** That is
literally what a roadmap *is*.

Therefore: **Gate 2 failing is OUR failure, not the founder's.** Either
- (a) there genuinely is no path — a real finding, worth surfacing; or
- (b) **our intake questions didn't elicit enough** — which asking them a harder question
  at the gate will not fix.

Either way, **interrogating the founder is the wrong response.**

### What replaces it

Deliver the value FIRST, honestly labelled. Then offer to improve it.

The founder answers questions **because they can see what the answer would buy them** —
which is a completely different motivation from being blocked at a gate. It also means we
**earn** the right to ask harder questions, rather than spending their patience before
they've seen anything.

---

## 2. The design

### Gate 2 becomes a PROVISIONALITY SIGNAL, not a halt

- Gate 2 fails → **generate the roadmap anyway.** Mark the affected themes
  `provisional = true`. Show what we inferred and why. Log to `refusals`
  (`kind='referee_reject'` / a Gate-2 kind) as a **diagnostic**.
- **Gate 2 firing often means OUR INTAKE QUESTIONS ARE WRONG.** The `refusals` table is a
  diagnostic on elicitation quality, never a judgement on the founder. **Fix upstream.**
- **Gate 1 remains a genuine halt** (no outcome = nothing to aim at). But Intake now
  captures the outcome, so it should fire rarely.

### Entry point 1 — at the reveal, before the app opens

The roadmap is shown. So is our confidence: *here's what we're sure of; here's what we
inferred.* Then a fork:
- **"Sharpen this"** → the opt-in question round.
- **"I trust this"** → accept as-is and go straight in.

### Entry point 2 — persistent, in-product

- **Roadmap tab** — its technical home. Provisional themes are marked here, with the
  sharpen entry point beside them.
- **Dashboard** — a prompt surfaces **only when provisional themes exist**. It disappears
  when there are none. It **never nags again about a theme the founder has accepted.**
  - ⚠️ **The rot risk:** if this prompt is always there, it becomes wallpaper. Conditional
    display is not a nicety — it is what keeps the signal meaningful.

### The sharpen round — the real design work of this epic

An **opt-in** round of deeper questions targeting exactly what's provisional.

- **The questions can be HARDER and more creative than intake questions.** This is the
  key unlock: the founder *chose* to be here. The elicitation budget is **given**, not
  taken. That is a bigger budget than any signup flow could ever have — no signup flow can
  ask a founder to think hard, because they haven't been given anything yet.
- **Soft guidance, tied to a concrete outcome.** Never a vague nag:
  > *"These two themes are our best inference. Ten minutes of questions and they'd be
  > built on what you actually know."*
  The founder can see exactly what they're buying with their attention.

### "I trust this" means ACCEPTED — full stop

- The provisional flag is **REMOVED**, not downgraded to "marked provisional".
- The theme runs like any other theme.
- **Rationale:** it is the founder's project and their adult decision. Keeping it flagged
  after they've accepted it would override them and treat our inference as more
  authoritative than their own judgement of their own project. We are not their parent.
- `inferred_from` **stays in the data** as a fact of provenance. The **UI** stops
  second-guessing them. Provenance is a fact; acceptance is a decision. Different things.

### Regeneration is TARGETED

- Sharpening regenerates **only the provisional themes.** Confident themes are untouched —
  the founder may already be attached to them.
- **Exception:** if **all** themes are provisional, regenerate the whole roadmap.
- If that exception fires often, **intake is broken.** Treat it as an alarm, not a feature.

---

## 3. Why this is BETA, not Alpha

**Alpha ships (small, and mostly already built):**
- Gate 2 sets `provisional = true` instead of halting. **This is a SMALLER change than the
  halt would have been** — one flag, no screen. The `initiatives.provisional` and
  `inferred_from` columns already exist.
- Provisional themes are **visibly marked** on the roadmap. This is the honesty promise;
  without it we'd be shipping silent inference, which is the thing the product exists to
  prevent.
- The **Gate 1 halt screen** — one question, no fork, no escape hatch.

**Beta ships (this epic):**
- The sharpen question round.
- The reveal fork (sharpen vs trust).
- The dashboard prompt.
- "I trust this" acceptance mechanics.

**The sequencing argument (this is the important bit):** we **cannot design good sharpen
questions today**, because we have no real data on where Gate 2 actually fails. Alpha
*generates that data* — every provisional theme and every Gate 2 miss lands in `refusals`.
Beta then writes the sharpen questions against **real failure patterns from real
founders**, instead of guessing now.

**Deferring is not a compromise here. It is the correct order.**

---

## 4. Consequences to propagate (things that are now WRONG elsewhere)

- **"Structure and refusal" is superseded** as the product's framing. That phrase is
  written into multiple docs (positioning, the master plan, role files). It needs
  replacing with the direction/confidence framing.
- **`FLOOR-GATE-DESIGN-FINDING.md`'s monetisation gate is partly resolved by this.** That
  doc says "don't sell refusal as the differentiator until the criterion is fixed." Under
  the new framing we're not selling refusal at all — we're selling *honest direction*. The
  criterion still needs fixing (Gate 2's "no natural endpoint" axis is wrong), but a
  wrong Gate 2 is now much less harmful: it produces an unnecessary provisional flag
  rather than wrongly refusing a legitimate founder.
- The floor's Gate 2 criterion redesign (see `FLOOR-GATE-DESIGN-FINDING.md`) is **still
  needed** — but it is **no longer a monetisation blocker.** It is a quality issue.

## 5. Open questions for whoever picks this up

- **What exactly are the sharpen questions?** This is the epic's core work. Do not write
  them from imagination — write them from the `refusals` data Alpha produces.
- **How is confidence displayed at the reveal** without undermining the founder's trust in
  the roadmap they've just been given? The line between "honest" and "unconvincing" is the
  whole design problem of the reveal screen.
- **Does the dashboard prompt have a cap?** How many times does it surface before it
  stops asking?
