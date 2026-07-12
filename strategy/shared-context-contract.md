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

## HANDOFF (before switching tools or ending a long session)

objective · completed · current state · decisions made · open decisions · blockers · risks · next action · sources to load · what not to repeat
