---
status: current
note: "Defined 8 Jul 2026; self-declares superseding the 4 Jul phase definitions."
---

# Resolve — Build Phases & Commercial Model

*Defined 8 July 2026. Supersedes the 4 July phase definitions, which were directionally right but mis-filed the cost problem and the commercial layer.*

---

## 0. The shape

**Three build phases, plus one commercial layer that sits on top of Alpha — not after it.**

Each phase is defined by the *question it answers*, not by a feature list. Features follow from the question.

| | Question | Who's using it | Is Simeon present? |
|---|---|---|---|
| **Alpha** | Does the journey deliver value? | ~5 invited testers | Yes |
| **Tier B** *(commercial layer)* | Will someone pay for this? | Paying clients | Yes — that's the product |
| **Beta** | Can a stranger self-serve without me? | 20–100 strangers | **No** |
| **Paid Launch** | Does revenue exceed cost? | Paying users | No |

**The organising insight:** almost every hard requirement in Beta exists to *replace Simeon's presence*. Real domain, hardened auth, reliability-grade error handling, cost caps, allowance enforcement — all of it substitutes for a human sitting next to the user. In a guided run he hasn't left, so none of it is needed. **That's why Tier B sits on Alpha, not Beta.**

---

## 1. Alpha — "Does the journey deliver value?"

**Goal:** a tester goes Intake → sign-in → roadmap → cockpit, unimpeded, and says the output was worth having.

- Invited testers (~5), Simeon present
- Vercel preview/production URLs are fine — no real domain needed
- Simeon's own API key
- **Cost profile: bounded and one-off.** One intake per user (~4 calls), one roadmap generation, occasional nano calls. No chat, no deep orchestrator. Five testers is a trivially small bill.
- Data persists in Supabase; fixtures and resets acceptable

**Remaining work:** Steps N9, N4, N6, N2, N7, N8, N5-lite + Step 1.5 (roadmap-generation quality) + switch on the `ai_usage_events` ledger.

**Exit criteria:** the full journey runs without hitting old scaffolding, dead ends, or garbled roadmap content — and a tester finds the result genuinely useful.

---

## 2. Tier B guided runs — a commercial layer, not a build phase

**Sits directly on Alpha.** Nothing else is required.

- £30–40 per guided session (existing Tier B definition; target £180/month ≈ 5 sessions)
- Simeon runs the session with the client; Resolve is the scaffold and the takeaway artifact
- **Revenue-positive from the first session.** Even at an implausibly high £5/intake, each session nets £25–35.
- **Tier B revenue funds Beta's free users.** Beta becomes affordable because Alpha earned the money.
- **Measurement comes free.** With the ledger switched on before the first session, each guided run measures its own cost. No separate spike required.

**Important caveat — these are two different products.** Tier B sells *Simeon's judgement, scaffolded by Resolve*. Beta sells *Resolve without Simeon*. The first is plausibly sellable today. The second must still clear the "better than pasting into Claude" bar (see §6). Tier B funds that work; it does not discharge it.

---

## 3. Beta — "Can a stranger self-serve without me?"

**Goal:** strangers complete the journey with zero intervention, and the API bill is bounded and known.

- **Free — but allowance-bounded, not unlimited.** Free-with-a-cap costs a *one-off* charge per user. Free-with-chat is unbounded and unaffordable. That distinction is the entire cost model.
- 20–100 strangers. Cap set from measured cost, not guessed.
- **No chat, no deep orchestrator.** Those are Paid Launch. Team tab = roster only (N5-lite already reflects this — nothing needs redesigning).
- What the allowance actually bounds: repeat intake runs and roadmap regenerations. Per-user Beta cost ≈ one intake + one roadmap generation + a few nano calls.

