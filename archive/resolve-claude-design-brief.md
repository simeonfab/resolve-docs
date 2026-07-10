---
status: historical
note: "Front-page / visual-system redesign brief - executed. Visual authority now strategy/resolve-brand-reference.md + the dc mockups."
---

# Resolve — Design Brief for Front Page & Visual System Redesign

Screenshots of the current build are attached separately — use them as the literal current state, this brief explains the context, constraints, and what needs to improve.

---

## What Resolve is

Resolve is an AI-assisted project clarity tool. It takes a messy, scattered project (notes everywhere, old AI chats, half-formed plans) and turns it into a clear understanding and one concrete next move. Primary audience: non-technical founders, solo product people, and small business owners — not developers. The tone should feel like a sharp, calm colleague who actually read what you wrote, not like enterprise software or a generic AI chatbot wrapper.

Direct competitors (BMAD-METHOD, GitHub Spec-Kit) are CLI/developer-tool-coded and visually utilitarian. Resolve's visual identity should lean the opposite way — warm, confident, editorial — because that's also a genuine product differentiator: approachable for someone who's never touched a terminal.

---

## Current design system (what exists today — starting point, not gospel)

**Palette:**
- Body background: `#F2EDE4` (warm cream)
- Ink/primary text: `#1A1A18`
- Amber: `#C8A040` — used for active states, progress indicators, accents
- Terracotta: `#8B4A2A` — used for eyebrow labels, section headings, card labels
- Cards: `#FFFFFF`
- Dark nav/header: `#1E1E1A`
- Borders: `#DDD8CE`

**Typography:**
- Display: DM Serif Display — used for H1/H2 only, one dominant serif element per screen
- Body: Inter — everything else

**Layout pattern:** Mobile-first, single column, card-based content blocks, a persistent step progress indicator, sticky bottom action bar with primary/back buttons.

This palette and type pairing came out of deliberately avoiding the "default AI app" look (the skill-flagged combo of warm cream + serif + terracotta, which is a known generic pattern) by adding the dark nav contrast and confident ink-black CTAs instead of a more expected blue/teal. **It's a reasonable starting point, but it hasn't been stress-tested by a real designer's eye** — that's exactly the gap this brief is for.

---

## What's working (keep these principles, even if exact values change)

1. **One dominant focal point per screen.** Every screen leads with one big serif heading and one clear next action — not competing elements.
2. **The processing/reveal moment matters emotionally.** Between the user submitting their messy input and seeing the AI's understanding, there's a short animated "thinking" state. This is a deliberate emotional beat — the reveal of understanding should feel earned, not instant. Don't flatten this into a simple spinner.
3. **The "win" moment at the end matters.** After completing the flow, there's a screen that explicitly names what the user just accomplished ("You just cleared the fog") before showing next steps. This should feel like a small, genuine moment of accomplishment — not a notification.
4. **Icons over labels where possible.** Section headers use a small icon + plain English rather than ALL CAPS jargon labels.

---

## What needs real design improvement (the actual brief)

1. **The current palette is functional but not distinctive enough.** It was assembled through iterative tweaks, not a single confident designed system. Wants: a genuinely considered palette that still feels warm/editorial/non-corporate, but reads as intentional rather than default. Open to amber/terracotta being replaced entirely if something better serves the brand — nothing here is precious except the *principle* of warm-not-corporate.

2. **Typography pairing should be reconsidered properly.** DM Serif Display + Inter is a reasonable but very common pairing (verges on the other flagged default combo). Wants: a pairing that still signals "considered editorial product," not generic SaaS.

3. **The front/landing screen (Screen 0 — name entry + journey selection) is the first impression and currently the weakest visually.** It's mostly empty space with a single input and three cards. This is the screen to be boldest on — it needs to feel like the start of something, not a form.

4. **Card design system needs more personality.** Currently flat white cards with thin borders — functional but generic. Open to texture, subtle shadow language, asymmetry, or anything that makes the "this is Resolve" feeling more recognizable at a glance.

5. **The dark nav/header treatment** is the strongest contrast element currently — worth deciding whether to lean further into a bold dark/light split as the signature device, or move away from it entirely for something more distinctive.

---

## Constraints (non-negotiable, regardless of visual direction)

- Must work as a mobile-first, single-column layout (this is primarily a phone experience) that also scales sensibly to desktop.
- Must support a step-based progress indicator — users need to always know where they are in a multi-step flow.
- Must support a sticky/persistent bottom action bar with a primary action button (and sometimes a back button).
- Text-heavy screens are unavoidable — some screens show 4+ short text blocks (project understanding, assumptions, roadmap phases). The system needs a card/block treatment that stays legible and uncluttered even with real content density, not just in idealized mockup states.
- Avoid corporate/SaaS-generic blue, avoid AI-hype neon/gradient styling, avoid anything that reads as "developer tool."

---

## Specific moments to design with intention, not just inherit

- **The processing/reveal animation** (AI "thinking" state) — currently a simple amber ring. Could be more visually distinctive while staying tasteful and not gimmicky.
- **The win/completion moment** — currently a dark banner with a trophy emoji and bold text. This is one of the few places where more visual ambition is explicitly welcome — it's meant to feel rewarding.
- **Empty/example states** — there's a "load an example project" affordance for new users to see the tool work with realistic content. Worth a clear visual treatment that signals "this is a demo, you can replace it" without looking like an error state or placeholder text.

---

## What to deliver

A redesigned visual direction for the front page (Screen 0) specifically, plus an articulated palette + type system that could extend across the rest of the flow (the other screens are functionally similar — confirmation cards, a recommendation screen, a roadmap, a final actions screen). Don't feel constrained to the current hex values or fonts listed above — they're the honest current state, not a brief to match. The goal is something more confidently designed than what currently exists, while keeping the constraints above (mobile-first, step progress, sticky bottom bar, text-density tolerance, non-corporate/non-AI-hype tone).
