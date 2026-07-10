---
status: current
---

# Ticket N1 — App Shell / Navigation (BUILD FIRST)

**Repo:** `cap-pm-cockpit-alaria`
**Type:** New build. **This must be built before N2–N8** — every other tab's mockup assumes this shell already exists.
**Reference file (pixel-exact):** `ResolvePM App Shell.dc.html` — note this file shows three exploration options (1a/1b/1c); **1a (bottom tab bar) is the chosen direction, confirmed.** Build only 1a; 1b (slide-over drawer) and 1c (scrolling pill tabs) are rejected alternatives shown for context only, do not build them.

## What to build
- **Fixed bottom tab bar**, five tabs: **Dashboard · Work · Roadmap · Team · Knowledge** (this exact order — note this differs from the order the tabs are described in elsewhere; this is the confirmed steady-state order).
- Active tab: amber icon. Inactive: muted grey-brown (`ink-ghost` token).
- **Header**, present on every tab: brand mark + greeting text (left), user avatar (right). Avatar tap opens the Settings menu (Ticket N7) — not a sixth tab.
- **Route gating, real logic requirement:** during onboarding (before the roadmap is confirmed), **only the Roadmap tab is reachable** — the other four tabs should be visible in the tab bar but in a locked/disabled state (do not hide them entirely, per earlier project decisions — they should be visible so the user knows what's coming). Once `roadmap_confirmed_at` is set (per Chunk 03's existing logic), all five tabs unlock and the nav order/behaviour becomes the steady-state described above.

## Explicitly not in scope
- Desktop layout — out of scope for Alpha. Mobile (390px) only. The brief notes the bottom-drawer pattern (1b) as the likely cleanest future path to a desktop sidebar — worth remembering for later, not building now.
- The Global Bar/Orchestrator — that's Ticket N8, though it docks visually above this tab bar once built.

## Acceptance criteria
- Bottom tab bar matches option 1a exactly.
- Route gating correctly locks four of five tabs until roadmap confirmation, using the existing `roadmap_confirmed_at` field.
- Header/avatar present and functional (opens Settings menu) on every unlocked tab.
- This ticket must be reported as complete and verified in production before N2–N8 are considered unblocked for their nav-dependent work (though their own screen-specific work can still be developed against a local/mocked shell in parallel if that's faster — flag if you take that approach).
