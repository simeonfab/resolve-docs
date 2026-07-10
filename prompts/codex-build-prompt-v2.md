---
status: historical
---

Repo: simeonfab/ResolveOS-intake-frontend (locate it locally, or clone if not present)

Two files exist in this repo: public/index.html (frontend) and api/resolve.js (Vercel
serverless function, multi-call OpenAI pipeline). Their current full content is pasted
at the bottom of this prompt as your starting point.

STANDING PRINCIPLES - apply throughout:
- Write modular, extensible code. Clear separation of concerns. Structure things so
  future journeys/report sections/screens can be added later without requiring a
  rewrite of what you build now.
- Do NOT touch, refactor, or "improve" anything not explicitly listed below. If you
  think something outside this list needs to change to make a listed item work
  correctly, stop and ask rather than guessing or silently expanding scope.
- If any instruction below is ambiguous or you're missing context to implement it
  correctly, ask before proceeding rather than assuming.

Implement everything below, then commit and push to main. Do not touch package.json
or DEPLOY.md.

================================================================================
REQUIRED CHANGES
================================================================================

## 1. Screen 0 - three-beat staggered reveal (currently only partially staggered)

After the user presses "Continue" from the name step, the greeting beat should reveal
in three SEPARATE sequential sub-beats, not as one fade block:
  a. "Hi [name]." appears alone first
  b. After a short pause, the explanation text ("Resolve takes a messy project and
     turns it into one clear next move" - or similar concise copy, keep it short)
     fades in beneath it
  c. After another short pause, the "Get started" button fades in beneath that

Each sub-beat should use a clean opacity fade-in transition (no abrupt pop-in). Use
similar timing to what's already used elsewhere in this screen (roughly 300-450ms per
fade, with a short stagger delay between each sub-beat - tune for a natural, unhurried
feel, not sluggish).

## 2. Sidebar entrance - simplify to fix the recurring flicker bug

The sidebar has now flickered/glitched on its entrance animation twice despite a
previous targeted fix (removing the stale 'constructing' class after the stagger
completed). Rather than continuing to patch the per-step staggered "build in one by
one" animation (4 separate timed class additions, each vulnerable to timing/race
conditions with updateNav() which runs on every screen change and rewrites each
sidebar step's className), SIMPLIFY this entirely:

Replace the per-step staggered entrance with ONE single clean fade-in for the entire
sidebar container at once (opacity 0 -> 1, one transition, no per-step stagger, no
'constructing' class, no per-step 'built' class tracking). This removes the fragile
multi-step timing logic that keeps causing this bug. The sidebar should simply not be
visible until the journey step is reached, then fade in as one unit, and from that
point on just follow the existing active/done/upcoming logic in updateNav() with no
special entrance-animation state to track or accidentally wipe.

Verify after this change that the sidebar reliably persists, correctly highlights the
active/done step, and never disappears or flickers on any subsequent screen change.

## 3. "What do you need help with" step - remove text duplication

Currently the eyebrow label and the H1 both say variations of "what do you need help
with", which reads as repeated. Change the eyebrow to a simple transition word: "So,"
Keep the H1 as the actual question: "What do you need help with?" Do not repeat the
user's name on this step (it was already used once on the previous greeting beat -
repeating it here would reintroduce the "overusing the name" problem from earlier
feedback).

## 4. Consistent smooth screen transitions everywhere (not just Screen 0)

Currently only Screen 0's internal beats have smooth fade transitions - every other
screen change (Screen 1 -> processing -> Screen 2 -> Screen 3 -> Screen 4, and back
navigation) is an instant hard cut via goToScreen()'s classList swap. Implement ONE
consistent, reusable crossfade transition used by goToScreen() for ALL screen changes
throughout the app, not just Screen 0. Should feel soft/smooth, not harsh or abrupt,
and should not noticeably slow down the flow (keep it snappy - roughly 200-300ms).
Apply this as a single shared mechanism (e.g. a small reusable transition helper)
rather than duplicating fade logic per-screen, in line with the modularity principle
above.

## 5. Screen 1 - trim to essentials

Remove the "Bring it in" eyebrow label and the "Rough is fine. Messy is expected. Just
get it out." subtitle entirely. Keep only the H1 heading and the textarea - the
existing placeholder text inside the textarea already carries sufficient guidance and
doesn't need to be duplicated above it.

## 6. Screen 2 - trim to essentials

Remove the "What Resolve understood" (or equivalent) eyebrow label above the H1. Keep
the H1 heading and the "correct anything wrong" subtitle line only - nothing else
added above the four understanding cards.

## 7. Screen 3 - trim text, remove the decision box entirely

Remove any dynamic subtitle text (e.g. "based on your answer..." / "you said you
want...") - the H1 heading should be the only text above the recommendation card, no
subtitle at all.

Remove the "One decision before you move" card/box entirely from this screen. It
currently surfaces a decision the user can't actually act on at this point in the
flow (decisions belong earlier, in the gap-answering step) - delete the card and its
markup. Leave that vertical space empty rather than filling it with something else for
now.

Also remove the underlying 'openDecision' field from the Call 2 (runRecommendationCall)
and Call 3 (QA check) system prompts and JSON schema/validation in api/resolve.js -
since it's no longer rendered anywhere, don't keep generating it. Keep the schema
validation logic structured so it's easy to re-add a field later if needed (modularity
principle).

## 8. "Output my project plan" - visible loading animation

When this card (already specified previously as a colour-accented card replacing one
of the 6 secondary action cards on Screen 4) is clicked, it must show a real, visible
loading animation INSIDE the card while the 4th API call is in flight - not just a
text change to something like "Loading...". Use a small spinner, pulsing dot(s), or
similar lightweight animation consistent with the rest of the app's visual style (the
existing processing-screen ring animation is a good reference point for tone, though
this can be a simpler, more compact version suited to living inside a card). Once the
call resolves, the card transforms into a "Download report" button as previously
specified.

## 9. Project plan report - significantly expand depth and structure

The previously specified report shape was too thin (a few short strings) compared to
the real ResolveOS example provided. Replace it with this expanded structure. This
requires restructuring the Call 4 (runProjectPlanCall) system prompt and JSON schema/
validation in api/resolve.js:

{
  "projectNarrative": "a genuine paragraph (3-5 sentences) covering what the project
    is, how it appears to have gotten to its current state, and what has actually
    been tried so far - grounded only in the confirmed understanding and gap answers,
    no invented detail",
  "currentState": "a full paragraph (not a phrase) describing where the project
    currently stands in concrete terms",
  "readiness": [
    {"area": "plain-language area name", "status": "Ready | Partially ready | Blocked | Not ready", "note": "a proper explanatory sentence, not a clipped phrase"}
    ... at least 4 rows, covering: direction/clarity, planning, readiness to act,
    validation/evidence (plain-language equivalents, no jargon)
  ],
  "whatsWorking": "a paragraph identifying genuine positives already present -
    existing assets, traction, decisions already made, clarity already achieved -
    pulled only from what's actually in the confirmed understanding, never invented",
  "whatsMissing": "a paragraph honestly identifying the real gaps and blockers,
    pulled from assumptions and unanswered context, not invented",
  "recommendedActions": [
    {"action": "short action title", "reasoning": "a short paragraph explaining why this matters now, grounded in the project specifics"}
    ... exactly 3 entries
  ],
  "doNotYet": [
    {"item": "thing not to do yet", "reason": "one sentence explaining why"}
    ... 2-3 entries
  ],
  "closingSummary": "a short paragraph (2-3 sentences) tying the report together as a
    statement, NOT ending in a question - this report is a final, immediately usable
    handoff document, not the start of another interaction"
}

Validate this shape with the same required-field / non-empty-string / array-length
pattern already used by the other three calls (readiness array length >= 4 with valid
status enum values, recommendedActions length === 3, doNotYet length >= 2), with the
same one-retry-then-clear-error behaviour already implemented elsewhere in
api/resolve.js.

