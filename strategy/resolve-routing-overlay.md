---
status: current
note: "Concrete instantiation of ResolveOS capability tiers (00-system/capability-tiers.md) for the Resolve project. This file names real models/vendors, so it lives here, not in ResolveOS core. Update when subscriptions or model line-ups change; the core tiers file does not change with it. Runtime extract lives in the product repos' CLAUDE.md/AGENTS.md."
---

# Resolve — Routing Overlay (tiers → actual tools)

Maps the provider-agnostic ResolveOS capability tiers to the tools actually in
use on the Resolve project as of July 2026.

| Tier | Runs on | Notes |
|---|---|---|
| reasoning-heavy | Claude Fable (Max) | Thinking, planning, architecture stays here. Protect this quota — it's the constrained resource. |
| mechanical | Claude Sonnet or Codex-Terra | Fully-specced builds and filing. **Exception — build-hard:** novel or data-touching builds route to a top model on high effort, not the mechanical default. |
| commodity | Claude Haiku or ChatGPT | Lookups, boilerplate, status chat. Explicitly to protect Max quota. |
| cross-check | Top model, **different provider than whoever built it** | e.g. Codex-built work verified by Claude, Claude-built work verified by a non-Claude model. |

**Modifier (from core):** ambiguous or risky → one tier up.

**Nudge behaviour (for chat instruction boxes):** classify silently at the
start of each task; speak up only on a significant tier mismatch or at a phase
boundary, with a single line; never repeat within the same task; never block
or delay the actual answer; never nudge over small stuff.

**Open item:** the pipeline-replacement provider race (Claude Code vs
Codex/Sol) settles the default tool for build-hard. Log the result here and in
the product-repo CLAUDE.md when it lands.

## Paste-in — chat instruction boxes

> Routing nudge: at the start of each new task, silently classify it as
> thinking / build-easy / build-hard / verify / commodity. Table: thinking,
> planning, architecture → Fable (stay here); build-easy (mechanical, fully
> specced) → Sonnet or Codex-Terra; build-hard (novel or data-touching) → top
> model on high effort; verify → top model, different provider than whoever
> built it; commodity chat, lookups, boilerplate → Haiku or ChatGPT (protect
> Max quota). Modifier: ambiguous or risky → one tier up. Speak up only on a
> significant tier mismatch or at a phase boundary, with a single line. Never
> repeat the nudge within the same task, never block or delay the actual
> answer, never nudge over small stuff.
