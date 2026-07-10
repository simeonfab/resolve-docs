---
status: current
last_confirmed: 2026-07-10
authority: validated decision (Simeon)
volatility: medium
supersedes: none
review_due: on next frontier model release (e.g. GPT-6) or when the open experiment resolves
note: Canonical source of truth for cross-provider model routing. Operational extracts live in each product repo's CLAUDE.md and AGENTS.md and point back here. This is the reasoning; those are the runtime rules.
---

# Resolve — Model & Provider Routing Framework

## Status
Validated 2026-07-10 against real usage. Settled except for one open experiment (bottom). This doc is the reasoning and the decision record; the product repos carry only the operational extract needed to act.

## Why this exists
Routing model choice is not solved by writing better prompts — it's solved by deciding, once and correctly, at the moment work is specified, and carrying that decision as structured state rather than re-deriving it every session. This framework is the manual prototype of a future Resolve routing feature: today Simeon is the routing engine; eventually Resolve is.

## The core decision
The only question per unit of work: is this **thinking**, **building-easy**, **building-hard**, or **verifying**? That picks the model. Where the work sits in the roadmap is irrelevant.

Routing is decided **when the ticket is written** (by the planning model), never when it's picked up. It travels as a one-line header on the ticket — see Operational Contract.

## Tier by phase
| Phase | Model |
|---|---|
| Plan / discuss / resolve ambiguity / architecture | Fable (top reasoning) |
| Build: mechanical, fully specced (reskins, simple screens) | Mid (Sonnet 5 / Codex-Terra) |
| Build: novel or data-touching (pipeline, N5) | Top + high effort — provider per experiment below |
| Verify / "prove it works" | Top, different provider than the builder |
| Chat, lookups, boilerplate | Haiku 4.5 / Luna |

**Modifier:** ambiguous or risky → treat one tier higher. This single move absorbs the risk / ambiguity / required-quality factors; no further rubric needed.

## Provider ownership
| Layer | Owner | Why |
|---|---|---|
| Thinking: planning, architecture, structure-and-refusal reasoning | Claude (Fable) | Trusted over Sol-max; Sol didn't close the gap in testing |
| Visual origination — anything you want to *see* | Claude Design | Top tier, not close |
| Designed docs / decks / CVs | Claude | ChatGPT design output is poor (confirmed on CVs) |
| Backend build + GitHub/Codex integration | ChatGPT / Codex | Frictionless integration; Claude fights you off-desktop; efficient |
| Image generation | ChatGPT | Theirs — but near-zero weight, rarely used |
| Broad historical context on the user | ChatGPT | Life history lives there |
| Resolve project context | Claude (project + Notion) | Kept here |
| Throughput / efficiency | ChatGPT | More mileage on Plus than Claude on Max |

## Benchmark context (why coding isn't cleanly "won")
As of the GPT-5.6 launch (2026-07-09), the coding benchmarks conflict: Sol leads the Artificial Analysis Coding Agent Index (80, ~2.8 above Fable 5), while Claude leads SWE-Bench Pro (Mythos 5 ~80.3% vs Sol 64.6%). Neither provider is cleanly best at code — task shape decides, which is why the hard-build provider is settled by a real head-to-head on Simeon's own repos, not by a published number.

## Design chain
Claude Design originates the visual → Claude turns it into a spec/instructions → Codex or Claude Code implements. A Design mockup is still a mockup: it exits the pipeline only after being built against real data and verified on a live preview. Design originates; it does not get to be its own proof. Same done-gate as the `.dc.html` specs, with Claude Design as the upstream source.

## Builder / Reviewer / Proof (the verification model)
Three separate layers; nothing verifies its own work.
- **Builder** — produces the work.
- **Reviewer** — defaults to the *other* provider, but must also be *competent* for the task. Competence overrides cross-provider when they conflict — a different provider is not automatically a better reviewer.
- **Proof** — real-world evidence, and never optional. Review is not proof. Proof means: live preview against real data (UI), integration tests / logs / real API responses (backend), source + citation + freshness validation (research), competing hypotheses + explicit assumptions + a validation plan (strategy). Human review is a further layer, used whenever available.

A ticket without its named Proof is not done, regardless of how good the review was.

## Quota discipline
Commodity work sent to Claude burns Max quota on something ChatGPT does equally well for less. Route routine tasks off Claude.

## Operational contract (how this reaches the agents)
Each ticket carries one line, stamped at ticket-writing time:
`Route: <phase> · Builder: <model> · Reviewer: <model> · Proof: <real-world evidence required>`

**Agent gate:** before starting a ticket, the agent reads the Route line; if its session model is not the named Builder, it stops and flags — it does not build. A ticket with no Route line is a ticket failure: flag and ask, don't guess.

The four surfaces that enforce this framework:
1. `cap-pm-cockpit-alaria/CLAUDE.md` — operational extract for Claude Code
2. `cap-pm-cockpit-alaria/AGENTS.md` — operational extract for Codex (points at CLAUDE.md as authoritative)
3. This project's instructions (Claude chat nudge)
4. ChatGPT custom instructions (ChatGPT chat nudge)

All four are dated. When this framework changes, re-sync all four against this canonical doc.

## Open experiment (pending)
The hard-build provider. Race the pipeline replacement once — Claude Code vs Codex/Sol — and the better result sets the default for the "build: novel or data-touching" row. Record the outcome here (updating `last_confirmed`) and in the product repo Mistakes/decisions log when it resolves.

## Escalation ladder (do not pre-build)
- **v1 (current):** this framework, zero code. Route header + agent gate + chat nudges.
- **v2 (only if volume demands):** a slash command or hook that reads the Route header and hard-blocks builder mismatches.
- **v3 (Resolve product):** the real routing engine with structured state — context/reasoning/output protocols, staleness metadata, canonical context packets. This is a Resolve feature, not solo-founder tooling; build it when it's the product, not before.