Update the client-side rendering/download generation (from the previous "Output my
project plan" spec) to render ALL of these sections clearly with proper headings, in
a clean, readable downloadable file - this should read as a substantial, multi-section
document (aim for something that feels like roughly a page of real content), not a
brief summary.

================================================================================
After implementing everything above: git add -A && git commit -m "Smooth screen 0
multi-beat reveal, fix sidebar entrance flicker, trim screen copy, remove decision
box, expand project plan report depth" && git push origin main. Confirm the push
succeeded and report the commit hash. If anything in this spec was ambiguous or you
had to make an assumption to proceed, list those assumptions explicitly in your
response rather than silently deciding and moving on.
================================================================================

CURRENT FILE: public/index.html
--------------------------------------------------------------------------------
```html
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Resolve</title>
<link href="https://fonts.googleapis.com/css2?family=DM+Serif+Display&family=Inter:wght@400;500;600&display=swap" rel="stylesheet">
<style>
*, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }
:root {
  --ink: #1A1A18; --ink-soft: #5A5A54; --ink-ghost: #9A9A92;
  --surface: #F2EDE4; --border: #DDD8CE; --nav-bg: #1E1E1A;
  --amber: #C8A040; --amber-light: #FBF5E6;
  --terracotta: #8B4A2A; --card-bg: #FFFFFF;
  --radius: 10px; --radius-sm: 6px;
}
html, body {
  min-height: 100%; background: var(--surface);
  font-family: 'Inter', sans-serif; color: var(--ink);
  margin: 0;
}

/* MOBILE: full screen, single column */
.phone {
  display: flex; flex-direction: column; min-height: 100vh;
  background: var(--surface); position: relative;
}

/* DESKTOP sidebar */
.desktop-sidebar {
  display: none;
}

@media (min-width: 768px) {
  html, body { background: #0F0F0D; }

  .phone {
    display: grid;
    grid-template-columns: 280px 1fr;
    grid-template-rows: auto 1fr;
    min-height: 100vh;
    max-width: 960px;
    margin: 0 auto;
    border-radius: 0;
    box-shadow: none;
    background: var(--surface);
  }

  /* Nav spans full width on desktop */
  .nav {
    grid-column: 1 / -1;
    display: flex;
    align-items: center;
    gap: 24px;
    padding: 14px 32px;
  }

  .nav-top { margin-bottom: 0; }

  .progress-wrap { flex: 1; max-width: 400px; }

  /* Sidebar */
  .desktop-sidebar {
    display: flex;
    flex-direction: column;
    background: #1A1A16;
    padding: 32px 24px;
    min-height: calc(100vh - 56px);
    border-right: 1px solid rgba(255,255,255,0.06);
  }

  .sidebar-steps { margin-bottom: 32px; }

  .sidebar-step {
    display: flex; align-items: flex-start; gap: 12px;
    margin-bottom: 20px; opacity: 0.3; transition: opacity 0.3s;
  }
  .sidebar-step.done { opacity: 0.5; }
  .sidebar-step.active { opacity: 1; }

  /* "Constructing" entrance, triggered once on first arrival at the journey stage */
  .desktop-sidebar.constructing .sidebar-step {
    opacity: 0;
    transform: translateX(-10px);
    transition: opacity 0.45s ease, transform 0.45s ease;
  }
  .desktop-sidebar.constructing .sidebar-step.built {
    opacity: 0.3;
    transform: translateX(0);
  }
  .desktop-sidebar.constructing .sidebar-step.built.active { opacity: 1; }
  .desktop-sidebar.constructing .sidebar-badge {
    opacity: 0;
    transition: opacity 0.5s ease;
  }
  .desktop-sidebar.constructing .sidebar-badge.built { opacity: 1; }

  .step-num {
    width: 24px; height: 24px; border-radius: 50%; border: 1.5px solid rgba(255,255,255,0.2);
    display: flex; align-items: center; justify-content: center;
    font-size: 11px; font-weight: 600; color: rgba(255,255,255,0.4);
    flex-shrink: 0; margin-top: 1px;
  }
  .sidebar-step.active .step-num { border-color: var(--amber); color: var(--amber); background: rgba(200,160,64,0.1); }
  .sidebar-step.done .step-num { background: rgba(200,160,64,0.2); border-color: var(--amber); color: var(--amber); }

  .step-info {}
  .step-name { font-size: 13px; font-weight: 600; color: rgba(255,255,255,0.5); margin-bottom: 3px; }
  .sidebar-step.active .step-name { color: #FFFFFF; }
  .sidebar-step.done .step-name { color: rgba(255,255,255,0.4); }
  .step-desc { font-size: 12px; color: rgba(255,255,255,0.3); line-height: 1.4; }
  .sidebar-step.active .step-desc { color: rgba(255,255,255,0.5); }

  .sidebar-divider { height: 1px; background: rgba(255,255,255,0.06); margin: 24px 0; }

  .sidebar-badge {
    background: rgba(200,160,64,0.08); border: 1px solid rgba(200,160,64,0.2);
    border-radius: 8px; padding: 14px;
  }
  .sidebar-badge-label { font-size: 10px; font-weight: 600; letter-spacing: 0.08em; text-transform: uppercase; color: var(--amber); margin-bottom: 6px; }
  .sidebar-badge-text { font-size: 12px; color: rgba(255,255,255,0.45); line-height: 1.5; }

  /* Content area on desktop */
  .content {
    padding: 40px 48px 120px;
    overflow-y: auto;
  }

  .bottom-bar {
    position: sticky;
    grid-column: 2;
    bottom: 0;
    margin-top: auto;
  }

  /* Wider text on desktop */
  h1 { font-size: 36px; }
  h2 { font-size: 28px; }
  .subtitle { font-size: 15px; }

  .card-grid { grid-template-columns: 1fr 1fr; }
  .phase-strip { grid-template-columns: repeat(3, 1fr); }
  .action-grid { grid-template-columns: repeat(3, 1fr); }
}
.nav { background: var(--nav-bg); padding: 16px 20px; flex-shrink: 0; }
.nav-top { display: flex; align-items: center; gap: 8px; margin-bottom: 14px; }
.nav-logo { width: 24px; height: 24px; background: var(--amber); border-radius: 6px; display: flex; align-items: center; justify-content: center; }
.nav-logo svg { width: 13px; height: 13px; }
.nav-name { font-family: 'DM Serif Display', serif; font-size: 15px; color: #FFF; }
.progress-wrap { display: flex; align-items: center; gap: 10px; }
.progress-bar-outer { flex: 1; height: 3px; background: rgba(255,255,255,0.1); border-radius: 2px; overflow: hidden; }
.progress-bar-inner { height: 100%; background: var(--amber); border-radius: 2px; transition: width 0.4s ease; }
.progress-label { font-size: 11px; font-weight: 500; color: rgba(255,255,255,0.4); white-space: nowrap; flex-shrink: 0; }
.content { flex: 1; overflow-y: auto; padding: 24px 20px 100px; }
.content::-webkit-scrollbar { display: none; }
.screen { display: none; }
.screen.active { display: block; }
.eyebrow { font-size: 11px; font-weight: 600; letter-spacing: 0.08em; text-transform: uppercase; color: var(--terracotta); margin-bottom: 8px; }
h1 { font-family: 'DM Serif Display', serif; font-size: 28px; line-height: 1.2; color: var(--ink); margin-bottom: 8px; }
h2 { font-family: 'DM Serif Display', serif; font-size: 22px; line-height: 1.25; color: var(--ink); margin-bottom: 8px; }
.subtitle { font-size: 14px; color: var(--ink-soft); line-height: 1.5; margin-bottom: 24px; }
.card { background: var(--card-bg); border: 1px solid var(--border); border-radius: var(--radius); padding: 16px; margin-bottom: 10px; }
.card-label { font-size: 10px; font-weight: 600; letter-spacing: 0.08em; text-transform: uppercase; color: var(--terracotta); margin-bottom: 6px; }
.card-value { font-size: 14px; color: var(--ink); line-height: 1.5; }
.card-grid { display: grid; grid-template-columns: 1fr 1fr; gap: 10px; margin-bottom: 10px; }
textarea, input[type="text"] {
  width: 100%; background: var(--card-bg); border: 1.5px solid var(--border);
  border-radius: var(--radius); padding: 14px; font-family: 'Inter', sans-serif;
  font-size: 13px; color: var(--ink); resize: none; outline: none;
  transition: border-color 0.2s; line-height: 1.6;
}
textarea:focus, input[type="text"]:focus { border-color: var(--amber); }
textarea::placeholder, input::placeholder { color: var(--ink-ghost); }
input[type="text"] { height: 48px; font-size: 14px; }
.field { margin-bottom: 20px; }
.journey-option {
  background: var(--card-bg); border: 1.5px solid var(--border); border-radius: var(--radius);
  padding: 16px; margin-bottom: 10px; cursor: pointer; transition: border-color 0.2s, background 0.2s;
  display: flex; align-items: flex-start; gap: 14px;
}
.journey-option:hover:not(.disabled) { border-color: var(--amber); }
.journey-option.active { border-color: var(--amber); background: var(--amber-light); }
.journey-option.disabled { opacity: 0.4; cursor: not-allowed; }
.journey-icon { width: 36px; height: 36px; border-radius: 8px; background: var(--surface); display: flex; align-items: center; justify-content: center; flex-shrink: 0; font-size: 18px; }
.journey-title { font-size: 14px; font-weight: 600; color: var(--ink); margin-bottom: 3px; }
.journey-desc { font-size: 12px; color: var(--ink-soft); line-height: 1.4; }
.journey-soon { font-size: 10px; font-weight: 600; color: var(--ink-ghost); text-transform: uppercase; letter-spacing: 0.06em; margin-top: 4px; }
.confirm-row { display: flex; gap: 8px; margin-top: 16px; }
.btn-outline {
  flex: 1; padding: 10px; border: 1.5px solid var(--border); border-radius: var(--radius-sm);
  background: transparent; font-size: 13px; font-weight: 500; color: var(--ink-soft);
  cursor: pointer; text-align: center; font-family: 'Inter', sans-serif;
}
.btn-outline:hover { border-color: var(--amber); color: var(--ink); }
.gap-item { background: var(--card-bg); border: 1.5px solid var(--border); border-radius: var(--radius); padding: 16px; margin-bottom: 10px; }
.gap-question { font-size: 14px; font-weight: 500; color: var(--ink); margin-bottom: 10px; line-height: 1.4; }
.gap-input {
  width: 100%; background: var(--surface); border: 1.5px solid var(--border);
  border-radius: var(--radius-sm); padding: 10px 12px; font-family: 'Inter', sans-serif;
  font-size: 13px; color: var(--ink); outline: none; transition: border-color 0.2s;
}
.gap-input:focus { border-color: var(--amber); }
.gap-input::placeholder { color: var(--ink-ghost); }
.assumed-item {
  background: var(--amber-light); border: 1px solid #E8D5A0; border-radius: var(--radius);
  padding: 12px 16px; margin-bottom: 8px; display: flex; align-items: flex-start; gap: 10px;
}
.assumed-text { font-size: 13px; color: var(--ink-soft); line-height: 1.4; }
.focus-card { background: var(--ink); border-radius: var(--radius); padding: 20px; margin-bottom: 10px; }
.focus-label { font-size: 10px; font-weight: 600; letter-spacing: 0.08em; text-transform: uppercase; color: var(--amber); margin-bottom: 8px; }
.focus-action { font-family: 'DM Serif Display', serif; font-size: 20px; line-height: 1.25; margin-bottom: 12px; color: #FFF; }
.focus-why { font-size: 13px; color: rgba(255,255,255,0.7); line-height: 1.5; }
.phase-strip { display: grid; grid-template-columns: repeat(3, 1fr); gap: 8px; margin-bottom: 10px; }
.phase-card { background: var(--card-bg); border: 1px solid var(--border); border-radius: var(--radius-sm); padding: 12px 10px; }
.phase-label { font-size: 9px; font-weight: 600; letter-spacing: 0.08em; text-transform: uppercase; color: var(--ink-ghost); margin-bottom: 6px; }
.phase-action { font-size: 12px; font-weight: 500; color: var(--ink); line-height: 1.4; margin-bottom: 6px; }
.phase-output { font-size: 11px; color: var(--ink-soft); line-height: 1.3; font-style: italic; }
.action-card-primary {
  background: var(--ink); border-radius: var(--radius); padding: 16px 14px;
  margin-bottom: 10px; display: flex; align-items: flex-start; gap: 10px; cursor: pointer;
}
.apt-icon { font-size: 20px; flex-shrink: 0; }
.apt-eyebrow { font-size: 10px; font-weight: 600; letter-spacing: 0.08em; text-transform: uppercase; color: var(--amber); margin-bottom: 5px; }
.apt-title { font-size: 16px; font-weight: 600; color: white; line-height: 1.35; }
.resolve-mark {
  width: 32px; height: 32px; border-radius: 50%; background: var(--amber);
  display: flex; align-items: center; justify-content: center; flex-shrink: 0;
}
.resolve-mark svg { width: 16px; height: 16px; }
.action-grid { display: grid; grid-template-columns: 1fr 1fr; gap: 8px; margin-bottom: 16px; }
.action-card {
  background: var(--card-bg); border: 1.5px solid var(--border); border-radius: var(--radius);
  padding: 14px 12px; cursor: pointer; transition: border-color 0.2s, background 0.2s;
  display: flex; align-items: flex-start; gap: 8px;
}
.action-card:hover { border-color: var(--amber); background: var(--amber-light); }
.action-icon { font-size: 16px; flex-shrink: 0; }
.action-text { font-size: 13px; font-weight: 500; color: var(--ink); line-height: 1.35; }
.bottom-bar {
  position: sticky; bottom: 0; left: 0; right: 0; background: var(--surface);
  border-top: 1px solid var(--border); padding: 12px 20px 20px;
  display: flex; align-items: center; gap: 10px;
  z-index: 10;
}
.btn-back {
  padding: 12px 16px; border: 1.5px solid var(--border); border-radius: var(--radius-sm);
  background: transparent; font-size: 14px; font-weight: 500; color: var(--ink-soft);
  cursor: pointer; font-family: 'Inter', sans-serif; white-space: nowrap;
}
.btn-primary {
  flex: 1; padding: 13px 16px; background: var(--ink); color: white; border: none;
  border-radius: var(--radius-sm); font-family: 'Inter', sans-serif; font-size: 14px;
  font-weight: 600; cursor: pointer; transition: background 0.2s; text-align: center;
}
.btn-primary:hover { background: #2E2E2A; }
.divider { height: 1px; background: var(--border); margin: 20px 0; }
.section-title { font-size: 13px; font-weight: 600; color: var(--ink-soft); margin-bottom: 10px; display: flex; align-items: center; gap: 6px; }
.collapse-toggle {
  background: none; border: none; font-family: 'Inter', sans-serif;
  font-size: 13px; color: var(--terracotta); font-weight: 500; cursor: pointer;
  padding: 0; margin-bottom: 16px;
}
.collapsed { display: none; }
.import-tag {
  display: inline-flex; align-items: center; gap: 6px;
  background: #F0EBE1; border: 1px solid #D4C9B4; color: var(--terracotta);
  font-size: 11px; font-weight: 600; padding: 5px 10px; border-radius: 6px;
  margin-bottom: 12px; letter-spacing: 0.04em;
}
.import-dot { width: 6px; height: 6px; background: var(--amber); border-radius: 50%; }
.clearbase-output {
  background: #FAF7F2; border: 1px solid var(--border); border-left: 3px solid var(--amber);
  border-radius: var(--radius); padding: 16px; margin-bottom: 8px;
  font-family: 'Inter', sans-serif; font-size: 12px; color: var(--ink-soft); line-height: 1.7;
}
.cb-heading { color: var(--terracotta); font-weight: 600; font-size: 10px; letter-spacing: 0.08em; text-transform: uppercase; margin-bottom: 5px; margin-top: 14px; }
.cb-heading:first-child { margin-top: 0; }
.cb-value { color: var(--ink); font-size: 13px; }
.cb-tag { display: inline-block; background: var(--amber-light); color: var(--terracotta); font-size: 10px; padding: 2px 7px; border-radius: 4px; font-weight: 600; margin-right: 4px; margin-bottom: 3px; border: 1px solid #E8D5A0; }

/* inline highlight for dynamic data */
.data-ref { color: var(--terracotta); font-weight: 600; }

.screen0-step {
  opacity: 1;
  transition: opacity 0.4s ease;
}
.screen0-step.screen0-step-hidden {
  display: none;
}
.screen0-pure {
  display: flex;
  flex-direction: column;
  align-items: flex-start;
  justify-content: center;
  min-height: 60vh;
}
.screen0-pure h1 { font-size: 34px; }
.inline-affirm-btn {
  margin-top: 18px;
  background: var(--ink);
  color: white;
  border: none;
  border-radius: var(--radius-sm);
  padding: 11px 22px;
  font-family: 'Inter', sans-serif;
  font-size: 14px;
  font-weight: 600;
  cursor: pointer;
  transition: background 0.2s, opacity 0.3s;
}
.inline-affirm-btn:hover { background: #2E2E2A; }
.inline-affirm-btn.screen0-step-hidden { display: none; }

/* mic + textarea */
.textarea-wrap { position: relative; }
.mic-btn {
  position: absolute;
  bottom: 12px;
  right: 12px;
  width: 34px;
  height: 34px;
  border-radius: 50%;
  border: 1.5px solid var(--border);
  background: var(--card-bg);
  font-size: 14px;
  cursor: pointer;
  display: flex;
  align-items: center;
  justify-content: center;
  transition: border-color 0.2s, background 0.2s;
}
.mic-btn:hover { border-color: var(--amber); }
.mic-btn.recording {
  background: var(--terracotta);
  border-color: var(--terracotta);
  animation: pulse-mic 1.2s infinite;
}
@keyframes pulse-mic {
  0%, 100% { box-shadow: 0 0 0 0 rgba(139,74,42,0.4); }
  50% { box-shadow: 0 0 0 8px rgba(139,74,42,0); }
}

/* Screen 1 utility row - minimal, icon-led */
.s1-utility-row {
  display: flex;
  gap: 8px;
  margin: 10px 0 16px;
}
.utility-icon-btn {
  background: transparent;
  border: 1px solid var(--border);
  border-radius: var(--radius-sm);
  padding: 7px 12px;
  font-family: 'Inter', sans-serif;
  font-size: 12px;
  font-weight: 500;
  color: var(--ink-soft);
  cursor: pointer;
  transition: border-color 0.2s, color 0.2s;
}
.utility-icon-btn:hover { border-color: var(--amber); color: var(--ink); }

/* PROCESSING SCREEN */
.processing-ring {
  width: 80px; height: 80px; margin-bottom: 28px;
}
.processing-ring svg { width: 80px; height: 80px; }
#progress-circle {
  transform-origin: center;
  transform: rotate(-90deg);
  transition: stroke-dashoffset 1.4s cubic-bezier(0.4,0,0.2,1);
}
.processing-label {
  font-family: 'DM Serif Display', serif; font-size: 22px;
  color: var(--ink); margin-bottom: 10px; line-height: 1.3;
}
.processing-sub {
  font-size: 14px; color: var(--ink-soft); line-height: 1.5; max-width: 260px;
}

/* WIN BANNER */
.win-banner {
  background: var(--ink); border-radius: var(--radius);
  padding: 24px 20px; text-align: center; margin-bottom: 8px;
}
.win-icon { font-size: 32px; margin-bottom: 12px; }
.win-title {
  font-family: 'DM Serif Display', serif; font-size: 22px;
  color: #FFFFFF; margin-bottom: 8px; line-height: 1.25;
}
.win-desc { font-size: 13px; color: rgba(255,255,255,0.65); line-height: 1.6; }
</style>
</head>
<body>
<div class="phone">
  <div class="nav">
    <div class="nav-top">
      <div class="nav-logo">
        <svg viewBox="0 0 14 14" fill="none"><path d="M2 7L6 11L12 3" stroke="white" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"/></svg>
      </div>
      <span class="nav-name">Resolve</span>
    </div>
    <div class="progress-wrap">
      <div class="progress-bar-outer"><div class="progress-bar-inner" id="progress-bar" style="width:0%"></div></div>
      <span class="progress-label" id="progress-label">Step 1 of 4</span>
    </div>
  </div>

  <div class="desktop-sidebar" id="desktop-sidebar">
    <div class="sidebar-steps">
      <div class="sidebar-step active" id="ss-0">
        <div class="step-num">1</div>
        <div class="step-info">
          <div class="step-name">Get started</div>
          <div class="step-desc">Tell Resolve your name and what you need help with</div>
        </div>
      </div>
      <div class="sidebar-step" id="ss-1">
        <div class="step-num">2</div>
        <div class="step-info">
          <div class="step-name">Bring it in</div>
          <div class="step-desc">Share your project context — rough notes, imports, anything you have</div>
        </div>
      </div>
      <div class="sidebar-step" id="ss-2">
        <div class="step-num">3</div>
        <div class="step-info">
          <div class="step-name">Confirm understanding</div>
          <div class="step-desc">Check what Resolve understood and fill any gaps</div>
        </div>
      </div>
      <div class="sidebar-step" id="ss-3">
        <div class="step-num">4</div>
        <div class="step-info">
          <div class="step-name">Your next move</div>
          <div class="step-desc">One clear recommended action and a roadmap built from your project</div>
        </div>
      </div>
    </div>

    <div class="sidebar-divider"></div>

    <div class="sidebar-badge">
      <div class="sidebar-badge-label">🔒 After this session</div>
      <div class="sidebar-badge-text">Unlock the ability to save your path, track progress, and return to your project any time.</div>
    </div>
  </div>

  <div class="content">

    <!-- S0 -->
    <div class="screen active" id="screen-0">
      <div id="name-step" class="screen0-step screen0-pure">
        <div class="eyebrow" id="s0-eyebrow">Let's get started</div>
        <h1 id="s0-h1">What's your name?</h1>
        <div class="field" style="margin-top:24px; max-width:340px;">
          <input type="text" id="name-input" placeholder="Your first name" onkeydown="if(event.key==='Enter'){event.preventDefault(); revealGreetingStep();}" />
        </div>
        <button class="inline-affirm-btn" onclick="revealGreetingStep()">Continue →</button>
      </div>

      <div id="greeting-step" class="screen0-step screen0-pure screen0-step-hidden">
        <h1 id="greeting-h1">Hi.</h1>
        <p class="subtitle" id="greeting-sub" style="margin-bottom:28px;">Resolve takes a messy project and turns it into one clear next move — no jargon, no busywork.</p>
        <button class="inline-affirm-btn screen0-step-hidden" id="get-started-btn" onclick="revealJourneyStep()">Get started →</button>
      </div>

      <div id="journey-step" class="screen0-step screen0-step-hidden">
        <div class="eyebrow" id="journey-greeting">What do you need help with?</div>
        <h1 id="journey-h1">What do you need help with?</h1>
        <div class="journey-option active" onclick="selectJourneyAndGo(this)">
          <div class="journey-icon">🗂️</div>
          <div>
            <div class="journey-title">I have a messy project</div>
            <div class="journey-desc">Notes everywhere, old AI chats, no clear next step. Help me make sense of it.</div>
          </div>
        </div>
        <div class="journey-option disabled">
          <div class="journey-icon">💡</div>
          <div>
            <div class="journey-title">I have a new idea</div>
            <div class="journey-desc">I want to shape it into something real.</div>
            <div class="journey-soon">Coming soon</div>
          </div>
        </div>
        <div class="journey-option disabled">
          <div class="journey-icon">🤔</div>
          <div>
            <div class="journey-title">I'm stuck, not sure where to start</div>
            <div class="journey-desc">Help me figure out what the actual problem is.</div>
            <div class="journey-soon">Coming soon</div>
          </div>
        </div>
      </div>
    </div>

    <!-- S1 -->
    <div class="screen" id="screen-1">
      <div class="eyebrow">Bring it in</div>
      <h1>Tell Resolve what's going on.</h1>
      <p class="subtitle">Rough is fine. Messy is expected. Just get it out.</p>

      <div class="field">
        <div class="textarea-wrap">
          <textarea id="project-input" placeholder="Describe the project however you like. What exists, what's confusing, what you're trying to decide, what's blocking you. Don't tidy it up." style="height:220px;"></textarea>
          <button class="mic-btn" id="mic-btn" onclick="toggleVoiceInput()" title="Voice note">🎙️</button>
        </div>
        <span class="input-hint" id="mic-status"></span>
      </div>

      <div class="s1-utility-row">
        <button class="utility-icon-btn" onclick="toggleOptional()" title="Add supporting material">📎 Add material</button>
        <button class="utility-icon-btn" onclick="loadExample()" title="Load an example project">✨ Try an example</button>
      </div>
      <div id="optional-section" class="collapsed">
        <div class="field" style="margin-top:8px;">
          <textarea id="supporting-material" placeholder="Links, copied notes, screenshots described in text, old AI chat summaries, anything else worth including." style="height:100px;"></textarea>
        </div>
      </div>
    </div>

    <!-- PROCESSING (between S1 and S2) -->
    <div class="screen" id="screen-processing">
      <div style="display:flex; flex-direction:column; align-items:center; justify-content:center; min-height:400px; text-align:center; padding: 0 20px;">
        <div class="processing-ring" id="processing-ring">
          <svg viewBox="0 0 60 60" fill="none" xmlns="http://www.w3.org/2000/svg">
            <circle cx="30" cy="30" r="26" stroke="#DDD8CE" stroke-width="3"/>
            <circle cx="30" cy="30" r="26" stroke="#C8A040" stroke-width="3" stroke-dasharray="163" stroke-dashoffset="163" id="progress-circle" stroke-linecap="round"/>
            <path d="M20 30L27 37L40 23" stroke="#C8A040" stroke-width="2.5" stroke-linecap="round" stroke-linejoin="round" opacity="0" id="check-mark"/>
          </svg>
        </div>
        <p class="processing-label" id="processing-label">Reading your project…</p>
        <p class="processing-sub" id="processing-sub">Resolve is making sense of what you shared</p>
      </div>
    </div>

    <!-- S2 -->
    <div class="screen" id="screen-2">
      <div class="eyebrow">What Resolve understood</div>
      <h2 id="s2-h">Here's what Resolve got from that.</h2>
      <p class="subtitle">Correct anything wrong — it shapes everything that follows.</p>
      <div class="card-grid">
        <div class="card">
          <div class="card-label">📁 Project</div>
          <div class="card-value" id="s2-project">—</div>
        </div>
        <div class="card">
          <div class="card-label">🎯 What you want</div>
          <div class="card-value" id="s2-goal">—</div>
        </div>
        <div class="card">
          <div class="card-label">📍 Where it is</div>
          <div class="card-value" id="s2-state">—</div>
        </div>
        <div class="card">
          <div class="card-label">❓ Biggest uncertainty</div>
          <div class="card-value" id="s2-uncertainty">—</div>
        </div>
      </div>
      <div class="confirm-row">
        <button class="btn-outline" style="flex:none; padding:10px 20px;">✏️ Edit</button>
      </div>
      <div class="divider"></div>
      <div class="section-title" id="s2-gap-title">Two things Resolve still needs</div>
      <div class="gap-item">
        <div class="gap-question" id="s2-gap1-q">What would make this session a success for you?</div>
        <input class="gap-input" type="text" id="gap1" placeholder="e.g. know my next step, feel unblocked, have something to show someone" />
      </div>
      <div class="gap-item">
        <div class="gap-question" id="s2-gap2-q">Any deadline or pressure driving this right now?</div>
        <input class="gap-input" type="text" id="gap2" placeholder="e.g. want to launch before summer, no real deadline" />
      </div>
      <div class="divider"></div>
      <div class="section-title">What Resolve is assuming</div>
      <div id="s2-assumptions">
        <div class="assumed-item"><span style="font-size:14px;">✓</span><span class="assumed-text">—</span></div>
      </div>
    </div>

    <!-- S3 -->
    <div class="screen" id="screen-3">
      <div class="eyebrow">Your next move</div>
      <h2 id="s3-h">Here's what to do next.</h2>
      <div class="focus-card" style="margin-top:20px;">
        <div class="focus-label">🎯 Do this first</div>
        <div class="focus-action" id="s3-action">—</div>
        <div class="focus-why" id="s3-why">—</div>
      </div>
      <div class="card" style="margin-bottom:10px;">
        <div class="card-label">🏁 First milestone</div>
        <div class="card-value" id="s3-milestone">—</div>
      </div>
      <div class="divider"></div>
      <div class="section-title">What happens after this</div>
      <div class="phase-strip" id="s3-phase-strip">
        <div class="phase-card" style="border-top:3px solid var(--amber);">
          <div class="phase-label" id="p1-label">Now</div>
          <div class="phase-action" id="p1-action">—</div>
          <div class="phase-output" id="p1-output">—</div>
        </div>
        <div class="phase-card" style="border-top:3px solid var(--terracotta);">
          <div class="phase-label" id="p2-label">Define</div>
          <div class="phase-action" id="p2-action">—</div>
          <div class="phase-output" id="p2-output">—</div>
        </div>
        <div class="phase-card" style="border-top:3px solid var(--border);">
          <div class="phase-label" id="p3-label">Launch</div>
          <div class="phase-action" id="p3-action">—</div>
          <div class="phase-output" id="p3-output">—</div>
        </div>
      </div>
      <div class="card">
        <div class="card-label">⚠️ One decision before you move</div>
        <div class="card-value" style="font-size:13px; color:var(--ink-soft);" id="s3-decision">—</div>
      </div>
    </div>

    <!-- S4 -->
    <div class="screen" id="screen-4">
      <div class="win-banner">
        <div class="win-icon">🏆</div>
        <div class="win-title" id="s4-win-title">You just cleared the fog.</div>
        <div class="win-desc" id="s4-win-desc">A messy rebrand with no clear starting point — turned into a focused path forward in under 10 minutes. That's the hard part done.</div>
      </div>
      <div class="eyebrow" style="margin-top:20px;">Next steps</div>
      <div class="action-card-primary">
        <div class="resolve-mark">
          <svg viewBox="0 0 24 24" fill="none"><path d="M4 13L10 19L20 6" stroke="white" stroke-width="2.5" stroke-linecap="round" stroke-linejoin="round"/></svg>
        </div>
        <div>
          <div class="apt-eyebrow">Resolve recommends</div>
          <div class="apt-title">Shortlist to one name and test it this week</div>
        </div>
      </div>
      <div class="action-grid">
        <div class="action-card"><span class="action-icon">📣</span><span class="action-text">Plan how to tell existing customers about the rebrand</span></div>
        <div class="action-card"><span class="action-icon">🎨</span><span class="action-text">Build a brief for the visual identity</span></div>
        <div class="action-card"><span class="action-icon">🗓️</span><span class="action-text">Map the 8-week relaunch timeline</span></div>
        <div class="action-card"><span class="action-icon">📦</span><span class="action-text">Prioritise what gets updated first — site, packaging, or social</span></div>
        <div class="action-card"><span class="action-icon">📱</span><span class="action-text">Plan the Instagram transition for 8k followers</span></div>
        <div class="action-card"><span class="action-icon">🔖</span><span class="action-text">Save this path and come back</span></div>
      </div>
      <div class="field">
        <textarea placeholder="Or ask Resolve something specific about this project…" style="height:72px;"></textarea>
      </div>
    </div>

  </div>

  <div class="bottom-bar" id="bottom-bar">
    <button class="btn-back" onclick="goBack()" id="back-btn" style="display:none;">Back</button>
    <button class="btn-primary" onclick="goNext()" id="next-btn">Get started →</button>
  </div>
</div>

<script>
let current = 0, userName = '';
let understandingData = null; // result from Call 1
let recommendationData = null; // result from Call 2 (post QA-check)
let pendingCall = 'understand'; // tracks which call the processing screen should retry

const total = 6;
const screenIds = ['screen-0','screen-1','screen-processing','screen-2','screen-3','screen-4'];
const nextLabels = ['Get started \u2192','Build my path \u2192','','Show my next move \u2192','See what to do next \u2192','Done'];
const progressPct = [0, 20, 20, 50, 75, 100];
const stepLabels = ['Step 1 of 4','Step 2 of 4','Step 2 of 4','Step 3 of 4','Step 4 of 4','Complete'];

const EXAMPLE_PROJECT = `I run a small e-commerce business called Nook & Thread, handmade homewares, been trading for about 4 years. Sales have plateaued and I think the brand feels too homespun for the price point I want to move to. I'm trying to rebrand to something more premium.

I've got three different name shortlists but haven't committed to any of them. There's a logo draft (v2) but I haven't tested it with anyone. I have about 1,400 existing customers and an Instagram with 8k followers. I don't have a plan for how to migrate existing customers over or communicate the change to them. Website, packaging, and social all need updating but I don't know what order to do things in.

I want to relaunch within about 8 weeks, before peak gifting season starts.`;

function loadExample() {
  document.getElementById('project-input').value = EXAMPLE_PROJECT;
}

function updateNav() {
  document.getElementById('bottom-bar').style.display = (current === 0) ? 'none' : 'flex';
  document.getElementById('next-btn').textContent = nextLabels[current];
  document.getElementById('next-btn').style.display = (current === 2) ? 'none' : 'flex';
  document.getElementById('back-btn').style.display = current > 0 && current !== 2 ? 'block' : 'none';

  // Processing screen (index 2) is shared by two different transitions.
  // Show the correct climbing progress depending on which call is pending,
  // rather than always resetting to the Call-1 value.
  let pct, label;
  if (current === 2) {
    if (pendingCall === 'recommend') {
      pct = 62; label = 'Step 3 of 4';
    } else {
      pct = 35; label = 'Step 2 of 4';
    }
  } else {
    pct = progressPct[current];
    label = stepLabels[current];
  }
  document.getElementById('progress-bar').style.width = pct + '%';
  document.getElementById('progress-label').textContent = label;

  const sidebarMap = [0, 1, 1, 2, 3, 3];
  const activeSidebar = (current === 2 && pendingCall === 'recommend') ? 2 : sidebarMap[current];
  for (let i = 0; i < 4; i++) {
    const el = document.getElementById('ss-' + i);
    if (!el) continue;
    el.className = 'sidebar-step' + (i < activeSidebar ? ' done' : i === activeSidebar ? ' active' : '');
  }
}

function setProcessingText(label, sub) {
  document.getElementById('processing-label').textContent = label;
  document.getElementById('processing-sub').textContent = sub;
}

function resetProcessingRing() {
  const circle = document.getElementById('progress-circle');
  const check = document.getElementById('check-mark');
  circle.style.transition = 'none';
  circle.style.strokeDashoffset = '163';
  check.style.opacity = '0';
  // force reflow so the next transition actually animates
  void circle.offsetWidth;
  circle.style.transition = 'stroke-dashoffset 1.4s cubic-bezier(0.4,0,0.2,1)';
}

function animateRingTo(value) {
  document.getElementById('progress-circle').style.strokeDashoffset = String(value);
}

function showCheck() {
  const check = document.getElementById('check-mark');
  check.style.transition = 'opacity 0.3s ease';
  check.style.opacity = '1';
}

function showProcessingError(message) {
  setProcessingText('Something went wrong', message);
  document.getElementById('next-btn').style.display = 'flex';
  document.getElementById('next-btn').textContent = 'Try again';
  document.getElementById('back-btn').style.display = 'block';
}

function goToScreen(idx) {
  document.getElementById(screenIds[current]).classList.remove('active');
  current = idx;
  document.getElementById(screenIds[current]).classList.add('active');
  document.querySelector('.content').scrollTop = 0;
  updateNav();
}

// ---------------------------------------------------------------------------
// CALL 1: Business Analyst understanding extraction (S1 -> processing -> S2)
// ---------------------------------------------------------------------------
async function runUnderstandingCall() {
  pendingCall = 'understand';
  resetProcessingRing();
  setProcessingText('Reading your project\u2026', 'Resolve is making sense of what you shared');
  animateRingTo(163);

  const projectInput = document.getElementById('project-input').value.trim();

  if (!projectInput) {
    showProcessingError('Add a few sentences about your project before continuing.');
    return;
  }

  setTimeout(() => animateRingTo(80), 200);

  try {
    const res = await fetch('/api/resolve', {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify({ stage: 'understand', projectInput })
    });

    if (!res.ok) {
      const errBody = await res.json().catch(() => ({}));
      throw new Error(errBody.error || 'Request failed');
    }

    understandingData = await res.json();

    animateRingTo(0);
    showCheck();
    setProcessingText('Path found.', "Here's what Resolve understood");

    populateScreen2(understandingData);

    setTimeout(() => goToScreen(3), 700);
  } catch (err) {
    console.error(err);
    showProcessingError(err.message || 'Resolve could not read this project. Try again or simplify your input.');
  }
}

function populateScreen2(d) {
  document.getElementById('s2-project').textContent = d.project || '\u2014';
  document.getElementById('s2-goal').textContent = d.goal || '\u2014';
  document.getElementById('s2-state').textContent = d.state || '\u2014';
  document.getElementById('s2-uncertainty').textContent = d.uncertainty || '\u2014';

  if (Array.isArray(d.gapQuestions) && d.gapQuestions.length >= 2) {
    document.getElementById('s2-gap1-q').textContent = d.gapQuestions[0];
    document.getElementById('s2-gap2-q').textContent = d.gapQuestions[1];
  }

  const assumptionsEl = document.getElementById('s2-assumptions');
  assumptionsEl.innerHTML = '';
  const assumptions = Array.isArray(d.assumptions) && d.assumptions.length ? d.assumptions : ['No explicit assumptions were needed \u2014 the input was specific enough.'];
  assumptions.forEach(a => {
    const div = document.createElement('div');
    div.className = 'assumed-item';
    div.innerHTML = `<span style="font-size:14px;">\u2713</span><span class="assumed-text"></span>`;
    div.querySelector('.assumed-text').textContent = a;
    assumptionsEl.appendChild(div);
  });
}

// ---------------------------------------------------------------------------
// CALL 2 + 3: Strategic Product Director recommendation + QA self-check
// (S2 -> processing -> S3)
// ---------------------------------------------------------------------------
async function runRecommendationCall() {
  pendingCall = 'recommend';
  resetProcessingRing();
  setProcessingText('Working out your next move\u2026', 'Weighing this against everything you confirmed');
  animateRingTo(163);

  const gapAnswers = {
    successDefinition: document.getElementById('gap1').value.trim(),
    deadline: document.getElementById('gap2').value.trim()
  };

  setTimeout(() => animateRingTo(80), 200);

  try {
    const res = await fetch('/api/resolve', {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify({
        stage: 'recommend',
        confirmedUnderstanding: understandingData,
        gapAnswers
      })
    });

    if (!res.ok) {
      const errBody = await res.json().catch(() => ({}));
      throw new Error(errBody.error || 'Request failed');
    }

    recommendationData = await res.json();

    animateRingTo(0);
    showCheck();
    setProcessingText('Path found.', 'Here\'s what to do next');

    populateScreen3(recommendationData, gapAnswers);
    populateScreen4(recommendationData);

    setTimeout(() => goToScreen(4), 700);
  } catch (err) {
    console.error(err);
    showProcessingError(err.message || 'Resolve could not finish your recommendation. Try again.');
  }
}

function populateScreen3(d, gapAnswers) {
  document.getElementById('s3-action').textContent = d.recommendedAction || '\u2014';
  document.getElementById('s3-why').textContent = d.why || '\u2014';
  document.getElementById('s3-milestone').textContent = d.milestone || '\u2014';
  document.getElementById('s3-decision').textContent = d.openDecision || '\u2014';

  if (gapAnswers.successDefinition) {
    document.getElementById('s3-sub').textContent = `You said you want: "${gapAnswers.successDefinition}". Here's the clearest path to that.`;
  }

  if (Array.isArray(d.roadmap) && d.roadmap.length >= 3) {
    const ids = ['p1', 'p2', 'p3'];
    d.roadmap.slice(0, 3).forEach((phase, i) => {
      document.getElementById(ids[i] + '-label').textContent = phase.phase || '\u2014';
      document.getElementById(ids[i] + '-action').textContent = phase.action || '\u2014';
      document.getElementById(ids[i] + '-output').textContent = phase.output ? `Output: ${phase.output}` : '\u2014';
    });
  }
}

