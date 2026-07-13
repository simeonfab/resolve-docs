---
status: current
note: "Project-agnostic. Canonical here for now; extract to a workflow repo when a second project adopts it."
---

# Shared Context Contract

## BOOTSTRAP (before substantial work)

1. Load the current-context entrypoint page named in this project's instructions.
2. Load only the deeper sources that entrypoint references as relevant.
3. Notion is authoritative for live state, planning, decisions, risks, evidence.
4. GitHub / current repo state is authoritative for code and technical evidence.
5. Never infer current code behaviour from planning docs when repo access exists.
6. Flag stale or contradictory sources; do not silently resolve them.
7. Prior chat memory is working material, never a source of truth.
8. Never ask the user to hand-attach a file that already exists in a repo. Reference it by path, or make it reachable. Hand-attaching is only for something not yet in any repo.

## CHECKPOINT (when work materially changes project state)

1. Update the canonical owning source during the session, not after.
2. Replace stale current-state info; never append another dated chronology.
3. Durable decisions go to the decision source with status, rationale, evidence.
4. Code claims carry repo, branch, commit, and validation evidence.
5. Never claim an update, commit, test, or deployment occurred without evidence — verify it, then say so.
6. Anything generated in a chat is not durable until committed to a repo.
7. Click through what you built. A screenshot of a rendered page proves the page renders. It proves NOTHING about whether it works. Before any screen or component is called done, you must actually USE it in the browser on the preview deploy:
   - Click every button, link, card, and tab you built. Report where each one actually took you. "It should open the Work tab" is not a verification — open it and say what happened.
   - Perform every interaction the ticket specifies (expand, collapse, submit, toggle, filter, dismiss). Screenshot the resulting state.
   - Follow every route through to its destination and confirm the destination is the right one and renders correctly.
   - Exercise the real states, not just the default: populated, empty, error, loading.
   - If something is unreachable, dead, or goes to the wrong place, that is a BUG — report it, do not quietly note it as out of scope.
   
   A build passing, TypeScript compiling, and a page rendering are three things that are all true of a screen whose buttons do nothing. Separately: motion, easing, timing and overall feel are the product owner's judgement, at the end. Do not claim an animation "looks good" — you cannot assess that. Report only that it fires and what it does.

## HANDOFF (before switching tools or ending a long session)

objective · completed · current state · decisions made · open decisions · blockers · risks · next action · sources to load · what not to repeat
