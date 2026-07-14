---
status: current
---

> ⚠️ **SUPERSEDED by [`briefs/ALPHA-BUILD-TICKETS.md`](./ALPHA-BUILD-TICKETS.md) where they differ** — that pack (THE AUTHORITY for the Alpha build) carries the binding element / action / motion contract per screen. Use this brief for background only.


# Ticket N7 — Settings

**Repo:** `cap-pm-cockpit-alaria`
**Type:** New build, plus a real reconciliation task.
**Reference file (pixel-exact):** `ResolvePM Settings.dc.html`
**Depends on:** Ticket N1 (nav shell, avatar menu entry point).

## Entry point
- Tapping the header avatar (present on every tab, per Ticket N1) opens a small dropdown menu: **Profile · Settings · Sign out.** Settings is not a sixth tab.

## Settings screen
- Header: back arrow, "Settings" (serif).
- **Account** section: avatar, name, email, chevron (opens profile detail — if profile editing isn't built yet, this can be a stub, but flag it rather than silently deciding).
- **Workspace** section: current project name, current plan ("Alpha · early access").
- **Preferences** section: two toggles — "Daily briefing" (a morning nudge on the win-for-today) and "Waiting-on reminders" (ping when something's stalled too long). **These toggles need real backend wiring to actually gate the described behaviour — if that logic doesn't exist yet, build the toggle UI but flag that the underlying notification behaviour isn't implemented.**
- **Session** section:
  - **"Reset onboarding flow"** — description: "Clears this session and replays the intake from the start. Your saved roadmap stays intact." **Real reconciliation required: confirm this is the same feature as the testing-only reset button built earlier this project (during Chunk 03 follow-up work). If a separate, hidden testing reset was built with different behaviour, reconcile the two into one consistent implementation matching this spec — do not leave two different reset mechanisms in the app.**
  - Sign out, below it.
- Footer: version string ("Resolve v0.1.0 — Alpha" + tagline).

## Acceptance criteria
- Avatar menu and Settings screen match the reference file.
- Reset-onboarding-flow behaviour is reconciled with any earlier testing-reset implementation — confirmed as one consistent feature, not two.
- Preference toggles are honest about what's actually wired up vs. UI-only — report clearly which is which.