function populateScreen4(d) {
  const titleEl = document.querySelector('#screen-4 .apt-title');
  if (titleEl) titleEl.textContent = d.recommendedAction || 'Your recommended next move';
}

// ---------------------------------------------------------------------------
// Navigation
// ---------------------------------------------------------------------------
function goNext() {
  if (current === 0) {
    if (screen0Stage === 'name') {
      revealGreetingStep();
      return;
    }
    if (screen0Stage === 'greeting') {
      revealJourneyStep();
      return;
    }
    // journey stage: handled by selectJourneyAndGo() directly on click,
    // bottom bar has no separate action here
    return;
  }

  if (current === 1) {
    document.getElementById(screenIds[current]).classList.remove('active');
    current = 2;
    document.getElementById(screenIds[current]).classList.add('active');
    document.querySelector('.content').scrollTop = 0;
    updateNav();
    runUnderstandingCall();
    return;
  }

  if (current === 2) {
    // processing screen retry path - retry whichever call was pending
    if (pendingCall === 'recommend') {
      runRecommendationCall();
    } else {
      runUnderstandingCall();
    }
    return;
  }

  if (current === 3) {
    // Leaving Screen 2 -> trigger call 2+3, route through processing again
    document.getElementById(screenIds[current]).classList.remove('active');
    current = 2;
    document.getElementById(screenIds[current]).classList.add('active');
    document.querySelector('.content').scrollTop = 0;
    updateNav();
    runRecommendationCall();
    return;
  }

  if (current < total - 1) {
    goToScreen(current + 1);
  }
}

