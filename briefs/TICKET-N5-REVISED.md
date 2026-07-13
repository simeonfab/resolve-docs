# Ticket N5 — Team Tab + Agent Chat

**Repo:** `cap-pm-cockpit-alaria`
**Route:** frontend + AI orchestration → **Claude Code** (frontend is Claude's lane).
**This is the heaviest ticket in the set.** Give it its own session. Do not bundle it.
**Mockups (pixel-exact, in `resolve-docs/mockups/`):** `ResolvePM_Team_Tab_dc.html`
(roster) and `ResolvePM_Agent_Chat_dc.html` (individual chat + stand-up room).
**Depends on:** N1 (nav shell). Parallel-safe with N2/N8 subject to a file-touch audit.
**Original ticket:** `11-N5-team-tab-agent-chat.md` — read it, it is still the spec for the
UI. This document adds the **architecture decisions** it was blocked on.

---

## The four decisions (APPROVED — do not re-derive, do not redesign)

### 1. Where the role prompts live
**Bundle committed copies of the three role `.md` files into this repo.** Canonical home
stays the ResolveOS repo; cap-pm holds **snapshots**.

- This is the same pattern Intake already uses (`resolveos-context/` — committed copies,
  bundled at build, nothing fetched at runtime). It works, it survives the ResolveOS
  branch mess, and it needs no cross-repo runtime fetch.
- Put them somewhere obvious, e.g. `src/lib/ai/roles/`. Each file gets a header noting:
  *snapshot; canonical home is the ResolveOS repo; if they disagree, ResolveOS wins.*
- ⚠️ **The role files are currently on an UNMERGED ResolveOS branch**
  (`claude/resolveos-docs-filing-2` is the coherent one — the other filing branch has a
  mixed tree with all ten roles). Take the three roles from there. If you cannot reach
  them, STOP and say so — **do not write role definitions from memory.**

### 2. How a role is "executed"
**The role `.md` file becomes the system prompt.** That is the whole mechanism. No agent
framework, no orchestration runtime, no tool loop.

Each chat call = `system: [role file] + [project context]` · `messages: [conversation
history] + [user message]`.

### 3. Stand-up routing — the hard part
The ticket says: **not all three every time, not random, real judgement.** The
architecture is already proven in this codebase — it is the same shape as the floor and
the referee:

- **An ISOLATED ROUTER CALL.** Input: the user's message + the three roles' scopes
  (from their role files). Output, strict `json_schema`:
  `{ responders: ["product_director" | "growth_lead" | "tech_lead", ...], reason: string }`
- Then **each selected role answers in its OWN call**, with its own system prompt.
- 🚨 **NEVER bundle routing into a role's response call.** A call asked to both decide
  whether it should speak and then speak will always speak. This is the exact failure
  that broke the floor gate (a judgement bundled with generation fires 0/50). One
  decision per call. This is not negotiable.
- Router returns **one or more** roles. It must be able to return one. If it always
  returns all three, it is broken — report that, do not paper over it.

### 4. What context each role gets
**The same data for all three.** The *role prompt* is what differentiates them, not the
data.

Context = the project session (`outcome`, `project_understanding`,
`recommended_action`), the confirmed roadmap themes (`initiatives`), and current work
items. Pull it from the real DB — never a fixture.

---

## Build

### Team tab (roster)
Per the original ticket and `ResolvePM_Team_Tab_dc.html`:
- **Exactly three roles: Product Director, Growth Lead, Tech Lead.** No fourth. (The
  Implementation Engineer is deliberately excluded — nobody chats with the implementation
  engineer; that is Codex's job.)
- Each: avatar, name, short role tag, and a **project-contextualised intro sentence** —
  generated from real project context, not hardcoded. The supporting line must adapt to
  the user's real project type ("rebrand" in the mockup is an example, not a string).
- **First-visit staggered intro animation; static on every visit after.** Persist that
  it has been seen.
- **Stand-up room row** below: stacked P/G/T avatars, "Talk to all three at once."

### Individual chat
- Header: back arrow, avatar, role name + tag.
- **On first open:** a context banner + an **opening greeting generated from real project
  context** — not hardcoded — plus 2–3 tappable starter-prompt chips relevant to *that
  role* and *that project*.
- A real, functioning conversation with that role's system prompt. **Not a scripted demo.**

### Stand-up room
- Same chat shell; header shows stacked P/G/T avatars.
- Router call → selected role(s) answer, each in its own call.
- **Each reply visually labelled with which role is speaking** (small avatar + name above
  the bubble).

---

## Proof — evidence, not claims (CLAUDE.md rule 11)

- **Click through everything.** Tap each role → does it open that role's chat? Tap
  stand-up → does it open the room? Back arrow → does it return? Starter chips → do they
  send? Report where each actually went.
- **Screenshot** the roster, an individual chat, and the stand-up room with a multi-role
  reply.
- **Prove the router discriminates:** send three messages — one obviously for the Growth
  Lead (e.g. positioning/customers), one obviously for the Tech Lead (e.g. build/stack),
  one genuinely cross-cutting. Report **who responded to each** and the router's reason.
  **If all three answer every message, the router is broken — say so.**
- Verify against the **canonical test project**
  (`c0a10000-0000-4000-8000-000000000001`) with real data. No fixtures.
- Do not claim an animation "looks good" — report only that it fires and what it does.
  Motion and feel are Simeon's judgement.

## Flag, don't fake
If the selective-response routing genuinely cannot be made to work well as an isolated
call, **say so and propose an approach** (e.g. a simpler keyword-scoped first pass).
**Do not silently ship a fake** that always answers with all three or picks at random.
The original ticket says this explicitly and it is the right instinct.
