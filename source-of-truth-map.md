---
status: current
last_confirmed: 2026-07-10
authority: validated structure (Simeon); resolve-docs-local entries verified on filing 2026-07-10; cross-repo entries (ResolveOS) pending the ResolveOS filing pass
volatility: medium
supersedes: none
review_due: whenever a canonical artifact moves, or a new one is created
note: The single index of where every canonical thing lives. Step 2 of the ResolveOS refresh. Before acting on any topic, come here to find its canonical home. If a topic isn't listed, it has no home yet — that's a gap to close, not permission to freelance. Entries touching the ResolveOS repo are marked [CROSS-REPO] and are confirmed once that repo's filing pass lands. Repo ownership marked [CONFIRM ORG] is unverified — Simeon indicated resolve-docs and ResolveOS sit in different orgs; correct before relying on it.
---

# Resolve — Source-of-Truth Map

Names in this file follow `resolve-naming-glossary.md`. Read that first if a name is unclear.

## How to use
1. Find your topic below.
2. Go to its **canonical home** — that is the authoritative version.
3. If two locations are listed, the one marked *canonical* wins; the others are aliases, extracts, or to-be-consolidated.
4. If your topic isn't here, it doesn't have an agreed home — flag it, don't invent one.

## The map

| Topic | Canonical home | What it's for | Status |
|---|---|---|---|
| **Naming — repos, layers, aliases** | `resolve-docs` → `resolve-naming-glossary.md` | What everything is called | Current |
| **This map** | `resolve-docs` → `source-of-truth-map.md` | Where everything lives | Current |
| **Session status / what's next** | Notion → **Active Priorities** (`390d8b53…d54c`) | Primary session-to-session handoff | Current (daily) |
| **Role definitions (the four roles)** | Notion → **Roles Epic** (`393d8b53…70b6`) | Product Director, Growth Lead, Tech Lead, Implementation Engineer — full definitions | Current. [CROSS-REPO] Codification to `ResolveOS` `02-roles/*.md` lands in the ResolveOS filing pass. |
| **Model/provider routing** | `resolve-docs` → `strategy/resolve-routing-overlay.md` | Which model/provider for which task | Current. Provider-agnostic tiers live in `ResolveOS` `00-system/capability-tiers.md`; concrete provider mapping is in this overlay. (The former `resolve-routing-framework.md` was a phantom — never existed; replaced by the tiers+overlay split 2026-07-10.) Runtime extract in product-repo CLAUDE.md/AGENTS.md. |
| **Chat routing nudges (paste-ins)** | `resolve-docs` → `strategy/resolve-routing-overlay.md` (Paste-in section) | Text for the two chat instruction boxes | Current. (The former standalone `routing-snippets.md` is superseded by the overlay, which now carries the paste-in blocks; not filed separately.) |
| **ResolveOS logic / entrypoint** | `ResolveOS` repo → `00-system/resolveos-entrypoint.md` | Routing + file-loading behaviour | [CROSS-REPO] Being refreshed to the four roles + capability tiers in the ResolveOS filing pass; confirm on landing. |
| **Theme definition (lever, no finish line)** | Notion → **Active Priorities** (`390d8b53…d54c`) | How Intake defines a roadmap theme vs a task | Confirmed 2026-07-10 (also referenced in Technical Foundation `391d8b53…00e1`) |
| **Roadmap generation pipeline (replacement design)** | Notion → **Active Priorities** (`390d8b53…d54c`) | Two-gate floor + referee design replacing the broken pipeline | Confirmed 2026-07-10; in design |
| **Technical foundation & product decisions** | Notion → **ResolvePM — Technical Foundation & Product Decisions** (`391d8b53…00e1`) | Schema, product decisions | Current |
| **Validation & evidence** | Notion → **Validation & Evidence** (`390d8b53…ae8b`) | What's been tested, what the evidence says | Current |
| **Alpha plan / sequencing** | `resolve-docs` → `RESOLVE-ALPHA-MASTER-PLAN.md` (root) | Current Alpha sequencing | Current — verified present on filing 2026-07-10. Supersedes original master plan. |
| **Design mockups (`.dc.html`)** | `resolve-docs` → `mockups/` + product repo | Literal pixel-exact screen specs | Current — `mockups/*_dc.html` verified present on filing 2026-07-10. |
| **Product-repo agent rules** | `cap-pm-cockpit-alaria` → `CLAUDE.md` / `AGENTS.md` | Runtime rules for Claude Code / Codex | Current. Note: the routing section currently points at the phantom `resolve-routing-framework.md` — repoint to the overlay when the product repo is touched (out of the doc-repo passes). |
| **Lessons learned** | Notion → **Lessons Learned — Role Agents & Codex** (`393d8b53…a830`), organised in five domains | Mistakes → prevention rules | Current — consolidated 2026-07-10 (see five-domain organisation below). |

## Lessons learned — five-domain organisation (consolidated 2026-07-10)
The step-3 `[SCATTER]` is resolved: the canonical home is the Notion **Lessons Learned** page, re-sectioned into five domains rather than one flat list. Each domain also has its operational enforcement surface:
- **Agent discipline** → Notion Lessons §1 + product-repo `CLAUDE.md` / `AGENTS.md`
- **Data reality** → Notion Lessons §2 + product-repo `CLAUDE.md`
- **Pipeline & quality** → `resolve-pipeline/` + the Tech Lead role file
- **Product & positioning** → Notion Technical Foundation & Product Decisions + Evidence Log + the Product Director role file
- **Source-of-truth & doc** → `resolve-docs` `06-governance/`

## Repos at a glance
Ownership marked [CONFIRM ORG] — Simeon indicated the doc repos sit in different orgs; the values below are memory-sourced and unverified.

| Repo | Holds | Owner |
|---|---|---|
| `ResolveOS` | ResolveOS (defined) — framework, roles, entrypoint | [CONFIRM ORG] |
| `resolve-docs` | Documentation archive, strategy, mockups, pipeline design | [CONFIRM ORG] |
| `cap-pm-cockpit-alaria` | ResolvePM app (Next.js/Supabase) | simeonfab (not transferred — Vercel dependency) |
| `ResolveOS-intake-frontend` | Resolve Intake (static site) | simeonfab (not transferred) |

## Known gaps (no canonical home yet — flagged, not invented)
- **ResolveOS (runtime)** — the in-product execution layer. Own epic, not yet designed.
- **Master records** (cross-role awareness) — Roles Epic task 8b, not started.
- **Role-execution / journey-mapping epic** — distinct from role *definitions*; no home yet.

## Filing status (this pass — resolve-docs, 2026-07-10)
- [x] Map + glossary filed into `resolve-docs`; both added to `INDEX.md`.
- [x] `[UNVERIFIED]` entries verified against the repo: Alpha master plan (root) and `mockups/*_dc.html` both present; flags removed.
- [x] `[SCATTER]` resolved → Notion Lessons Learned, five-domain organisation, consolidated 2026-07-10.
- [x] Routing rows repointed to `resolve-routing-overlay.md`; `resolve-routing-framework.md` confirmed phantom; `routing-snippets.md` superseded by the overlay (not filed).
- [ ] [CROSS-REPO] Confirm the ResolveOS-repo rows (role-file codification, entrypoint refresh) once the ResolveOS filing pass lands.
- [ ] [CONFIRM ORG] Correct the repo-ownership table with the real orgs.
- [ ] Product-repo `CLAUDE.md` routing pointer repointed off the phantom (separate, out of the doc-repo passes).
