---
status: current
last_confirmed: 2026-07-10
authority: validated decision (Simeon)
volatility: low
supersedes: none
review_due: when a repo is renamed/created, or a new product-family member is added
note: Canonical names for the whole Resolve estate. Step 1 of the ResolveOS refresh track. Everything downstream (source-of-truth map, role files, ChatGPT logic) references these names — if a name changes, change it here first, then propagate.
---

# Resolve — Naming Glossary

The single place that says what everything is called. Use these names everywhere; do not coin new ones ad hoc. If two names refer to the same thing, this file names the canonical one and lists the alias.

## A. Product family (what the user sees)
The user experiences one product called **Resolve**, not separate tools handing off to each other.

| Canonical name | What it is |
|---|---|
| **Resolve** | The whole product family. One thing from the user's perspective. |
| **Resolve Intake** | The guided six-screen onboarding flow. |
| **ResolvePM** | The ongoing project-management cockpit. |
| **ResolveOS** | The underlying role/agent framework powering Intake and ResolvePM. |

## B. Repos (true identity of each)
| Repo | Is | Notes |
|---|---|---|
| `ResolveOS-intake-frontend` | The Resolve Intake repo | Static site, Vercel-deployed |
| `cap-pm-cockpit-alaria` | The ResolvePM repo | Legacy technical name — see D3 |
| `resolve-docs` | The documentation archive | Under the ResolveOS GitHub org |
| `ResolveOS` | The ResolveOS logic/framework repo | Renamed from `ResolveOS-beta` — see D1 |

## C. Framework layers (so we stop conflating them)
| Term | Means |
|---|---|
| **ResolveOS (defined)** | The roles, skills, workflows, and governance as *definitions* — the documentation/thinking. Lives in the `ResolveOS` repo. |
| **ResolveOS (runtime)** | ResolveOS actually *executing inside* Resolve Intake / ResolvePM — roles attached to real user messages, in-product. A distinct, later epic; not the docs. |
| **ResolveOS logic / entrypoint** | The routing + loading behaviour (`resolveos-entrypoint.md`) that decides which files load for a given interaction. |
| **The four roles** | Product Director, Growth Lead, Tech Lead, Implementation Engineer. Settled — no other roles are current. |

**Retired role names** (do not use — folded into the four): Strategic Product Director & Product Manager → Product Director; Technical Strategy Lead → Tech Lead; QA Tester → a mode within Implementation Engineer; Business Analyst → Product Director.

## D. Decisions folded in (2026-07-10)

**D1 — the ResolveOS repo.** `ResolveOS-beta` becomes the canonical `ResolveOS` repo. The pre-existing repo named `ResolveOS` is superseded (it was the beta files, slightly extended, no longer needed). Safe path: archive the old one (rename to `ResolveOS-archive`) to free the name and preserve its files, rename `ResolveOS-beta` → `ResolveOS`, confirm nothing was lost, then delete the archive. Renaming `-beta` is low-risk (docs/logic repo, no deployment wired to it).

**D2 — defined vs runtime.** "ResolveOS (defined)" = the documentation/framework. "ResolveOS (runtime)" = the in-product execution layer. These are different things and must be named distinctly to avoid talking past each other.

**D3 — legacy technical IDs kept as aliases (not renamed).** `cap-pm-cockpit-alaria` (repo) and "CAP PM Dashboard" (Supabase project, ID `urmwilgdevgkdnntojnl`) are technical IDs that are **aliases of ResolvePM**. Not renamed — they're wired to Vercel/Supabase and renaming mid-Alpha risks breaking deployment. Spoken/canonical name is always **ResolvePM**.

## Aliases quick-reference
- ResolvePM = `cap-pm-cockpit-alaria` (repo) = "CAP PM Dashboard" (Supabase project)
- Resolve Intake = `ResolveOS-intake-frontend`
- ResolveOS (defined) = the `ResolveOS` repo (formerly `ResolveOS-beta`)
- "Nook & Thread rebrand" = the fictional example project in mockups — never real data
