# Handoff: ResolvePM — Alpha Motion System

## Overview
This package specifies **every animation for the ResolvePM alpha**: the sign-in → app transition and the motion for the Dashboard, Roadmap, Work, and Knowledge (Library) tabs, plus a cross-cutting feedback (toast + undo) system. It is the single source of truth for how the app should *move*.

The centerpiece is **`Resolve Flow Spec.html`** — an implementation-grade document listing every animation as literal, copy-pasteable values (duration, easing, delay, stagger, from→to keyframes), organized into Surfaces A–I. Everything else in this bundle is a playable reference that demonstrates those values running.

## About the Design Files
The files in this bundle are **design references created in HTML** — motion prototypes showing intended timing and behavior, **not production code to copy directly**. Each `*.Motion.dc.html` file is a "workbench": a phone mockup on the left that runs each animation for real, and a spec rail on the right printing that animation's exact values. What plays is exactly what's printed.

Your task is to **recreate these animations in the target codebase's existing environment** (React, Vue, SwiftUI, native, etc.) using its established patterns and animation libraries — or, if no environment exists yet, to choose the most appropriate framework and implement there. The reference files happen to use the Web Animations API (`element.animate(keyframes, options)`), but the values are framework-agnostic: the cubic-bezier easings paste directly into CSS `animation-timing-function`, Framer Motion, React Spring, Core Animation, etc.

## Fidelity
**High-fidelity (motion).** Durations, easings, delays, staggers, and keyframe transforms are final and internally consistent — implement them as specified. The surrounding UI in each workbench is representative (real layout, colors, and copy) but is **not** the pixel-perfect screen spec — the canonical screen designs live in the app's own screen files (`ResolvePM Dashboard.dc.html`, `ResolvePM Roadmap Tab.dc.html`, `ResolvePM Work Tab.dc.html`, `ResolvePM Knowledge Base Tab.dc.html`, etc.). Build the screens from those; take the motion from here.

## Motion character
All animations are locked to a **"match-seam"** character — a light spring on arrivals/expansions/sheets, and a calm ease on fades/dims. A "Slower" alternative exists in each workbench for comparison only; **ship "match-seam".**

### Easing tokens (use everywhere)
| Token | cubic-bezier | Use for |
|---|---|---|
| `--e-spring` | `cubic-bezier(.34,1.56,.64,1)` | arrivals, expansions, sheets, panels (slight overshoot) |
| `--e-out` | `cubic-bezier(.22,.61,.36,1)` | decelerate reveals, restores |
| `--e-std` | `cubic-bezier(.4,0,.2,1)` | dims, presses, tab-color, filter reflow |
| `--e-in` | `cubic-bezier(.55,0,.85,.35)` | exits, collapses, route-out, dismissals |

All timings assume WAAPI `fill: 'both'`. Staggered steps apply `delay + (index × stagger)` per element in DOM order.

## Surfaces (the animations)
Full literal values for each are in **`Resolve Flow Spec.html`** — open it in a browser. Summary:

- **A–D · Sign-in → app transition** — dark sign-in, the "bring it in" intake bridge, and the dark→light OS reveal. Amber is the through-line so the transition reads as arrival.
- **E · Dashboard** — (E1) screen entrance: top bar → card stack (stagger 70ms) → global bar; (E2) blocked card → expand with backdrop dim + recede + staggered detail reveal; (E3) expand → collapse; (E4) global bar → agent-chat sheet (reused across tabs); (E5) win card idle arrow loop + tap route-out; (E6) pointer card route-out; (E7) profile menu open; (E8) tab switch (directional).
- **F · Roadmap** — (F1) entrance; (F2) view toggle By phase ⇄ By theme (directional, spring thumb); (F3) action card → expand inline + "Go to work item"; (F4) collapse; (F5) "whole picture" matrix pull-up overlay; (F6) onboarding → tab slide-away.
- **G · Work** — (G1) entrance with progress-bar fill; (G2) task check-off (tick only) — row collapses out of list, progress advances; (G3) open item → editor push-in (with End date); (G4) capture → inbox; (G5) view completed (push-in, lives in Knowledge); (G6) global bar → chat (reused).
- **H · Knowledge / Library** — (H1) library entrance (grid pop-in); (H2) filter chips; (H3) add / brain-dump bottom sheet + new card lands; (H4) open item → preview push-in with Download; (H5) global bar → chat (reused).
- **I · Feedback (cross-cutting)** — (I1) confirmation toast + undo with 4s depleting timer; (I2) undo reverses the action; (I3) plain confirmation (no undo, 2.6s). One toast at a time; a new action fast-swaps the current (160ms out → in).

## Key interaction rules (don't lose these)
- **Work tab: the tick and the row do different things.** Tapping the **tick** completes an item; tapping the **row body** opens it for editing. Only the tick completes.
- **Undo cancels pending follow-through.** When Undo fires, cancel any queued timer (e.g. the check-off's row-collapse) or the action completes anyway after dismissal.
- **Reversible vs not.** Reversible/destructive actions → toast **with** Undo + timer (I1). Non-reversible/low-stakes (Send reminder, Save) → plain toast, no Undo (I3).
- **Global bar is reused, not rebuilt.** E4 is the one chat-open animation; every tab points at it. Only the resting caption changes per tab.
- **Matrix is a pull-up overlay, not a tab.** The Roadmap "whole picture" is a zoom-out you pull up (grid icon) and dismiss — deliberately not a 6th bottom tab or a 3rd toggle segment.

## Shared / consistent values (verified)
- **Header entrance:** 340ms `--e-out`, `translateY(-8px)` → none (all tabs).
- **Global bar rise:** 480ms `--e-spring`, `translateY(30px)` → none, delayed to land last (all tabs).
- **Grouped-section stagger:** 80ms.
- **Bottom sheets (chat + add):** 460ms `--e-spring`, `translateY(100%)` → none.
- **Push panels (editor, completed, preview):** in 420ms `--e-spring` `translateX(100%)→0`; close 300ms `--e-in` `0→translateX(100%)`; contents reveal staggered ~160ms delay / 45ms stagger.
- **Task check-off:** tick fill pop 300ms spring (scale 1→1.25→1), text strike+dim, row collapse 340ms `--e-in` (height + margin + padding → 0), progress advances 520ms `--e-out`.

## Out of scope for alpha
- **Team tab** — not built this version; its header reads **"Coming soon."**
- **Agent chat conversation** (send → thinking → streamed reply) — deferred post-alpha. The global bar → chat *opening* (E4) IS specced and in use.
- **Touch gestures** (swipe-to-dismiss, press-drag) — descoped; taps only.

## Files in this bundle
- **`Resolve Flow Spec.html`** + `doc-page.js` — the authoritative motion spec (Surfaces A–I). **Start here.**
- `ResolvePM Dashboard Motion.dc.html` — Dashboard workbench (Surface E).
- `ResolvePM Roadmap Motion.dc.html` — Roadmap workbench (Surface F).
- `ResolvePM Work Motion.dc.html` — Work workbench (Surface G).
- `ResolvePM Knowledge Motion.dc.html` — Knowledge/Library workbench (Surface H).
- `ResolvePM Feedback Motion.dc.html` — Toast + undo workbench (Surface I).
- `support.js` — runtime for the `.dc.html` workbench files (needed only to open them locally; not for production).

To run a workbench locally, open its `.dc.html` file in a browser: toggle **Match seam / Slower** at the top, then hit **▶ play** on any row in the spec rail (or interact with the phone directly).
