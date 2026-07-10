---
status: current
note: "Imported from three byte-identical download copies (-3/-4/-5); duplicates dropped."
---

# Resolve — Brand Reference Sheet
Last updated: 2 July 2026
Status: working design system, not permanent, subject to review post-design phase

---

## Colours

| Name | Hex | Usage |
|---|---|---|
| Ink | `#1A1A18` | Primary text, primary CTA buttons, focus cards, win banner background |
| Ink Soft | `#5A5A54` | Secondary text, subtitles, card labels |
| Ink Ghost | `#9A9A92` | Placeholder text, disabled states |
| Surface | `#F2EDE4` | Page/app background (warm cream) |
| Card | `#FFFFFF` | Card backgrounds |
| Border | `#DDD8CE` | Card borders, dividers |
| Amber | `#C8A040` | Active states, progress bar, accent colour, "Resolve" wordmark highlight |
| Amber Light | `#FBF5E6` | Active card backgrounds, assumed-item backgrounds |
| Terracotta | `#8B4A2A` | Eyebrow labels, section headings, card labels |
| Nav Dark | `#1E1E1A` | Navigation/header background |

---

## Typography

| Role | Font | Weight | Usage |
|---|---|---|---|
| Display | DM Serif Display | Regular | H1 and H2 only — one dominant serif element per screen |
| Body | Inter | 400, 500, 600 | Everything else — labels, body copy, buttons, captions |

**Principle:** serif = the single most important thing on the screen. Sans = everything else. Never compete with two equally-weighted display elements.

---

## Spacing & Shape

| Token | Value |
|---|---|
| Border radius (default) | `10px` |
| Border radius (small) | `6px` |
| Card padding | `16px` |
| Content padding (mobile) | `24px 20px` |
| Content padding (desktop) | `40px 48px` |

---

## Logo

TBC — not yet finalised.

---

## CSS Variables (as used in code)

```css
:root {
  --ink:        #1A1A18;
  --ink-soft:   #5A5A54;
  --ink-ghost:  #9A9A92;
  --surface:    #F2EDE4;
  --border:     #DDD8CE;
  --nav-bg:     #1E1E1A;
  --amber:      #C8A040;
  --amber-light:#FBF5E6;
  --terracotta: #8B4A2A;
  --card-bg:    #FFFFFF;
  --radius:     10px;
  --radius-sm:  6px;
}
```

---

## Tone & Voice

- Conversational, plain English, active voice
- No jargon, no ALL CAPS labels
- Sentence case + icon rather than label-only
- Direct and confident — not apologetic, not hyped
- "Resolve" not "ResolveOS" externally

---

## Key UI Principles

- **One dominant focal point per screen** — one serif heading, one clear action
- **Processing reveal** — animated ring between input and output. Emotional anchor, do not remove.
- **Win banner** — dark background, trophy, personalised. Do not remove.
- **Only show if earned** — never pad sections, never invent content to fill space
- **Mobile-first** — primary experience is phone, scales to desktop two-column
- **Consistent crossfade transitions** — soft, ~200-300ms, never harsh cuts

---

## Desktop Layout

- Max width: 960px, centered
- Left column: 280px dark sidebar (`#1A1A16`) — step indicators, progress, teaser badge
- Right column: content area
- Nav spans full width at top
