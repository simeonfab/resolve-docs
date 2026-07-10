---
status: current
note: "N5 rescoped to N5-lite: roster + intros only, no chat, no orchestration (master plan Phase 2.2). Chat portion is Beta scope."
---

# Ticket N5 — Team Tab + Agent Chat

**Repo:** `cap-pm-cockpit-alaria`
**Type:** New build. **This is the heaviest ticket in the new-build set** — real AI orchestration logic, not just UI. Consider giving it its own dedicated session rather than bundling with anything else.
**Reference files (pixel-exact):** `ResolvePM Team Tab.dc.html` (roster) and `ResolvePM Agent Chat.dc.html` (individual chat + stand-up room)
**Depends on:** Ticket N1 (nav shell). Can otherwise run parallel to N3, N4, N6, N7, subject to file-touch audit.

## Team tab (roster)
- Header, eyebrow "Your team," headline "Meet your team" (serif), supporting line: "Three AI leads, already briefed on your rebrand. Tap any one to talk — or gather them in the stand-up room." (adapt "rebrand" reference to the user's real project type, not hardcoded).
- **Exactly three roles: Product Director, Growth Lead, Tech Lead** — no others, no Compliance/HR, no fourth role.
- Each role: avatar (single letter, dark circle, amber text), name, short role tag (e.g. "Roadmap & sequencing" / "Customers & positioning" / "Build & migration"), and a project-contextualised intro sentence.
- **First-visit behaviour, real requirement:** each role's intro **staggers in** (one after another with a delay, per the file's animation timing) on the user's first visit to this tab. On every visit after that, it's static — no replay of the staggered animation.
- Below the three roles: a **Stand-up room** row — stacked overlapping avatars (P/G/T), title "Stand-up room," subtitle "Talk to all three at once." Tapping it opens the stand-up chat (see below).

## Individual agent chat
- Opened by tapping a role from the Team tab. Header: back arrow, avatar, role name + tag.
- **Real requirement:** on first open, shows a context banner (e.g. "Briefed on your [project] rebrand") and an opening greeting message **generated from real project context** (Call 1/2 data, confirmed roadmap) — not hardcoded. Below the greeting: 2–3 tappable starter-prompt chips relevant to that specific role and project.
- Standard chat input at the bottom: text field + send button.
- This must be a real, functioning conversation with that role's underlying prompt/persona (per the ResolveOS role definitions already established for Product Director, Growth Lead, Tech Lead this project) — not a static scripted demo.

## Stand-up room (group chat)
- Same chat shell, but header shows the stacked P/G/T avatars instead of one, title "Stand-up room."
- **Real orchestration requirement, the hardest part of this ticket:** when the user sends a message to the room, **only the relevant lead(s) respond — not all three every time.** Each reply is visually labelled with which role is speaking (small avatar + name above their message bubble). This requires real logic to determine relevance per role per message — do not build a version that always responds with all three, and do not build a version that picks one at random; this needs to be an actual judgement based on message content and each role's defined scope (per the ResolveOS role definitions). If this logic is genuinely ambiguous to implement well, flag it and propose an approach rather than shipping a fake/simplified version silently.

## Acceptance criteria
- Exactly three roles, correct names/scopes, no more no less.
- First-visit staggered intro animation, static on subsequent visits.
- Individual chats are real, project-contextualised conversations with the correct role persona.
- Stand-up room correctly selects relevant responder(s) per message rather than always-all or random, with clear per-reply role labelling.
- Report back honestly if the selective-response logic needs a simpler first-pass approach (e.g. keyword-based routing) rather than full judgement-based routing — this is better than silently faking full intelligence.
