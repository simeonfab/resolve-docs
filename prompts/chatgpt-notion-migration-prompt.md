---
status: historical
---

# Resolve-Planning → Notion Migration

We are migrating the valuable content from the Resolve-Planning GitHub repo into the new shared Notion workspace. Notion is now the active source of truth for Resolve product planning (replacing GitHub for this purpose), so both Claude and ChatGPT can read and write to it.

The new Notion structure already exists at:
https://app.notion.com/p/390d8b53b7a88127aab7c1aec8e6fd22 (🚀 Resolve - top level)

Child sections already created:
- 🎯 Current (page ID: 390d8b53-b7a8-8127-bbc1-fabf4805d16c)
- 🏗️ Products (page ID: 390d8b53-b7a8-8104-b344-da3d1117f43c)
  - ⚙️ ResolveOS (page ID: 390d8b53-b7a8-817f-bf02-d4e354e754a6)
  - 🚪 Resolve Intake (page ID: 390d8b53-b7a8-81d4-9778-d5a07f9c82f1)
  - 📋 ResolvePM (page ID: 390d8b53-b7a8-8117-b906-c675342a423f)
- 📀 Strategy & Vision (page ID: 390d8b53-b7a8-81d8-bea0-f6026d55b577)
- 🧪 Validation & Evidence (page ID: 390d8b53-b7a8-810c-a296-d6d6fba3ae8b)
- 🗄️ Reference & Archive (page ID: 390d8b53-b7a8-816b-bb48-cf660ff3bd96)

---

## YOUR TASK

Migrate the 7 items below into the correct Notion sections. For each one, create a new child page in the correct parent section, with the full content from Resolve-Planning faithfully transferred (do not summarise — bring the full content). Where content is superseded by more recent decisions (noted below), add a clear note at the top of the page stating what's superseded and what the current position is.

After migrating all 7 items, update the Resolve-Planning README.md in GitHub to state that Notion is now the active source of truth and link to the new hub.

---

## ITEM 1 — Long-term roadmap
Source file: 01-strategy/long-term-roadmap.md
Migrate to: 📀 Strategy & Vision (parent page ID: 390d8b53-b7a8-81d8-bea0-f6026d55b577)
Page title: 📍 Long-term Roadmap
Status note to add at top: CURRENT — this roadmap is still accurate. Phase 1 (Resolve Intake validation) is in progress. The exit criteria (2-3 external testers) has been updated to 5 external testers in more recent planning.
Content: migrate in full.

## ITEM 2 — Route priorities
Source file: 01-strategy/route-priorities.md
Migrate to: 📀 Strategy & Vision (parent page ID: 390d8b53-b7a8-81d8-bea0-f6026d55b577)
Page title: 🗺️ Route Priorities
Status note to add at top: CURRENT — these route priorities remain valid as hypotheses. Existing Messy Project is still the active first route. Everything after Priority 1 is not yet in scope.
Content: migrate in full.

## ITEM 3 — Layer model
Source file: 03-layers/resolve-layer-model.md
Migrate to: 🏗️ Products (parent page ID: 390d8b53-b7a8-8104-b344-da3d1117f43c)
Page title: 🧱 Resolve Layer Model
Status note to add at top: CURRENT — this layer model is the correct architecture. Treat as the authoritative definition of what each layer is and how they relate. The Runtime Surface definition is a separate child page.
Content: migrate in full.

## ITEM 4 — ResolveOS Runtime Surface definition
Source file: 03-layers/resolveos-runtime-surface.md
Migrate to: ⚙️ ResolveOS (parent page ID: 390d8b53-b7a8-817f-bf02-d4e354e754a6)
Page title: ⚡ ResolveOS Runtime Surface
Status note to add at top: CURRENT STRATEGY — this is the most important underdocumented layer. It defines what happens after Intake — where the user keeps talking to ResolveOS. Implementation is still blocked (not approved to build yet), but this definition is correct and should guide all future product decisions about what comes after Intake.
Content: migrate in full.

