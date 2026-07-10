---
status: historical
note: "Planning brief for N3. N3 read as built and merged: master plan (9 Jul) Phase 2.1 lists the 'Batch B remainder' as N9/N4/N6/N7, excluding N3. Kept for decision rationale."
---

# N3 — Dashboard: Planning Brief (for review before build)

**Purpose of this doc:** fully plan N3 so the engineer (Codex/Claude Code) makes *near-zero* decisions — pixel execution + wiring to pre-decided data sources only. Every ambiguity is resolved here or flagged as a reviewer decision. Hand this to a reviewing model / sign off, then it becomes the build prompt.

**Repo:** `cap-pm-cockpit-alaria` (Next.js 14 / Supabase / Vercel) · **Ticket:** `09-N3-dashboard.md` · **Mockups:** `ResolvePM Dashboard.dc.html` (pixel target), `ResolvePM App Shell.dc.html` (shell it renders inside)
**Supabase project:** `CAP PM Dashboard` (`urmwilgdevgkdnntojnl`), live/production.

---

## 1. What N3 is and why
The Dashboard is the app's home screen after sign-in — it answers "what do I do next?" in four fixed-order cards (Win / Blocked / Today / Pointer). It currently renders **old pre-existing scaffolding** in production, not the new brand system. N3 replaces that.

It's part of **Batch B** (N3+N4+N6+N7); N3 goes first because **N2 (OS Reveal) depends on it** and it carries the one real data-layer decision the batch has.

## 2. Hard constraints (non-negotiable, no engineer judgement)
- **Strip, don't stack.** Delete the old Dashboard scaffolding markup entirely and replace it. Do NOT build the new design alongside/on top of the old. (This mistake was made once before and explicitly flagged not to repeat.)
- **Render inside the existing AppShell** (N1, already live) — do not recreate the header/brandmark/bottom-nav. Nav order is Dashboard · Roadmap · Work · Team · Knowledge.
- **Branch off `origin/main`** (`git fetch origin` first — local main has been stale twice on this repo). Branch-first → preview → verify → merge. Commit as you go and push (ephemeral container).
- **File-touch discipline:** only the Dashboard's own files. Don't touch shared shell/nav or files another Batch-B ticket owns — the batch must stay parallel-safe.
- **Pixel-exact to `ResolvePM Dashboard.dc.html`.** Tokens/type/spacing already in the mockup; introduce no new styles.

## 3. The real data model (verified against live Supabase, not assumed)
- **`work_items`** (93 rows): `title`, `description`, `status` (todo/in_progress/**blocked**/done/cancelled), `priority` (low/medium/high/critical), `type` (task/action/issue/risk/decision/dependency/milestone), `source_type` (roadmap/integration/global_capture/voice/agent/manual), `roadmap_trace`, `initiative_id`, `due_date`, `updated_at`, `project_id`, `user_id`, `confirmation_state` (draft/suggested/**confirmed**/rejected).
- **`initiatives`** (28 rows): roadmap themes — `name`, `priority`, `rag_status`, `target_metric`, `definition_of_done`, `project_id`.
- **`projects`** (17 rows): `id`, `user_id`, `roadmap_confirmed_at`. **Only place to persist a stable "win" pointer.**
- **`project_sessions`** (17 rows): Intake handoff — `recommended_action` (text), `top_priorities` (jsonb), `project_understanding` (jsonb), `what_not_to_do` (jsonb).
- **`team_threads`** (0 rows): role-scoped agent chat threads (product_director/growth_lead/tech_lead) — the future N5 home.
- **`updates`** (0 rows): `update_type` incl. `blocker` — structured blocker detail *could* live here, but table is empty.
- **NOT present:** any meetings/calendar table; any roadmap-change history/audit. These are the only two genuinely-missing sources — and they map exactly to the two cards that must be stubbed (below).

## 4. Card-by-card spec — mockup requirement → decided data source → real vs stub

### Card 1 — Win ("Decide this first") — **REAL, requires the one migration (§5)**
- **Mockup:** dark card, amber eyebrow "Decide this first", serif headline, support line, amber route pill. **Entire card is the tap target** (no button). Tapping **expands in place** (background dims, detail inline, no open/close buttons).
- **Content mapping (decided):** headline = `work_items.title`; support = `work_items.description` (fallback `roadmap_trace`); pill label = **"Opens work item"** (Alpha: always this — see routing).
- **Selection (decided):** the win is a single **persisted** `work_item` reference (see §5). Seeded deterministically: among the project's `work_items` where `confirmation_state='confirmed'` AND `status IN ('todo','in_progress')`, pick highest `priority`, tie-broken by earliest `created_at`. Persist that choice; do **not** recompute on every load.
- **Supersede rule (decided):** the win only changes when its work_item becomes `done`/`cancelled` (or is manually superseded) — then re-select by the same rule. **No refresh affordance** anywhere.
- **Routing (decided, removes a whole decision):** in Alpha the win **always opens its linked work_item** (routes into Work focused on that item). The mockup's "contextual routing to agent chat" is **deferred to N5** (agent chat doesn't exist yet) and "to roadmap" to N2 — so Alpha needs **no route-type enum**. Flag this deferral; don't build the agent-chat/roadmap branches.

