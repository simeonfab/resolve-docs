---
status: current
note: "Human decision checklist for changes to ResolveOS itself. Do not load into an active project's standard instructions."
---

# ResolveOS Self-Hosting Governance

**Purpose:** A reference for decisions about changing ResolveOS itself. This is a human decision checklist, NOT an instruction set for ResolveOS to follow autonomously. Do not load this into the active project's standard instructions — only consult it when evaluating a proposed change to the framework.

---

## The Core Problem

ResolveOS is increasingly used to improve, extend, and govern itself: product strategy, workflow design, governance updates, skill creation, routing changes, documentation structure. This creates risk of:

1. **Circular validation** — ResolveOS approving changes because they conform to ResolveOS.
2. **Governance accretion** — endless new rules/guardrails/skills causing bloat and contradiction.
3. **Local optimisation** — the framework overfitting to specific projects (ResolveYGO, ResolvePM) instead of staying generally useful.
4. **Authority drift** — ResolveOS becoming architect, PM, reviewer, and validator simultaneously.
5. **Hidden boundary violations** — scope/authority quietly expanding without explicit approval.

---

## The Five Rules

1. **Two-context rule.** A problem only becomes a framework-wide change if it has shown up in at least two unrelated projects. One occurrence goes into a backlog, not into a merge.

2. **No self-certification.** Proposal and approval must never happen in the same pass. Either a human reviews cold, or a separately-prompted pass argues against the change before a decision is made.

3. **Net-neutral complexity.** Every new rule, skill, or workflow must identify something it replaces or makes obsolete. Default bias against pure addition.

4. **Human-only merge authority.** Any change touching governance, scope definitions, or how ResolveOS validates its own output requires human sign-off — never autonomous merge.

5. **Quarterly pruning.** A scheduled, human-led review to cut unused rules/skills/workflows. Growth without removal is the default failure mode.

---

## Practical Test Before Adding Anything

Ask: *does this need to be true on every interaction, or only when modifying the framework itself?*

- If every interaction → it's an operating instruction (lives in the normal skill/instruction files).
- If only when changing the framework → it's a governance matter (lives here, consulted manually, not loaded by default).

Keep these two categories separate. Mixing them means ResolveOS re-reads its own constitution to fix a typo, which is exactly the overhead and circularity this model exists to prevent.
