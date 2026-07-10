---
status: current
---

# Feature Comparison: ResolveOS vs BMAD-METHOD vs GitHub Spec-Kit

Compiled from direct review of ResolveOS source files (README, entrypoint, all role/skill/workflow files) and web research on BMAD-METHOD and GitHub Spec-Kit. Where information could not be verified from available sources, it is marked **Unverified** rather than guessed.

| Dimension | ResolveOS | BMAD-METHOD | GitHub Spec-Kit |
|---|---|---|---|
| **Access model** | Markdown files loaded into a ChatGPT Project / Claude context. No install. | npm install (`npx bmad-method install`). Requires Node.js 20.12+, Python 3.10+, uv. | CLI tool (`specify init`), uv/pipx package manager. |
| **Primary orientation** | Project operating layer — covers product strategy, commercial strategy, requirements, architecture, implementation, QA, AND information storage governance across tools. Broader than code. | Software development lifecycle — analysis → PRD → architecture → dev → QA. Code-centric. | Spec-driven **code generation** specifically — spec → plan → tasks → implement. Narrower than ResolveOS. |
| **Primary interface** | Conversational — talk to it in plain English inside a chat. | Mix: conversational agent personas + CLI-driven workflow inside coding tools (Cursor, Claude Code, Codex). | CLI-first, generates files consumed by coding agents via slash commands. |
| **Number of defined roles** | 4 (Product Director, Growth Lead, Tech Lead, Implementation Engineer) | 12–21 depending on module (Analyst, PM, Architect, PO, Scrum Master, Dev, QA, Orchestrator, Tech Writer, +) | None as personas — uses phase-based commands, not role personas. |
| **Role definition format** | Markdown with YAML frontmatter, `Purpose`/`When To Use`/behaviour sections. | Markdown/YAML "Agent-as-Code" files — persona, commands, dependencies. | N/A (no roles) |
| **Entrypoint/routing logic** | Yes — single `resolveos-entrypoint.md` classifies interaction type and routes to governing workflow via a deterministic routing table. | Orchestrator agent coordinates; routing logic less centrally documented in sources reviewed. **Unverified** in detail. | Sequential command flow (`/speckit.constitution` → `/speckit.specify` → `/speckit.plan` → `/speckit.tasks` → `/speckit.implement`), not a flexible router. |
| **Governance / constitution file** | Yes — `06-governance/` (architecture decisions, source-of-truth rules, duplication control, codex working rules, project readiness, update process). | Governance present but documented as "versioned governance" generally; specific file structure **unverified**. | Yes — `constitution.md`: non-negotiable project principles (coding standards, workflow rules). Narrower scope than ResolveOS governance (code-focused, not multi-tool). |
| **Source-of-truth ownership across tools** (e.g. Notion vs Jira vs GitHub) | Yes — explicit `information-architecture.md` skill + `source-of-truth-rules.md` governance. Decides which tool owns which fact type before writing anything. | **Unverified** — no evidence found of explicit cross-tool source-of-truth arbitration; BMAD artifacts are designed to live in the repo. | No — Spec-Kit assumes the repo itself is the source of truth (specs live in `.specify/`); does not arbitrate across external tools like Notion/Jira. |
| **Missing-context / anti-hallucination rules** | Yes — explicit tiered response (ask smallest input / give caveated general guidance / stop), with a mandated "Blocked" output format. | **Unverified** — not found in sources reviewed. | Not explicitly — relies on spec completeness/checklists rather than a formal "stop and ask" protocol. |
| **Acceptance criteria discipline** | Yes — dedicated skill, observable outcomes, testability, drift detection. | Yes — PM agent defines FRs/NFRs as part of PRD. | Yes — built into spec templates and "quality checklists" (described as "unit tests for English"). |
| **Ticket/task writing** | Yes — dedicated skill (`ticket-writing.md`), sequencing, blockers, non-goals. | Yes — Scrum Master/PO shard specs into stories. | Yes — `/speckit.tasks` generates `tasks.md` with dependency ordering and parallel-execution tags. |
| **Completion honesty / no-fake-completion enforcement** | Yes — explicit skill + workflow (`implementation-review-loop.md`) triggered by failed builds, drift, stuck loops; bans "fake functionality" claims. | Partially — QA agent role exists, but explicit "no fake completion" language as a named principle is **unverified** in BMAD sources reviewed. | Partially — `/speckit.analyze` does cross-artifact consistency checking, but no explicit "false completion" guardrail found. |
| **Requirement traceability** (intent → requirement → code → validation) | Yes — dedicated skill, explicit principle of preserving original intent through every stage. | Implied via document handoffs (Brief → PRD → Architecture → Story) but not named as a discrete traceability discipline. | Yes, structurally — spec→plan→tasks are linked artifacts; "intent is the source of truth" is a stated philosophy. |
| **Dependency management as discrete skill** | Yes — `dependency-management.md`, explicit "current-ticket-first" discipline. | Not found as a separate discrete skill — embedded in PM/Architect role behaviour. | Tasks have dependency ordering (model→service→endpoint) but framed as a feature of `/speckit.tasks`, not a standalone skill. |
| **Feedback processing as discrete skill** | Yes — `user-feedback-processing.md` + `feedback-to-ticket.md` workflow, explicit separation of raw feedback from interpretation. | **Unverified** — not found as a discrete artifact in sources reviewed. | Not found — feedback loops not a named feature. |
| **AI-tool agnosticism** | Yes — explicit rule to avoid locking language to one AI provider; works for human, AI chat, or coding agent roles. | Partial — works across Claude Code/Cursor/Codex but agent personas and commands are somewhat tool-specific. | Yes — explicitly 30+ integrations, designed to be agent-agnostic. |
| **Maturity / ecosystem signal** | Solo project, ~3–4 weeks of active development, no external users yet (per your own account). | Documented across many independent blogs/tutorials since late 2025, dedicated docs site, active npm package, community marketplace for custom agents/workflows. | Backed by GitHub directly; 70+ community extensions; explicitly framed as "the new GitHub-endorsed standard" for AI-assisted dev. |
| **Cost** | Free (personal use) | Free, open source | Free, open source |
| **Non-coding domains covered** (commercial strategy, role recommendation for non-dev work, multi-tool documentation governance) | Yes — explicitly covers product strategy, commercial strategy, planning/prioritisation, general project guidance, not just software delivery. | No — scoped to software development lifecycle. | No — scoped to code generation. |

