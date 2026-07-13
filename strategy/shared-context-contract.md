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
7. Verification means an ARTIFACT, not a claim. "Verified", "matches the mockup", "confirmed", "tested" mean nothing without the thing that proves it: a screen matches its mockup → a screenshot of the rendered page next to the mockup; data is real → the actual query and rows; a migration applied → the schema queried back; a commit landed → the SHA; an interaction works → you performed it and describe what happened. A passing build and a clean typecheck prove the code compiles — nothing about whether the screen is right or the button goes anywhere. Click through what you built. Never substitute a confident summary, and never end by telling Simeon to go and check it himself — that IS the work.
8. Built ≠ wired ≠ shipped. A feature is not done until a user can REACH it. Before calling anything done, trace the path a real user takes to reach it and walk it: is it rendered by a route they can navigate to, is there a link/button/tab that gets them there, does the data it depends on flow in? An unreferenced component is not shipped — it is dead code. If a component has zero consumers, say so — that is a bug, not a detail.

## HANDOFF (before switching tools or ending a long session)

objective · completed · current state · decisions made · open decisions · blockers · risks · next action · sources to load · what not to repeat