## ITEM 5 — Risks and assumptions
Source file: 01-strategy/risks-and-assumptions.md
Migrate to: 📀 Strategy & Vision (parent page ID: 390d8b53-b7a8-81d8-bea0-f6026d55b577)
Page title: ⚠️ Risks & Assumptions
Status note to add at top: CURRENT — these risks remain valid. R3 (Notion/GitHub split-brain) is actively being resolved by this migration. R4 (ResolvePM dominates Resolve) remains an active risk to guard against. R7 (framework changes too early) is still relevant.
Content: migrate in full.

## ITEM 6 — Validation evidence and wife test feedback
Source files:
  - 04-validation/evidence-log.md
  - 04-validation/tester-feedback/2026-06-24-wife-test.md
Migrate to: 🧪 Validation & Evidence (parent page ID: 390d8b53-b7a8-810c-a296-d6d6fba3ae8b)
Page titles:
  - 📋 Evidence Log (parent)
  - 🧪 2026-06-24 Wife Test (child under Evidence Log)
Status note to add at top of Evidence Log: PARTIALLY CURRENT — the evidence log structure and validation model are still correct. The specific evidence entries are from June 2026 and have been partially superseded: the prototype is now a live app with a real OpenAI pipeline (not static). External tester count now includes Mani (completed, feedback received) plus others in progress via LinkedIn. Update this log as new tester sessions complete.
Status note to add at top of Wife Test: HISTORICAL EVIDENCE — this was the first usability test on the static prototype. Many of the issues flagged here have since been addressed in the redesigned live app. Keep as evidence audit trail.
Content: migrate both in full.

## ITEM 7 — Glossary
Source file: 07-references/glossary.md
Migrate to: 🗄️ Reference & Archive (parent page ID: 390d8b53-b7a8-816b-bb48-cf660ff3bd96)
Page title: 📖 Glossary
Status note to add at top: CURRENT — these definitions are accurate. Use this as the canonical reference for all Resolve product terminology.
Content: migrate in full.

---

## WHAT NOT TO MIGRATE (superseded)

Do NOT migrate these files — they are superseded and should remain only in GitHub as historical record:

- 06-operating-model/implementation-readiness.md — says "only static prototype polish approved." Superseded: a live API app now exists.
- 06-operating-model/do-not-build-yet.md — same reason.
- 06-operating-model/storage-rules.md — says Notion is deprecated. Superseded: Notion is now the active source of truth.
- 06-operating-model/source-of-truth-boundaries.md — same reason.
- 00-current/current-plan.md, current-todo.md, next-actions.md, current-decisions.md — superseded by the Active Priorities and Current Status pages already in Notion.
- 05-history/* — history files, useful in GitHub, not worth migrating.
- 02-resolve-intake/issue-16-reframe.md — the Issue 16 framing is historical. Resolve Intake is already documented in the Products section of Notion.

---

## AFTER MIGRATING ALL 7 ITEMS

Update the Resolve-Planning README.md in GitHub to say:

```
# Resolve-Planning

⚠️ NOTE: As of July 2026, Notion is now the active source of truth for Resolve product planning.

New hub: https://app.notion.com/p/390d8b53b7a88127aab7c1aec8e6fd22

This repository is retained as historical reference. The content in 01-strategy/, 03-layers/, 04-validation/, and 07-references/ has been migrated to Notion and updated. The 06-operating-model/ files are superseded by the live implementation that now exists.

For current priorities, status, decisions, roadmap, and validation evidence — use Notion, not this repo.
```

Push that README update to main and confirm the commit hash.

---

## IMPORTANT PRINCIPLES

- Do not summarise content when migrating — bring the full text.
- Do add the status note at the top of each page as specified.
- Do not create new pages beyond what's listed above.
- Do not modify the existing pages already in Notion (Current Control Panel, Products pages, etc.) — only add new child pages.
- If Notion write access fails for any reason, report exactly which pages failed rather than skipping silently.
- Confirm each page created with its Notion URL.
