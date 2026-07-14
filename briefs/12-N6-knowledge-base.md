---
status: current
note: "N6 hosts the refusals / cut record (master plan Phase 2.1)."
---

> ⚠️ **SUPERSEDED by [`briefs/ALPHA-BUILD-TICKETS.md`](./ALPHA-BUILD-TICKETS.md) where they differ** — that pack (THE AUTHORITY for the Alpha build) carries the binding element / action / motion contract per screen. Use this brief for background only.


# Ticket N6 — Knowledge Base Tab

**Repo:** `cap-pm-cockpit-alaria`
**Type:** New build, deliberately minimal (Alpha scope is a stub, not a full feature).
**Reference file (pixel-exact):** `ResolvePM Knowledge Base Tab.dc.html`
**Depends on:** Ticket N1 (nav shell). Parallel-safe with N3–N5, N7, N8.

## Layout
- Header, eyebrow "From your intake," headline "Knowledge base" (serif).
- A pill badge: "Read-only · captured at intake" (lock icon).
- **Section: Project understanding** — the same four cards from Intake's Confirm Understanding screen (📁 Project / 🎯 What you want / 📍 Where it is / ❓ Biggest uncertainty), shown here **read-only, no editing, no pencil icons.**
- **Section: Recommended action** — dark card, amber eyebrow "Decide this first," the actual recommended action text.
- **Section: Full report** — a row with a document icon, "Intake report" title, "Full project breakdown · PDF" subtitle, and a chevron.

## Real gap, flag explicitly, do not silently build a fake version
**Tapping the "Full report" row should view/download an actual PDF of the intake report. No backend exists yet for generating or serving this PDF.** Build the UI as specified, but the actual document generation/serving is a real, unbuilt piece of work — report this back rather than stubbing in a fake download or silently deciding not to implement it.

## Explicitly out of scope for Alpha
No search/ask function, no decision/ADR tracking. These are real, named Beta-or-later features — do not build placeholders that look functional for either of these.

## Acceptance criteria
- All four understanding cards render with real data, correctly non-editable.
- Recommended action and report sections match the reference file.
- PDF gap explicitly flagged in your completion report, not silently worked around.
