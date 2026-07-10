---
status: current
---

# Work-Mode Activity System — Two Prompts

## Prompt 1 — USE NOW (give this to the orchestrator today, to start using immediately)

> From now on, when I tell you what mode I'm in — PC, phone, voice-only/driving, or minimal/glance — only suggest activities that actually fit that mode. PC: file edits, implementation, design, write-ups. Phone: outreach, replying to testers, short posts. Voice-only: strategy talk, rehearsing pitches, dictating drafts, reviewing priorities. Minimal: nothing, that's fine. Don't resurface anything I've already marked done. If something urgent is blocking progress, prioritise it — but only if it fits my current mode. If nothing fits, just tell me that's a genuine gap rather than forcing something low-value on me.

This is informal, immediate use — not a ResolveOS framework change. Just behaviour for the orchestrator chat going forward.

---

## Prompt 2 — IDEA BANK (store for later; this is the "build it properly into ResolveOS, maybe" version)



Status: idea bank — not active scope, for future adoption into Resolve if it proves useful
Origin: recognised need for a way to keep making progress on Resolve regardless of location/device, without forcing low-fit work into the wrong context

## The problem this solves

Progress on Resolve currently assumes PC access. In reality, the day is split across several different "modes" — PC, phone, hands-free/voice, and minimal/glance — and a lot of potential progress time (driving, walking, queuing) goes unused because the available activities don't match the current mode.

## The four modes

1. **PC** — full keyboard/screen access. For: file edits, repo/code work, UI/screen design, governance/role file edits, detailed write-ups, spreadsheet/financial work, landing page building, Notion restructuring.
2. **Phone (hands-free not required)** — walking, queuing, between tasks. For: LinkedIn outreach (connection requests, messages), replying to tester responses, screener replies, sending tester links, short posts (Indie Hackers, LinkedIn groups), quick research lookups, scheduling.
3. **Voice-only (driving, or otherwise hands/eyes occupied)** — talking, not typing. For: strategic discussion/brainstorming, rehearsing how to explain Resolve out loud, dictating draft copy to revisit later, talking through tester feedback received so far, prioritisation/roadmap discussion, problem diagnosis conversations.
4. **Glance/minimal (church, family time, genuinely occupied)** — deliberately protected. Default is nothing, not busywork. Only single-tap, near-zero-effort actions belong here if anything does (e.g. a quick reaction/upvote). This mode existing and being allowed to mean "no Resolve work" is itself part of the system, not a gap in it.

## The activity bank (initial list — expand as real ones come up)

| Activity | Mode | Notes |
|---|---|---|
| Tester outreach (LinkedIn/Indie Hackers/DMs) | Phone | Needs typing, light screen use |
| Reply to tester responses / screener | Phone | |
| Send tester link + brief | Phone | |
| Post in LinkedIn group / Indie Hackers | Phone | |
| Review/process tester feedback against criteria | PC | Needs the actual criteria doc open |
| Edit prototype screens (copy/layout) | PC | |
| Write/edit role or governance files | PC | |
| Implementation (Codex/Claude Code) | PC | |
| Landing page / doc creation | PC | |
| Define the £180/month offer (pricing, format, who) | Voice | Pure decision-making, no artifact needed yet |
| Rehearse explaining Resolve in one breath | Voice | Useful prep for future tester/contact conversations |
| Talk through feedback patterns across testers | Voice | Synthesis, not data entry |
| Roadmap/priority review — what's next and why | Voice | |
| Dictate a draft (message, brief, pitch) to clean up later | Voice | Needs a PC pass afterward to finalise |
| Mentally plan next outreach batch (who, what order) | Voice | Executable later on phone |
| Quick research question (competitor check, terminology) | Phone or Voice | Voice if asking an AI verbally; phone if reading results |

## Logic for how this should behave once adopted

1. **State your mode, get matched activities.** When you say "I'm driving" / "I'm walking, no PC" / "I'm on my phone," the assistant filters the activity bank to that mode only — it should not suggest PC-only work for voice-only time.
2. **Don't resurface completed activities.** Once an activity is marked done (e.g. "outreach batch sent," "tester 3 feedback processed"), it drops out of suggestions. This needs the assistant to actually track what's been marked complete, not re-offer it from a static list.
3. **Roadmap priority overrides default suggestions, but only within the current mode.** If something is actively blocking progress (e.g. a confirmed tester is waiting on a reply), that should surface first — but still filtered to what's actually doable in the current mode. Don't suggest a PC-only blocker as the top item while driving.
4. **If nothing in the bank fits the current mode, say so honestly.** Don't force a low-value activity into a slot just to seem productive. "Nothing fits voice-only right now, this is fine as downtime" is a valid, correct answer.
5. **New activities get added to the bank as they come up**, tagged with a mode, rather than this list staying fixed.

## Suggested short prompt to give the orchestrator (for when this is adopted)

> When I tell you what mode I'm in (PC / phone / voice-only / minimal), check the work-mode activity bank and suggest only activities tagged for that mode. Skip anything already marked complete. If something on the roadmap is actively blocking progress, prioritise it — but only if it fits my current mode; otherwise hold it for when I'm in the right mode. If nothing fits, tell me that directly instead of forcing something low-value.

## Why this is parked, not active

This is a system-level improvement to how Resolve work gets scheduled — useful, but it's infrastructure for managing work, not work that moves the £180/month goal or tester validation forward directly. Revisit once Intake validation and the first paid engagement are further along.
