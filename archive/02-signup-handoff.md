---
status: superseded
superseded_by: "briefs/00-MASTER-PLAN.md + /RESOLVE-ALPHA-MASTER-PLAN.md"
note: "Old chunk-plan generation, replaced by the I/R/N brief set."
---

# Chunk 02 — Sign-up + Intake Handoff

**Depends on:** `01-database-foundation` (needs `project_sessions` table to exist)
**Blocks:** `03-roadmap-onboarding-gate`

## What this chunk does
Connects Intake's final-screen recommendation card to ResolvePM account creation, and passes the user's Intake session data into their new account.

## Trigger
On Intake's final screen, the primary recommendation card currently has a "coming soon" placeholder. This chunk replaces that placeholder with a real, working button/link.

## Sign-up mechanism — decided, do not use anything else
**Supabase magic link (email only, no password).**
- Chosen specifically over password auth (extra field, something to forget) and OAuth (requires provider setup not yet built).
- Reasoning locked: matches the "frictionless sign-up" principle and the "prefer proven modular tools over custom builds" principle — Supabase's magic-link auth is already a built-in, proven feature, use it as-is rather than building custom auth flow.
- OAuth is explicitly flagged as future scope (Beta or later), not Alpha. Do not build OAuth now.

## Flow
1. User clicks the recommendation card / high-leverage-action button on Intake's final screen.
2. User is prompted for their email only (magic link flow).
3. On successful auth, a new ResolvePM account is created (or an existing one is logged into, if the email matches — check Supabase auth docs for standard magic-link existing-user handling).
4. The user's Intake session data is written into `project_sessions`, linked to `user_id`:
   - Full project understanding (Call 1 output)
   - Recommended action + top priorities + what not to do (Call 2 output)
   - Confirmed tools
   - Full project plan report (Call 4 output)
   - Gap answers
   - Output templates the user interacted with
5. User is redirected into ResolvePM, landing directly on the **Roadmap onboarding gate** (see `03-roadmap-onboarding-gate.md`) — NOT the Dashboard. Dashboard is inaccessible until the Roadmap gate is completed (see that file for why).

## What this chunk explicitly does NOT include (Alpha scope)
- No animation or transition polish — a functional redirect is sufficient for Alpha. Polish is a later phase.
- No OAuth.
- No "cold start" flow (user arriving at ResolvePM without any Intake session data) — this is explicitly deferred, flagged as needing its own decision later, and is not expected to be a real case in Alpha since Intake handoff is currently the only documented entry point into ResolvePM.

## Acceptance criteria
- Clicking the Intake button creates a working Supabase magic-link sign-up flow
- On completion, a `project_sessions` row exists with all six data fields populated from the actual Intake session (not placeholder/dummy data)
- User lands on the Roadmap onboarding gate immediately after auth completes, not on Dashboard or anywhere else
