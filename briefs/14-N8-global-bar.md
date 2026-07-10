---
status: current
note: "Build against ResolvePM_Global_Bar_Contextual_dc.html. ResolvePM_Global_Bar_dc.html is a rejected exploration (archived)."
---

# Ticket N8 — Global Bar / Orchestrator

**Repo:** `cap-pm-cockpit-alaria`
**Type:** New build.
**Reference file (pixel-exact) — the CHOSEN design:** `ResolvePM Global Bar Contextual.dc.html`. **Note:** a second file, `ResolvePM Global Bar.dc.html`, shows earlier placement exploration options — that file is background/reference only, not the spec to build. Build only what's in the Contextual file.
**Depends on:** Ticket N1 (docks visually above the tab bar). Benefits from N3–N6 existing for full contextual wiring, but the component itself can be scaffolded in parallel.

## What it is
A **floating pill**, docked just above the bottom tab bar, present on every tab **once the app shell has revealed** (i.e. not during the chrome-free build sequence, screens 8–10/R2–R4 — those stay free of this element).

- Collapsed state: dark pill, a sparkle/orchestrator icon (amber), placeholder text ("Ask Resolve, or capture a thought" — wording adapts per tab, see below), blinking cursor accent.
- Tapped/expanded state: a suggestions panel rises above the pill — a small card listing 2–3 tappable "Try" suggestions, contextual to the current tab, above the input itself.

## Contextual behaviour per tab — real requirement, this is the point of the feature
The prompt placeholder and suggestions must change based on which tab the user is currently on:
- **On Dashboard:** framed around orientation + capture. Placeholder: "Ask Resolve, or capture a thought." Suggestions: "What should I focus on right now?" / "Why is [blocked item] blocked?" / "Capture a quick note."
- **On Work:** framed around doing. Placeholder: "Add a task, or ask what's next." Suggestions: "Add a task to Now" / "What's waiting on someone else?" / "Clear my captured inbox."
- **On Roadmap:** framed around the plan, routes to Product Director. Placeholder: "Ask about your roadmap." Suggestions: "What's missing from this plan?" / "Move a theme into Now" / "Why is this sequenced this way?"
- Team/Knowledge Base: not shown in the reference file — **use the same contextual pattern (ask/capture framed around that tab's purpose); if the right framing isn't obvious, flag it rather than inventing wording that might conflict with those tabs' own dedicated chat surfaces (Team already has its own agent chat — clarify whether the global bar should even appear there, or defer to the tab's own input).**

## Function — scope, confirmed earlier this project
Supports: **ask, find, navigate, capture, confirmed-create.** It supersedes the old "Search workflows · Ctrl K" sidebar element seen in earlier builds — remove that if it still exists anywhere in the current codebase.

## Real technical note
This should be one shared component whose contextual copy/suggestions are driven by the current route/tab, not five separately built bars. Keep the underlying orchestrator logic (intent understanding, routing) separate from the visual shell, so future placements or additional tabs don't require rebuilding the whole thing.

## Acceptance criteria
- Appears on every unlocked tab once the shell has revealed (per N1/N2), never during the chrome-free build sequence.
- Contextual prompt/suggestions correctly change per tab as specified.
- Old "Search workflows · Ctrl K" element removed if present.
- Team/Knowledge Base framing question above is explicitly resolved with the team, not guessed.