function goBack() {
  if (current > 0) {
    const prev = current === 3 ? 1 : current === 4 ? 3 : current - 1;
    goToScreen(prev);
  }
}

let screen0Stage = 'name'; // 'name' | 'greeting' | 'journey'

// Beat 1 -> Beat 2: name entered, fade to greeting + explanation
function revealGreetingStep() {
  const n = document.getElementById('name-input').value.trim();
  if (!n) {
    document.getElementById('name-input').focus();
    document.getElementById('name-input').style.borderColor = '#C0392B';
    return;
  }
  userName = n;

  const nameStep = document.getElementById('name-step');
  const greetingStep = document.getElementById('greeting-step');

  nameStep.style.opacity = '0';

  setTimeout(() => {
    nameStep.classList.add('screen0-step-hidden');
    greetingStep.classList.remove('screen0-step-hidden');
    greetingStep.style.opacity = '0';
    void greetingStep.offsetWidth;
    document.getElementById('greeting-h1').textContent = `Hi ${n}.`;
    greetingStep.style.opacity = '1';

    setTimeout(() => {
      document.getElementById('get-started-btn').classList.remove('screen0-step-hidden');
    }, 450);
  }, 400);

  screen0Stage = 'greeting';
  document.getElementById('s4-win-title').textContent = `You just cleared the fog, ${n}.`;
}

