---
status: historical
---

Repo: simeonfab/ResolveOS-intake-frontend. Locate the existing public/index.html and
api/resolve.js - these already contain the live pipeline, do not rebuild from scratch.

STANDING PRINCIPLES (carry over from previous prompts): modular, extensible code;
do not touch anything not listed below; ask if context is missing rather than guessing.

================================================================================
1. Screen 0 greeting beat - slower timing, bigger text, fixed wording
================================================================================

The three-beat reveal ("Hi [name]." -> explanation -> "Get started" button) is too
fast and the text is too small relative to the available space (this is a centred,
mostly-empty screen with a lot of unused real estate).

FIX:
- Increase the pause between each beat to roughly 1.5 seconds (currently much
  shorter). Sequence: "Hi [name]." appears -> ~1.5s pause -> explanation line fades
  in -> ~1.5s pause -> "Get started" button fades in. Keep the same fade-in transition
  style already used, just with longer gaps between each beat triggering.
- Increase the font size of both "Hi [name]." and the explanation line significantly -
  there's ample empty space on this screen, use it. These should read as the dominant
  visual elements on the page, similar scale to how H1 headings are sized elsewhere
  but can be larger given this screen has nothing else competing for attention.
- Replace the explanation copy. Current text: "Resolve will turn your messy project
  into a clear next move." New text: "Resolve takes a messy project and gives you one
  clear next move." (Tighter phrasing, drops the future-tense hedge.)

================================================================================
2. Processing ring animation direction - currently backwards
================================================================================

Reported: the loading ring animation appears to go from a full/mostly-full circle to
a half-full circle, rather than filling up as work progresses. The intended behaviour
is the OPPOSITE: it should start as an empty/clear circle outline and progressively
fill in (clockwise, becoming a complete circle) as the call completes - i.e. visually
represent 0% -> 100% progress, not appear to shrink or empty out.

Investigate the actual SVG stroke-dasharray/stroke-dashoffset logic currently driving
this ring (used on both the Screen 1 -> Screen 2 processing transition and the
Screen 2 -> Screen 3 processing transition). Verify the offset values and any
rotation transform are producing a genuine "filling up from empty to complete" visual,
not the reverse. Fix the actual direction/values if they're inverted - don't just
adjust timing, confirm the visual direction is correct.

================================================================================
3. "Output my project plan" card - loading animation still missing
================================================================================

A previous prompt specified that clicking this card should show a visible loading
animation inside the card while the 4th API call (project plan generation) is in
flight, before transforming into a "Download report" button. This either wasn't
implemented or isn't rendering - currently there is no visible indication the card is
working while the call runs.

FIX: Verify this was actually implemented. If missing or broken, implement a small,
clearly visible loading animation (spinner, pulsing dots, or similar - consistent with
the app's visual style) that displays inside the card immediately on click, persists
for the duration of the API call, and is replaced by the "Download report" button only
once the response has been received and validated.

================================================================================
4. Screen 4 - secondary action cards and win-banner text are currently FAKE
================================================================================

This is the most important fix in this batch. The five secondary action cards on
Screen 4 (everything except the primary recommended-action card and the "Output my
project plan" card) currently contain hardcoded, leftover placeholder text from the
original seeded mockup (e.g. references to a rebrand, Instagram followers, packaging -
specific to a fictional example project, not the real user's actual project). The
win-banner description text at the top of Screen 4 has the same problem.

This directly undermines the actual purpose of this test build: giving the real
tester genuine value from their own input, so we can tell whether the tool actually
helps. Static, irrelevant suggestions fail that test regardless of how good the rest
of the flow looks.

REQUIRED FIX - make both of these genuinely dynamic, grounded in the real session
data, not templated or invented:

a) Extend the Call 2 (runRecommendationCall, in api/resolve.js) system prompt and JSON
   schema to ALSO return:
   - "secondaryActions": an array of exactly 5 objects, each {"icon": "single relevant
     emoji", "label": "short, specific, actionable suggestion relevant to THIS
     project"} - generated from the actual confirmedUnderstanding and gapAnswers for
     this session, not generic categories. These should feel like genuinely useful,
     specific next steps a real person would want to click on for their actual
     project (similar spirit to how the primary recommended action is already grounded
     in real input - apply the same "evidence over invention" rule already used
     elsewhere in this call's system prompt).
   - "winSummary": one sentence following this exact style/structure, but
     substituting in language grounded in the real project rather than a fixed
     template: "A messy project with no clear direction — turned into a focused path
     forward. That's the hard part done." The sentence structure/tone should match
     this example (acknowledge the messy starting point, name the transformation,
     close with "that's the hard part done" or equivalent) but should reference what
     the project actually was about where natural, not be copy-pasted verbatim for
     every session. Correct spelling and grammar (the literal example text provided
     contains an em-dash and is grammatically the target style to follow).

b) Validate both new fields the same way other fields in this call are validated
   (secondaryActions must be an array of exactly 5 objects each with non-empty icon
   and label strings; winSummary must be a non-empty string), using the existing
   one-retry-then-error pattern already implemented for this call.

c) Update the frontend (public/index.html) to render these 5 secondaryActions
   dynamically into the action-grid (replacing the current hardcoded cards entirely),
   and to render the winSummary text into the win-banner description (replacing the
   current hardcoded placeholder text there too).

================================================================================
After implementing everything above: git add -A && git commit -m "Slow down screen 0
timing, fix processing ring direction, fix missing plan-loading animation, make
screen 4 secondary actions and win text fully dynamic" && git push origin main.
Confirm the push succeeded and report the commit hash. If anything above was
ambiguous or you had to make an assumption, list it explicitly rather than silently
deciding.
================================================================================
