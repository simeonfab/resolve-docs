# Ticket N8 — Global Bar / Orchestrator

**Repo:** `cap-pm-cockpit-alaria`
**Route:** frontend + AI orchestration → **Claude Code**, its own dedicated session.
**Mockup (pixel-exact):** `ResolvePM_Global_Bar_Contextual_dc.html` — this is the spec.
(`ResolvePM_Global_Bar_dc.html` is earlier placement exploration. **Background only. Do not
build it.**)
**Original ticket:** `14-N8-global-bar.md` — still the spec for the UI. This document adds
the **architecture it was blocked on**, plus the decisions already made.

---

## ⚠️ Read this first — this ticket has been faked once already

A previous session built the visual shell and shipped it: an expanding pill, contextual
copy from a hardcoded route→string map, tappable suggestions that prefilled the textbox,
and a submit that displayed *"Resolve's assistant connects in a later release."*

**No LLM call. No routing. No writes. Nothing.** A picture of a feature.

That is the failure mode this ticket exists to avoid. **The bar IS the orchestrator.** The
input surface is the trivial part; the engine is the whole point. A global bar that looks
right and does nothing is **worse than no global bar** — it looks finished and isn't.

**If you cannot build the engine, build nothing and say so.**

---

## The architecture (DECIDED — do not re-derive, do not redesign)

### 1. Intent routing is an ISOLATED CALL. Non-negotiable.

The bar supports five intents: **ask · find · navigate · capture · confirmed-create.**

- **A router call, on its own.** Input: the user's message + the current tab + the five
  intent definitions. Output, strict `json_schema`:
  `{ intent: "ask"|"find"|"navigate"|"capture"|"create", confidence: "high"|"low", reason: string }`
- **Then a SEPARATE call (or deterministic handler) executes that intent.**
- 🚨 **NEVER bundle routing into the execution call.** A call asked to both decide what to
  do *and then do it* will always do something. This is the exact failure that broke the
  floor gate (a judgement bundled with generation fired 0/50) and it is the same trap the
  N5 stand-up router faces. **One decision per call.** This codebase has the pattern
  already — follow it (`src/lib/ai/` — the floor, the referee, `update-drafts.ts`).
- Use `assertAiUsageAllowance()` and `recordGenerationAttempt()` on every model call.

### 2. Writes are CONFIRMATION-GATED. Always.

**The bar never writes silently.** `capture` and `confirmed-create` both produce a
**proposal the user confirms before anything is persisted.** Show what will be created,
where it will go, and let them cancel.

If the router returns `confidence: "low"`, do not act — **ask the user what they meant.**
Guessing and writing is the worst possible behaviour here.

### 3. What each intent actually does

| intent | behaviour |
|---|---|
| **navigate** | Deterministic. Route to the tab/item. **No model call needed to execute** — the router already decided. |
| **find** | Search existing work items / initiatives / knowledge base. Deterministic query. Return results in the panel. |
| **ask** | A question about the project → answer from real project context (session, outcome, roadmap themes, work items). **Read-only. Never writes.** |
| **capture** | A quick note. → **`knowledge_base_items`** (the table exists — verified). Confirmation-gated. See §4. |
| **confirmed-create** | Create a work item / task. → **`work_items`**. Confirmation-gated: show the proposed item (title, type, priority, which theme it traces to) and let the user confirm or edit before insert. |

### 4. "Captured inbox" — the destination that didn't exist

The old ticket said capture feeds N4's "Captured inbox". **A previous session correctly
flagged that no such thing exists in the codebase or schema.** Here is the decision:

**Capture writes to `knowledge_base_items`** (table exists, verified live). N4's Work tab
already renders a "Captured inbox" section with an empty state — **wire that section to
read from `knowledge_base_items`** filtered to captured notes. Add a `kind` or `source`
discriminator if one is needed to distinguish captured notes from the roadmap "cut" that
also lands there — **check the table's real columns first and flag if a discriminator is
missing rather than inventing one.**

### 5. Where the bar appears — DECIDED

- **Shown on:** Dashboard, Work, Roadmap.
- **NOT shown on: Team or Knowledge Base.** Team has its own agent chat (N5) — two
  competing input surfaces on one screen confuses the user. Knowledge Base has no agreed
  framing and one will not be invented.
- **Never during the chrome-free build sequence** (R2–R4). It appears only once the app
  shell has revealed.
- **One shared component**, contextual copy driven by the current route. **Not five bars.**
- Contextual placeholders and suggestions per tab: exactly as in `14-N8-global-bar.md`.

---

## Proof — evidence, not claims (CLAUDE.md rule 11)

**The router must be shown to discriminate.** Send five messages, one per intent, and
report what the router returned and what actually happened:

1. *"Take me to the roadmap"* → **navigate**
2. *"What's blocking the pricing work?"* → **find** or **ask**
3. *"Why is this sequenced this way?"* → **ask**
4. *"Remember that Sarah suggested a founder-led sales motion"* → **capture**
5. *"Add a task to write the pricing page copy"* → **confirmed-create**

**If the router returns the same intent for all five, it is broken. Say so.**

Then:
- **Show the confirmation gate.** Screenshot the proposal for a `capture` and a
  `confirmed-create` **before** it writes, and prove cancelling writes nothing.
- **Show the real rows** in `knowledge_base_items` and `work_items` afterwards. Query them.
- **Prove the captured note appears in N4's Captured inbox.**
- **Click through everything** — every suggestion chip, submit, cancel, and the navigate
  path. Report where each actually went.
- Verify against the canonical test project (`c0a10000-0000-4000-8000-000000000001`) with
  **real data. No fixtures.**
- Do not claim an animation "looks good" — report that it fires and what it does. Motion
  and feel are Simeon's judgement.

## Flag, don't fake

If any part of the orchestrator genuinely cannot be built well, **stop and propose an
approach.** A partial, honest bar (e.g. navigate + capture working, ask deferred) is
acceptable **if it is labelled and Simeon agrees**. A complete-looking bar with a stubbed
engine is not. That has already happened once.