// Beat 2 -> Beat 3: greeting confirmed, fade to journey selection
function revealJourneyStep() {
  const greetingStep = document.getElementById('greeting-step');
  const journeyStep = document.getElementById('journey-step');

  greetingStep.style.opacity = '0';

  setTimeout(() => {
    greetingStep.classList.add('screen0-step-hidden');
    journeyStep.classList.remove('screen0-step-hidden');
    journeyStep.style.opacity = '0';
    void journeyStep.offsetWidth;
    document.getElementById('journey-greeting').textContent = `Hi ${userName}, what do you need help with?`;
    journeyStep.style.opacity = '1';
  }, 400);

  screen0Stage = 'journey';

  // Desktop sidebar "constructing" stagger - steps build in one by one,
  // then the .constructing class is removed so steady-state opacity rules
  // (driven by updateNav's active/done classes) take over cleanly.
  const sidebar = document.getElementById('desktop-sidebar');
  if (sidebar) {
    sidebar.classList.add('constructing');
    for (let i = 0; i < 4; i++) {
      setTimeout(() => {
        const el = document.getElementById('ss-' + i);
        if (el) el.classList.add('built');
      }, 500 + i * 140);
    }
    setTimeout(() => {
      const badge = sidebar.querySelector('.sidebar-badge');
      if (badge) badge.classList.add('built');
      sidebar.classList.remove('constructing');
    }, 500 + 4 * 140 + 250);
  }
}