---

## Where ResolveOS appears to have a genuine edge (verified, not assumed)

1. **Cross-tool source-of-truth arbitration.** Neither competitor appears to explicitly decide *which external tool* (Notion vs Jira vs GitHub vs a local doc) should own a given fact before writing it. ResolveOS's `information-architecture.md` does this directly. This is your strongest, most defensible differentiator from what I can verify.

2. **Formal missing-context / stop-and-ask protocol.** ResolveOS has a named, structured response (smallest missing input → caveated guidance → hard stop with a fixed output format). I found no equivalent named mechanism in either competitor's public documentation.

3. **Breadth beyond software.** Both competitors are scoped to the software development lifecycle. ResolveOS explicitly covers commercial strategy, general project guidance, and role recommendation for non-coding work. This is a real scope difference — but it's also a risk (see gaps below): breadth without depth in each area is a weaker pitch than BMAD's narrow, deep dev focus.

4. **Conversational, no-install entry point.** Lower friction for non-developers — no CLI, no Node/Python/uv setup. This matters if your target user is a PM/founder rather than a developer.

## Where ResolveOS has real gaps (verifiable, not invented)

1. **No CLI or tooling integration.** BMAD and Spec-Kit both integrate directly into the coding agent's workflow (slash commands, skills folders, IDE-level hooks). ResolveOS depends entirely on the user manually pasting/uploading context into a chat — there's no mechanism to "live" inside a repo the way Spec-Kit's `.specify/` folder does.

2. **No multi-agent orchestration.** BMAD explicitly runs *multiple* agent personas that hand off to each other with defined artifacts. ResolveOS's roles are described as things a single assistant can "adopt," not separate coordinating agents. If multi-agent handoff turns out to matter to users, this is a structural gap, not a tweak.

3. **No public adoption, docs site, or community signal.** Both competitors have external validation (docs sites, blog coverage, GitHub stars, in BMAD's case a marketplace). ResolveOS currently has none. This isn't a feature gap, but it's a credibility gap if you're trying to convince anyone this is more than a personal tool.

4. **Unverified: explicit no-fake-completion enforcement isn't unique.** Worth confirming directly (try BMAD or Spec-Kit yourself) before claiming this as a differentiator publicly — my research found it implied in both but not as explicitly named as in ResolveOS's `implementation-review-loop.md`.

5. **No installed/portable artifact.** ResolveOS lives as markdown you paste into a chat project. Spec-Kit and BMAD install into the actual project repo and persist via git. That makes their artifacts auditable in version control alongside code — yours currently are not, unless you're separately committing them to your own repo (which you are, but the *workflow* doesn't yet treat repo-residency as central the way Spec-Kit does).

---

## Honest caveat

This matrix is built from your source files (which I read directly) plus public documentation and third-party write-ups about BMAD and Spec-Kit (I did not install or test-drive either). Anything marked **Unverified** should be confirmed by actually trying their tools before you rely on it for a competitive claim — secondhand blog descriptions can miss real features either product has.
