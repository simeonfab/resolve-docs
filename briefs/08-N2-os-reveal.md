---
status: current
---

> ⚠️ **SUPERSEDED by [`briefs/ALPHA-BUILD-TICKETS.md`](./ALPHA-BUILD-TICKETS.md) where they differ** — that pack (THE AUTHORITY for the Alpha build) carries the binding element / action / motion contract per screen. Use this brief for background only.


# Ticket N2 — OS Assembly Reveal → Dashboard

**Repo:** `cap-pm-cockpit-alaria`
**Type:** New build.
**Reference file (pixel-exact):** `ResolvePM OS Reveal.dc.html`
**Depends on:** Ticket N1 (tab bar must exist to slide in) and Ticket N3 (Dashboard cards must exist to populate).

## Two distinct behaviours — real state requirement, not just animation
This screen has two entirely different treatments depending on whether the user has ever seen it before. **This requires a real persisted flag**, not just a client-side check — e.g. a boolean/timestamp on `projects` or the user's profile record tracking whether the grand reveal has played for this user. **This data-layer piece is not yet designed — flag back if the right place to store this isn't obvious rather than picking one silently.**

### First run (flag not yet set)
Plays once, then sets the flag. Sequence (see file's exact keyframe timing):
1. Dark full-screen veil: brand mark, "Welcome in, [name]." (serif), "Your operating system is ready."
2. Veil lifts (fades out).
3. Header drops in from the top.
4. Bottom tab bar slides up from below.
5. The four Dashboard cards (Ticket N3) populate **in leverage order** — win card first, then blocked, then schedule, then pointer — each fading/sliding in with a staggered delay.
6. Settles on the Dashboard, fully interactive.

### Every later open (flag already set)
No dark veil beat at all. Header and tab bar are simply present immediately (shell doesn't animate in). The four cards do a quick, calm fade-in — much faster and subtler than the first-run version, not the full staggered reveal.

## Acceptance criteria
- A real persisted "has seen OS reveal" flag exists and is checked correctly — first run only plays once per user, ever.
- First-run sequence matches the specified order and timing exactly.
- Recurring-open version is visibly calmer/faster, not a repeat of the grand version.
- Both paths land on a fully functional Dashboard (Ticket N3) at the end.
- Flag back if the "first run" flag's storage location isn't already obvious from the existing schema.