**Work required (all of it exists to replace Simeon's presence):**
- Real home / domain — not preview URLs
- Real accounts, durable data that survives deploys, recoverable
- Reliability-grade error states; the known Vercel env-var issue actually fixed
- **Enable the allowance ledger** (`ai_usage_events` + `assertAiUsageAllowance()` — already written, never fired)
- Set and enforce a per-user allowance; cap total user count

**Open sequencing question, worth deciding later:** the "5 people I know" could be either (a) guided runs on Alpha, or (b) the first *unguided* self-serve cohort — a friendly smoke test before opening to strangers. These test different things (the artifact vs. self-serve), and both are worth doing. Not decided.

**Exit criteria:** N strangers complete the journey unaided; the bill is bounded and measured.

---

## 4. Paid Launch — "Does revenue exceed cost?"

**Goal:** unit economics positive.

- **Chat + the deep orchestrator switch on here, as the paid tier.** This is what people pay for.
- Billing, plan definition, per-plan usage caps, subscription management
- Payment mechanism: undecided
- Name: **"Paid Launch."** Dropped "Monetizable Beta" — it implies Beta is free-and-unlimited, which is precisely the trap.

**Explicitly rejected:** offering chat free on a cheap model. A Product Director giving thin, cheap-model advice is *worse than one that isn't there yet* — you'd spend money to damage the thing people would otherwise pay for. "Coming with the paid tier" is the honest, stronger story.

---

## 5. The cost model

### What we know (verified)
- `ai_usage_events` exists with the right shape (`model`, `input_token_count`, `output_token_count`, `total_token_count`, `event_type`, `status`). **0 rows. Never fired.**
- `assertAiUsageAllowance()` — a lifetime-allowance gate — **already exists in code, never exercised.** This *is* the Beta cost control. It's built.
- That table lives in the **ResolvePM** database only. **Intake is a separate static site and has never been metered at all** — and Intake is almost certainly the expensive part (4 calls, with the full ResolveOS role + governance markdown injected into every prompt).
- **Two cost profiles, completely different:**
  - *Intake:* bounded, one-off, predictable. Paid once per person, forever.
  - *Chat + orchestrator:* unbounded, recurring, scales with engagement, no ceiling.
- **Alpha only has the bounded profile.** That's why Alpha is affordable and Beta-with-chat isn't. The unaffordability was never caused by the absence of billing — it's caused by switching on unbounded surfaces.

### The dominant unknown
**What model does Intake use, and how many tokens of ResolveOS files are injected per call?** Four calls × a large fixed prefix is very likely the bulk of the bill. If it's `gpt-4o` with 30k tokens of context, it's expensive. If it's nano, it's pennies. **This is a ten-minute look at the Intake repo, and the whole problem may turn out to be one model string.**

### The levers, ordered by how cheap they are to pull
1. **Trim input tokens before touching model quality.** Do all four Intake calls need the *full* ResolveOS file set? Call 4 already reuses Call 1/2 output verbatim — it likely needs no role definitions at all. Free.
2. **Check prompt caching.** If the ResolveOS files are a fixed prefix across every call and user, OpenAI's caching should discount them heavily. Zero code change. Nearly free.
3. **Switch on the allowance ledger.** Already written. Set an allowance, enforce it. Cheap.
4. **Cap the user count** at whatever measured cost affords. 100 free users is a choice, not a constraint.
5. **Downgrade the Intake model. Last resort.** Intake output quality is *already* flagged as weak, and it's the thing Alpha exists to test. Cheapen it and you get a false negative on your own product.

### Rejected
- **Bring-your-own-key.** No easy path exists, and it's a hard ask for the audience. It would cost nothing and kill the "just try it" magic.

---

## 6. The honest problem — not solved by any of this

Standing note, unresolved: *"Is output quality genuinely better than pasting into Claude? Answer: still weak."* And Mani, repeatedly: *"what does this do differently from just pasting into ChatGPT?"*

No amount of phase re-labelling fixes that. **Tier B survives it** — the value of a guided session is a product person sitting with a founder, structuring their mess; the tool is the scaffold. **Beta does not survive it** — Beta sells Resolve without Simeon, and must clear the bar on its own.

Tier B revenue buys the time to fix it. It does not remove the requirement.

---

## 7. Deliberately TBD (do not invent these)
- Per-user cost — **measure it**, don't estimate it
- Beta allowance size
- Beta user cap
- Paid Launch pricing and plan structure
- Payment mechanism

*Every one of these is arithmetic once the unit cost is known, and guesswork until then. The ledger switched on before the first guided session yields the number for free.*
