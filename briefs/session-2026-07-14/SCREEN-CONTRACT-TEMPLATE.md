# THE SCREEN CONTRACT

**Why this exists.** A mockup shows *appearance*. It says nothing about which elements are
interactive, what each one does when pressed, what data fills it, or what it shows when
there is no data. So an agent builds something that **looks** right, verifies its own work,
calls it done — and nobody ever clicks a button.

Every frontend failure this week is that shape: the floor built but unwired · the halt screen
built but unrendered · the team chat merged with no link to it · N9 built and never merged ·
the legacy `/roadmap` still live. **A "done" screen has never meant a working screen.**

**One artifact fixes three problems at once.** The Screen Contract is:
- the **spec** you hand an engineer,
- the **checklist** an auditor walks,
- the **acceptance criteria** that decide whether it shipped.

**The rule it enforces:** a screen is not done when it builds. A screen is done when **every
row in its contract is ticked, each with an artifact** (a screenshot of the actual
interaction, a query result, a SHA). No self-certification.

---

## THE TEMPLATE

Copy this per screen. Fill it before anyone writes code.

### Screen: `<name>` — route `<path>`
**Mockup:** `ResolvePM_<name>_dc.html` · **Brief:** `briefs/<nn>-<name>.md`
**Reachable from:** `<what the user clicks to get here>`
**Who can see it:** `<signed out / signed in / only pre-confirmation / etc>`

**A. LAYOUT**
> Matches the mockup: structure, hierarchy, spacing, type scale, colour.
> Deviations are listed explicitly or they are bugs.

**B. INTERACTIVE ELEMENTS** — *every* control. If it can be pressed, it is on this list.

| Element | Type | What it does | Where it goes / what changes | Disabled when | Verified |
|---|---|---|---|---|---|
| e.g. "Yes, add to my roadmap" | button | accepts the theme, advances to the next card | local state; persists on final confirm | never | ☐ |
| e.g. "Not quite" | button | opens the correction box | reveals textarea + re-check | never | ☐ |
| e.g. correction textarea → submit | input | POST `/api/roadmap-onboarding/check-theme` | referee verdict; warning if it reads like a task | empty text | ☐ |

**C. DATA** — every piece of real data on the screen.

| What is shown | Source (table.column / API) | Scoped by | If missing |
|---|---|---|---|
| e.g. theme title | `initiatives.name` | **`project_id`** (not `user_id`) | — |
| e.g. provisional marker | `initiatives.provisional` | `project_id` | hidden |

**D. STATES** — each one is a real screen, and each one gets built.

| State | What the user sees |
|---|---|
| Loading | |
| Empty (no data yet) | |
| Error | |
| Success / settled | |

**E. MOTION** — only where the mockup specifies it. Timing, easing, trigger.
Reduced-motion behaviour is stated, not assumed.

**F. NOT ON THIS SCREEN** — the deletion list. If it isn't in the spec it doesn't ship.
> e.g. no debug tools · no "Reset onboarding" · no quarter/RAG filters · no CRUD controls

---

## THE ACCEPTANCE RULE

A screen ships when, and only when:

1. Every row in **B** has been **clicked in a real browser**, against **real data**, and the
   observed behaviour matches the contract.
2. Every state in **D** has been **seen**, not assumed.
3. Every item in **F** is **absent**.
4. Each of the above carries an **artifact** — a screenshot, a query result, a SHA.
5. **The builder did not certify its own work.** A different agent, a different provider, or
   Simeon.

A passing build is not verification. "It renders" is not verification. **An unclicked button
is an unbuilt button.**