### Card 2 — Blocked ("Waiting on") — **REAL where data exists**
- **Mockup:** white card, "Waiting on" eyebrow, title + "N days" chip, body; expanded adds "stalled since", "waiting on [who]", a note, and Send-reminder / View-in-Work actions.
- **Decided:** query `work_items` where `status='blocked'` for the project. Title = `title`; "N days" = days since `updated_at`; body = `description`. If none exist → **card hidden** (see empty-state rule).
- **Stubbed/omitted (no structured source):** "waiting on [person]" — no person field exists → **omit that line** (don't invent a name). The expanded "note" — would come from a linked `update` of type `blocker`, but `updates` is empty → **omit for Alpha**. "Send reminder" action → **placeholder** (no email/reminder system in Alpha), flag it. "View in Work" → real nav to Work.

### Card 3 — Today (schedule) — **to-dos REAL, meetings STUBBED**
- **Mockup:** "Today" eyebrow, rows of time + title + tag (mix of meetings and to-dos).
- **Decided (to-dos, real):** `work_items` with `due_date = today` (project-scoped), shown as rows (title + a "To-do" tag). No time column for to-dos (they have dates, not times).
- **Stubbed (meetings):** **there is no calendar/meetings data source in the schema.** Do **not** invent a meetings table. Meetings rows are **omitted** for Alpha. Flag "calendar/meetings integration" as a future data source. If no to-dos are due today → card shows a defined empty state (§ empty states).

### Card 4 — Pointer ("Worth a look") — **navigation REAL, dynamic text STUBBED**
- **Mockup:** lighter nudge, "Your roadmap shifted — two items moved," arrow → Roadmap.
- **Decided:** the card is a **real navigation** nudge → opens the Roadmap tab. The **"2 items moved" dynamic text is stubbed** — there is **no roadmap-change history** in the schema to detect movement. Use static copy ("Review your roadmap") rather than a fabricated change count. Flag roadmap-change-detection as future.

### Expand-in-place behaviour — **REAL (pure UI, no data)**
Tapping any card expands it in place: background dims, detail surfaces inline, no open/close buttons. Only one expanded at a time. This is pixel/interaction work straight from the mockup.

### Empty states (decided, so Codex doesn't invent them)
- **Win:** if no confirmed active work_item exists (shouldn't happen post-roadmap-confirmation, but defensively) → show a quiet "You're all caught up" style state (match mockup tone; no fabricated task).
- **Blocked:** if no blocked items → **hide the card entirely** (don't show an empty shell).
- **Today:** if nothing due today → show the card with a one-line "Nothing scheduled today" empty state.
- **Pointer:** always shown (static nudge).

## 5. THE decision needing reviewer sign-off — win-card persistence
The ticket requires the win to be **stored, stable state**, not recomputed. That needs somewhere to persist the selection. Two options:

**Option A (recommended) — minimal additive migration.**
Add two nullable columns to `projects`: `win_work_item_id uuid null references work_items(id)` and `win_set_at timestamptz null`. On first Dashboard load post-roadmap-confirmation, if `win_work_item_id` is null, select per §4-Card-1 and store it; thereafter read the stored one; re-select only on supersede. Additive, nullable, no backfill, low-risk on the live DB. This is the "correct" implementation the ticket asks for.

**Option B (lower live-DB risk, weaker) — deterministic derive-on-load, no migration.**
Compute the win from existing `work_items` each load by the same deterministic rule, accept that it *can* shift if underlying data changes, and defer true persistence. Ships without touching the schema, but **violates the ticket's "stable, not recomputed, no refresh" requirement** — so it's a knowing scope-reduction, not spec-compliant.

**Recommendation:** Option A. It's a genuinely small migration and it's what makes the win card behave as designed (and as N2's OS-reveal will assume). **Reviewer: confirm A, or accept B as a flagged Alpha shortcut.**

## 6. What Codex must NOT decide (all pre-resolved above)
- Which work_item is the win, how it's ordered, how it persists, when it changes → §4-Card-1 + §5.
- Win routing target → opens the linked work_item; agent-chat/roadmap routing deferred.
- What's real vs stubbed on each card → §4.
- Whether to invent a meetings table or a change count → **no** (§4 Cards 3–4).
- Empty states → §4.
- Anything about agent chat (N5), OS reveal (N2), reminders/email → **placeholder + flag**, don't build.
If anything here is genuinely underspecified at build time, Codex **flags it and stops**, does not guess.

## 7. Acceptance criteria
- Old Dashboard scaffolding fully removed; new Dashboard renders inside the existing AppShell, pixel-exact to `ResolvePM Dashboard.dc.html`, nav order intact.
- Four cards in fixed order; Win/Blocked/Today driven by **real** project-scoped Supabase data per §4; meetings + "items moved" correctly **stubbed/omitted**, not invented.
- Win card persists per the chosen §5 option, has no refresh affordance, and opens its linked work_item.
- Tap-to-expand works on each card (dim, inline detail, no buttons, one at a time).
- Empty states behave per §4.
- Only Dashboard files touched; `tsc`/`eslint`/`next build` clean; deployed to a preview off `origin/main`.
- A short report of exactly what is real vs stubbed, and (if Option A) the migration applied.

## 8. Open questions for the reviewer
1. **§5 win persistence: Option A (migration) or B (derive-on-load)?** — the one real decision.
2. **Win selection rule** — is "highest `priority`, tie-break earliest `created_at`, among confirmed todo/in_progress" the right definition of "the win"? Or should it seed from the Intake `recommended_action` on first load instead?
3. **Blocked "waiting on [person]"** — confirm we omit it in Alpha (no person field), rather than parse it out of `description`.
4. **Today card** — confirm meetings are omitted for Alpha (no calendar source), rather than showing an empty "meetings" section.
