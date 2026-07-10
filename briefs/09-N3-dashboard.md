---
status: current
---

# Ticket N3 — Dashboard

**Repo:** `cap-pm-cockpit-alaria`
**Type:** New build (existing live Dashboard is old pre-existing scaffolding — "Command centre" stats cards — and must be fully replaced, not built alongside).
**Reference file (pixel-exact):** `ResolvePM Dashboard.dc.html`
**Depends on:** Ticket N1 (nav shell). Parallel-safe with N4–N8 after that, subject to file-touch audit.

## Real requirement before writing any code
**Strip the existing old Dashboard scaffolding entirely.** Do not add these new cards alongside the current "Active initiatives / Overdue work / Blocked work / Active risks / Upcoming milestones / Priorities this week" stat-card layout — that is a different, older design and must be removed, not layered under/behind the new one.

## Layout — four cards, fixed order, no exceptions
1. **Win for today** — dark (`Ink`) full-width card. Amber eyebrow (e.g. "Decide this first"). Serif headline stating the single highest-leverage action. Supporting paragraph in lighter grey. A small route-label pill at the bottom (e.g. "→ Opens work item") indicating what tapping the card will do. **Entire card is the tap target — no separate button.**
2. **Blocked/waiting-on** — white card, bordered. Eyebrow "Waiting on." Title + a small chip showing how long it's been stalled (e.g. "3 days"). Supporting text.
3. **Schedule** — white card. Eyebrow "Today." A list of time-stamped rows (meeting/to-do title + a small tag like "Meeting · 30 min" or "To-do").
4. **Pointer** — white card, icon + text row. Eyebrow "Worth a look." Title + supporting text nudging toward another tab (e.g. Roadmap).

## Interaction — real requirement, not just visual
**Tapping any card expands it in place.** The rest of the dashboard dims behind it (a semi-transparent dark scrim), and the tapped card grows into an expanded state showing more detail (see the reference file's "expanded" state for the Blocked card specifically — it shows a divider, metadata rows for "Stalled since" / "Waiting on," a note, and two action buttons: "Send reminder" and "View in Work"). **No separate open/close buttons anywhere** — the tap itself is the only affordance; tapping the scrim or the card again should collapse it back.

## Routing on the win card
Tapping the win card routes **contextually based on the action's type**: a work-item-type action opens that item (Work tab), an agent-relevant action opens that role's chat (Team tab), etc. This requires the underlying recommendation object to carry a `type` field determining the route — check whether this already exists on `work_items`/`initiatives` from earlier chunks; if not, flag it as a real schema gap rather than inventing a default routing behaviour.

## Persistence — critical, do not get this wrong
**The win-for-today content must NOT be recomputed on every page load.** It is a stored, stable value that only changes when its underlying task is completed or explicitly superseded. Never show a loading/refresh state for this card on a normal page visit — if the data layer doesn't already support this (per earlier chunks' persistence rules), flag it, don't build a "recompute on load" version as a stopgap.

## Acceptance criteria
- Old Dashboard scaffolding fully removed, not left running underneath.
- Four cards, fixed order, matching the reference file exactly.
- Click-to-expand-in-place works with scrim dimming, no separate open/close controls.
- Win card routes contextually by action type.
- Win card content is confirmed persistent/stable across page reloads within the same day (does not silently recompute).