// Clicking a journey option navigates immediately - no separate button needed
function selectJourneyAndGo(el) {
  if (el.classList.contains('disabled')) return;
  document.querySelectorAll('.journey-option:not(.disabled)').forEach(j => j.classList.remove('active'));
  el.classList.add('active');
  setTimeout(() => goToScreen(1), 150);
}

function toggleOptional() {
  const s = document.getElementById('optional-section');
  if (s.classList.contains('collapsed')) {
    s.classList.remove('collapsed');
  } else {
    s.classList.add('collapsed');
  }
}

// ---------------------------------------------------------------------------
// Voice note transcription (Web Speech API)
// ---------------------------------------------------------------------------
let recognition = null;
let isRecording = false;

function toggleVoiceInput() {
  const SpeechRecognition = window.SpeechRecognition || window.webkitSpeechRecognition;
  const micBtn = document.getElementById('mic-btn');
  const statusEl = document.getElementById('mic-status');

  if (!SpeechRecognition) {
    statusEl.textContent = 'Voice input is not supported in this browser - try Chrome on desktop or Android.';
    return;
  }

  if (isRecording) {
    recognition.stop();
    return;
  }

  recognition = new SpeechRecognition();
  recognition.continuous = true;
  recognition.interimResults = true;
  recognition.lang = 'en-GB';

  const textarea = document.getElementById('project-input');
  const baseText = textarea.value;
  let finalTranscript = '';

  recognition.onstart = () => {
    isRecording = true;
    micBtn.classList.add('recording');
    statusEl.textContent = 'Listening... tap the mic again to stop.';
  };

  recognition.onresult = (event) => {
    let interim = '';
    for (let i = event.resultIndex; i < event.results.length; i++) {
      const transcript = event.results[i][0].transcript;
      if (event.results[i].isFinal) {
        finalTranscript += transcript + ' ';
      } else {
        interim += transcript;
      }
    }
    textarea.value = (baseText + ' ' + finalTranscript + interim).trim();
  };

  recognition.onerror = (event) => {
    statusEl.textContent = 'Voice input error: ' + event.error + '. Try again or type instead.';
    isRecording = false;
    micBtn.classList.remove('recording');
  };

  recognition.onend = () => {
    isRecording = false;
    micBtn.classList.remove('recording');
    statusEl.textContent = finalTranscript ? 'Transcribed. Feel free to edit before continuing.' : '';
  };

  recognition.start();
}

