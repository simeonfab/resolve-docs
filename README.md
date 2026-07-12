# resolve-docs

Document archive for the **Resolve** product family — an AI-powered project management suite for non-technical and solo founders.

- **[ResolveOS-intake-frontend](https://github.com/simeonfab/ResolveOS-intake-frontend)** — Resolve Intake: the guided 6-screen discovery flow (static site, live on Vercel).
- **[cap-pm-cockpit-alaria](https://github.com/simeonfab/cap-pm-cockpit-alaria)** — ResolvePM: the ongoing PM cockpit (Next.js 14 / Supabase / Vercel).
- **ResolveOS** — the role-based AI agent framework powering both. Role definitions live in Notion (Roles epic), not yet as code.

**Live project state lives in Notion, not here.** This repo holds artifacts: briefs, mockups, specs, strategy docs, and their history.

## Start here

1. `INDEX.md` — every document, one line each, with status.
2. `RESOLVE-ALPHA-MASTER-PLAN.md` — the top-level plan (phases, standing rules, reference file index).
3. `CLAUDE.md` / `AGENTS.md` — mandatory rules for any AI agent working in this repo.

## Structure

| Folder | Contents |
|---|---|
| `briefs/` | Current Alpha frontend build tickets (I/R/N series) + their batch plan |
| `resolve-pipeline/` | Phase 1 roadmap-generation pipeline modules (stages 1–7, integration map, test data) |
| `mockups/` | Current pixel-target mockups (the `*_dc.html` set) |
| `strategy/` | Strategy and reference docs: phases & commercial model, roles epic pointer, brand reference, competitive matrix, governance |
| `prompts/` | Executed agent prompts (Codex / Claude Code / ChatGPT) — historical record |
| `snapshots/` | Point-in-time copies of code/docs whose canonical home is a product repo |
| `archive/` | Everything superseded or spent. Never current. |

## Provenance

Imported 10 July 2026 from Claude chat artifacts (plus the project-knowledge mockup set). Browser download-counter suffixes (`-1`, `-2`, `-3`…) were stripped on import to restore original artifact names; each rename is recorded in that file's frontmatter `note:`. Three byte-identical duplicates were dropped. Two files were renamed beyond suffix-stripping, both recorded in frontmatter: the superseded early drafts of the I2/R1 briefs (to free their canonical names) and the ResolvePM `CLAUDE.md` snapshot (to prevent agent auto-load).