updateNav();
</script>

</body>
</html>

```

CURRENT FILE: api/resolve.js
--------------------------------------------------------------------------------
```javascript
// /api/resolve.js
// Vercel serverless function. Holds the OpenAI API key server-side.
// Implements a 3-call pipeline grounded in the actual ResolveOS role/governance files:
//   Call 1: Business Analyst   -> understanding extraction (S1 -> S2)
//   Call 2: Strategic Product Director -> recommendation + roadmap (S2 -> S3)
//   Call 3: QA self-check      -> validates Call 2 against Call 1, regenerates if it invents facts

export default async function handler(req, res) {
  if (req.method !== 'POST') {
    return res.status(405).json({ error: 'Method not allowed' });
  }

  const { stage, projectInput, confirmedUnderstanding, gapAnswers } = req.body;

  const apiKey = process.env.OPENAI_API_KEY;
  if (!apiKey) {
    return res.status(500).json({ error: 'Server misconfigured: missing API key' });
  }

  try {
    if (stage === 'understand') {
      const result = await runUnderstandingCall(projectInput, apiKey);
      return res.status(200).json(result);
    }

    if (stage === 'recommend') {
      const draft = await runRecommendationCall(confirmedUnderstanding, gapAnswers, apiKey);
      const checked = await runQACheckCall(confirmedUnderstanding, gapAnswers, draft, apiKey);
      return res.status(200).json(checked);
    }

    return res.status(400).json({ error: 'Unknown stage' });
  } catch (err) {
    console.error('Resolve API error:', err);
    const isShapeError = err.name === 'ResolveShapeError';
    return res.status(isShapeError ? 502 : 500).json({
      error: isShapeError
        ? "Resolve's response didn't come back in the right shape. This is usually temporary — try again."
        : 'Resolve could not process this right now. Try again, or simplify your input.',
      detail: process.env.NODE_ENV === 'development' ? String(err) : undefined,
    });
  }
}

// ---------------------------------------------------------------------------
// SCHEMA VALIDATION
// Guards against malformed/incomplete JSON from the model before it ever
// reaches the frontend. If a call fails validation, it retries once with a
// stricter instruction before giving up and surfacing a clear error.
// ---------------------------------------------------------------------------
class ResolveShapeError extends Error {
  constructor(message) {
    super(message);
    this.name = 'ResolveShapeError';
  }
}

function isNonEmptyString(v) {
  return typeof v === 'string' && v.trim().length > 0;
}

function validateUnderstandingShape(data) {
  const required = ['project', 'goal', 'state', 'uncertainty', 'gapQuestions', 'assumptions'];
  for (const key of required) {
    if (!(key in data)) {
      throw new ResolveShapeError(`Understanding response missing required field: ${key}`);
    }
  }
  for (const key of ['project', 'goal', 'state', 'uncertainty']) {
    if (!isNonEmptyString(data[key])) {
      throw new ResolveShapeError(`Understanding field "${key}" is empty or not a string`);
    }
  }
  if (!Array.isArray(data.gapQuestions) || data.gapQuestions.length < 1) {
    throw new ResolveShapeError('Understanding field "gapQuestions" must be a non-empty array');
  }
  if (!data.gapQuestions.every(isNonEmptyString)) {
    throw new ResolveShapeError('Understanding field "gapQuestions" contains an empty or non-string entry');
  }
  if (!Array.isArray(data.assumptions)) {
    throw new ResolveShapeError('Understanding field "assumptions" must be an array');
  }
  if (data.assumptions.length > 0 && !data.assumptions.every(isNonEmptyString)) {
    throw new ResolveShapeError('Understanding field "assumptions" contains an empty or non-string entry');
  }
  // Pad gapQuestions to 2 if the model only gave 1, so the frontend's two
  // fixed gap-question slots always have content rather than breaking.
  while (data.gapQuestions.length < 2) {
    data.gapQuestions.push('Is there anything else about this project Resolve should know?');
  }
  return data;
}

function validateRecommendationShape(data) {
  const required = ['recommendedAction', 'why', 'milestone', 'roadmap', 'openDecision'];
  for (const key of required) {
    if (!(key in data)) {
      throw new ResolveShapeError(`Recommendation response missing required field: ${key}`);
    }
  }
  for (const key of ['recommendedAction', 'why', 'milestone', 'openDecision']) {
    if (!isNonEmptyString(data[key])) {
      throw new ResolveShapeError(`Recommendation field "${key}" is empty or not a string`);
    }
  }
  if (!Array.isArray(data.roadmap) || data.roadmap.length < 3) {
    throw new ResolveShapeError('Recommendation field "roadmap" must be an array of at least 3 phases');
  }
  data.roadmap.slice(0, 3).forEach((phase, i) => {
    if (!phase || typeof phase !== 'object') {
      throw new ResolveShapeError(`Roadmap phase ${i} is not an object`);
    }
    for (const key of ['phase', 'action', 'output']) {
      if (!isNonEmptyString(phase[key])) {
        throw new ResolveShapeError(`Roadmap phase ${i} missing or empty field: ${key}`);
      }
    }
  });
  return data;
}

// Calls OpenAI, parses JSON, validates shape. Retries once with a stricter
// instruction if the first attempt fails parsing or validation.
async function callWithValidation(buildPrompt, apiKey, validateFn, attempt = 1) {
  const systemPrompt = buildPrompt(attempt > 1);
  const raw = await callOpenAI(systemPrompt, apiKey, { jsonMode: true });

  let parsed;
  try {
    parsed = JSON.parse(raw);
  } catch (e) {
    if (attempt >= 2) {
      throw new ResolveShapeError(`Response was not valid JSON after retry: ${e.message}`);
    }
    return callWithValidation(buildPrompt, apiKey, validateFn, attempt + 1);
  }

  try {
    return validateFn(parsed);
  } catch (e) {
    if (attempt >= 2) {
      throw e;
    }
    return callWithValidation(buildPrompt, apiKey, validateFn, attempt + 1);
  }
}

// ---------------------------------------------------------------------------
// CALL 1 — Business Analyst: understanding extraction
// Grounded in: 02-roles/business-analyst.md
// Core rules preserved verbatim from that file:
//   "Do not invent missing requirements. Do not guess missing acceptance criteria.
//    Do not infer stakeholder intent, priority, severity, data fields, business
//    rules, or workflow decisions when source context does not support them."
//   "Document assumptions explicitly rather than letting them become invisible
//    implementation constraints."
//   "If the source ticket, comments, feedback, or context needed to validate a
//    requirement is missing, stop or proceed only with an explicit limitation.
//    Do not infer missing project facts from memory."
// ---------------------------------------------------------------------------
async function runUnderstandingCall(projectInput, apiKey) {
  const buildPrompt = (isRetry) => `You are operating as the ResolveOS Business Analyst role.

Your job is requirements clarity: clarifying unclear input, identifying missing information, documenting assumptions explicitly, and reducing ambiguity — applied here to a messy project description instead of a formal requirement.

CORE RULES (do not violate these):
- Do not invent missing facts. Do not guess details the user did not provide.
- Do not infer intent, priority, urgency, or business rules the input does not support.
- Document assumptions explicitly rather than letting them become invisible. Every assumption you make must be labelled as an assumption, not stated as fact.
- If something genuinely needed to understand the project is missing, surface it as a question rather than filling it in yourself.
- Separate what is stated, what is implied, and what is assumed. Do not blur these together.
- Keep output concise and practical. No filler, no generic encouragement, no hype.

THE USER'S PROJECT (raw, unstructured input — may be messy, incomplete, or informal):
"""
${projectInput}
"""

TASK:
Produce a structured understanding of this project. Return ONLY valid JSON matching this EXACT shape — all six fields are REQUIRED, none may be omitted or empty, with no markdown fences, no commentary outside the JSON:

{
  "project": "one or two sentences naming what the project actually is, based only on what was stated",
  "goal": "what the user appears to be trying to achieve, based only on what was stated or clearly implied",
  "state": "where the project currently stands, based only on what was stated",
  "uncertainty": "the single biggest open question or unknown that the input does not resolve",
  "gapQuestions": [
    "one specific, useful question whose answer would materially change the recommendation",
    "a second specific, useful question whose answer would materially change the recommendation"
  ],
  "assumptions": [
    "an assumption you are making to proceed, stated plainly as an assumption, not as fact"
  ]
}

REQUIRED: "project", "goal", "state", and "uncertainty" must each be a non-empty string — if the input is too sparse for genuine confidence, write something like "Not enough detail provided to determine this" rather than leaving it blank or omitting the field. "gapQuestions" must contain at least 2 non-empty strings. "assumptions" must be an array (it may be empty only if truly no assumptions were needed).${isRetry ? '\n\nIMPORTANT: Your previous response did not match this exact shape or had empty/missing fields. Follow the shape precisely this time — every field listed above must be present and non-empty (except assumptions, which may be an empty array).' : ''}`;

  return callWithValidation(buildPrompt, apiKey, validateUnderstandingShape);
}

// ---------------------------------------------------------------------------
// CALL 2 — Strategic Product Director: recommendation + roadmap
// Grounded in: 02-roles/strategic-product-director.md
// Core rules preserved verbatim from that file:
//   "Challenge weak assumptions directly... Prefer evidence over invention...
//    Do not invent market facts, user needs, business value, strategic priority,
//    or product direction when source context does not support them."
//   "Outcome before output. Evaluate work by user outcome, strategic value,
//    product coherence... not by activity volume."
//   "Define why and what... clear enough that [other roles] can act without
//    inventing strategic intent."
// ---------------------------------------------------------------------------
async function runRecommendationCall(confirmedUnderstanding, gapAnswers, apiKey) {
  const buildPrompt = (isRetry) => `You are operating as the ResolveOS Strategic Product Director role.

Your job is strategic judgement: deciding what matters most right now, challenging weak thinking, and giving ONE clear, opinionated recommendation with reasoning — not a menu of options.

CORE RULES (do not violate these):
- Prefer evidence over invention. Do not invent market facts, business value, or strategic priority that the input does not support.
- Evaluate by outcome, not activity. Do not recommend "more work" as a substitute for "the right work."
- Define why and what, clearly enough that someone else could act on it without having to guess your reasoning.
- Challenge weak assumptions directly if you see them in the confirmed understanding below — but do so within your output, not by refusing to answer.
- Give exactly ONE recommended next action. Do not hedge with multiple equally-weighted options.
- Be direct, practical, and free of generic startup hype ("game-changing," "unlock your potential," etc).

CONFIRMED PROJECT UNDERSTANDING (already reviewed and corrected by the user):
"""
${JSON.stringify(confirmedUnderstanding, null, 2)}
"""

ADDITIONAL CONTEXT FROM THE USER:
What would make this session a success: "${gapAnswers?.successDefinition || 'not provided'}"
Deadline or pressure: "${gapAnswers?.deadline || 'not provided'}"

TASK:
Produce ONE clear recommendation and a short roadmap. Return ONLY valid JSON matching this EXACT shape — all fields are REQUIRED, none may be omitted or empty, with no markdown fences, no commentary outside the JSON:

{
  "recommendedAction": "one clear, specific sentence describing the single most important thing to do next",
  "why": "2-3 sentences explaining why this is the right move now, grounded in the confirmed understanding above — not generic advice",
  "milestone": "what 'done' looks like for this recommended action — specific and observable",
  "roadmap": [
    {"phase": "Now", "action": "short phrase", "output": "what this phase produces"},
    {"phase": "Define", "action": "short phrase", "output": "what this phase produces"},
    {"phase": "Launch", "action": "short phrase", "output": "what this phase produces"}
  ],
  "openDecision": "one specific decision the user still needs to make, framed as a direct question, grounded in the confirmed understanding"
}

REQUIRED: "recommendedAction", "why", "milestone", and "openDecision" must each be a non-empty string. "roadmap" MUST contain exactly 3 phase objects, each with non-empty "phase", "action", and "output" string fields. Do not omit any field or leave any value empty.

If the deadline or success definition were not provided, do not invent urgency or goals that weren't stated — work from what's actually known.${isRetry ? '\n\nIMPORTANT: Your previous response did not match this exact shape or had empty/missing fields, especially in the roadmap array. Every roadmap phase must have all three fields filled in. Follow the shape precisely this time.' : ''}`;

  return callWithValidation(buildPrompt, apiKey, validateRecommendationShape);
}

// ---------------------------------------------------------------------------
// CALL 3 — QA self-check
// Grounded in: 06-governance/project-readiness.md (Validation State Model)
//   "Evidence: a source-backed observation... that supports or weakens an
//    assumption." / "Do not hide uncertainty about readiness."
// And the cross-role anti-pattern repeated in every role file:
//   "approve fake functionality, fake workflow states, fake data, or fake
//    certainty" — applied here to catch the recommendation overreaching
//    beyond what the confirmed understanding actually supports.
// ---------------------------------------------------------------------------
async function runQACheckCall(confirmedUnderstanding, gapAnswers, draftRecommendation, apiKey) {
  const buildPrompt = (isRetry) => `You are operating as a ResolveOS QA self-check pass.

Your job is to check a draft recommendation against the original confirmed understanding it was supposed to be based on, and catch any case where the recommendation states something as fact that was not actually established.

CORE RULE: Do not approve fake certainty. If the draft recommendation asserts something the confirmed understanding does not support, flag and correct it. If the draft is well-grounded, pass it through unchanged.

CONFIRMED UNDERSTANDING (the source of truth):
"""
${JSON.stringify(confirmedUnderstanding, null, 2)}
"""

USER-PROVIDED CONTEXT:
Success definition: "${gapAnswers?.successDefinition || 'not provided'}"
Deadline: "${gapAnswers?.deadline || 'not provided'}"

DRAFT RECOMMENDATION TO CHECK:
"""
${JSON.stringify(draftRecommendation, null, 2)}
"""

TASK:
Check every claim in the draft recommendation against the confirmed understanding and user-provided context above. If everything is properly grounded, return the draft recommendation exactly as given. If something is invented, overstated, or not actually supported by the source material, correct ONLY that part — keep everything else from the draft unchanged.

Return ONLY valid JSON in this EXACT shape — all fields REQUIRED and non-empty, with no markdown fences, no commentary outside the JSON, and no notes about what you changed:

{
  "recommendedAction": "non-empty string",
  "why": "non-empty string",
  "milestone": "non-empty string",
  "roadmap": [
    {"phase": "non-empty string", "action": "non-empty string", "output": "non-empty string"},
    {"phase": "non-empty string", "action": "non-empty string", "output": "non-empty string"},
    {"phase": "non-empty string", "action": "non-empty string", "output": "non-empty string"}
  ],
  "openDecision": "non-empty string"
}

The roadmap array must always contain exactly 3 phase objects, even if you are passing the draft through unchanged — copy them across completely, do not drop or truncate any field.${isRetry ? '\n\nIMPORTANT: Your previous response did not match this exact shape or had empty/missing fields. Copy every field from the draft across completely unless you are specifically correcting it. Follow the shape precisely this time.' : ''}`;

  return callWithValidation(buildPrompt, apiKey, validateRecommendationShape);
}

// ---------------------------------------------------------------------------
// Shared OpenAI call helper
// ---------------------------------------------------------------------------
async function callOpenAI(systemPrompt, apiKey, { jsonMode = false } = {}) {
  const response = await fetch('https://api.openai.com/v1/chat/completions', {
    method: 'POST',
    headers: {
      'Content-Type': 'application/json',
      Authorization: `Bearer ${apiKey}`,
    },
    body: JSON.stringify({
      model: 'gpt-4o',
      messages: [{ role: 'system', content: systemPrompt }],
      temperature: 0.4,
      ...(jsonMode ? { response_format: { type: 'json_object' } } : {}),
    }),
  });

  if (!response.ok) {
    const errBody = await response.text();
    throw new Error(`OpenAI API error (${response.status}): ${errBody}`);
  }

  const data = await response.json();
  const content = data?.choices?.[0]?.message?.content;
  if (!content) {
    throw new Error('OpenAI returned an empty response');
  }
  return content;
}

```
