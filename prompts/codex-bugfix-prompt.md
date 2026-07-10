---
status: historical
---

Two bugs found after the last round of changes deployed. Locate them in the existing
codebase (public/index.html in simeonfab/ResolveOS-intake-frontend) and fix properly -
investigate root cause rather than patching symptoms. Do not touch anything else in
the file beyond what's needed to fix these two issues.

================================================================================
BUG 1 - "So," eyebrow renders wrong (capitalisation + size)
================================================================================

On the "what do you need help with" step (Screen 0, journey step), the small
transition label intended to read "So," (capital S, lowercase o, comma) is rendering
incorrectly - almost certainly because the shared `.eyebrow` CSS class applies
`text-transform: uppercase` globally, which would force "So," to display as "SO,"
regardless of what's actually in the HTML.

FIX: This specific instance needs to NOT be forced to uppercase - it should render
exactly as written ("So," in sentence case). It should also be noticeably bigger than
the standard eyebrow label size elsewhere in the app, since it's meant to function as
a brief, casual transition word, not a small all-caps section label.

Do not change the `.eyebrow` class's behaviour everywhere else in the app (it's used
correctly as an uppercase label on other screens) - this needs a scoped
override/exception for this one instance only, not a global change to the class.

================================================================================
BUG 2 - content visibly jumps/settles after screen transitions (likely systemic)
================================================================================

Reported behaviour: when moving to a new screen (e.g. clicking "I have a messy
project" to move to the next screen, and also observed on other screen transitions
generally - described as happening "on a lot of pages", likely systemic rather than
isolated to one screen), the content area first appears positioned lower on the
screen than it should be, then visibly jumps/snaps upward into its correct final
position shortly after.

This most likely relates to the crossfade transition mechanism added in the previous
round of changes (the shared fade-in/fade-out used by goToScreen() for all screen
changes). A common cause of this exact symptom: content becomes visible (opacity
fading in) before the browser has finished computing its final layout/height/
position - for example if scroll position, content height, or vertical centering is
being calculated or reset AFTER the fade-in has already started, or if there's a
mismatch between when content is unhidden (display/visibility) versus when its
opacity transition begins.

FIX: Investigate the actual sequencing in the crossfade transition logic - ensure
the incoming screen's final layout (height, scroll position, any centering) is fully
settled BEFORE its fade-in (opacity transition) begins, so nothing is visible moving
into place after it's already partially visible. The user should see a clean fade
between two already-correctly-positioned states, with no visible reflow, jump, or
snap at any point during or after the transition.

Test this across multiple screen transitions (not just Screen 0 -> Screen 1) to
confirm the fix is systemic and not just a one-screen patch, since the report
suggests this is happening broadly.

================================================================================
After fixing both: git add -A && git commit -m "Fix eyebrow capitalisation/sizing and
screen transition layout jump" && git push origin main. Confirm the push succeeded and
report the commit hash. If you can't reproduce bug 2 or aren't confident you've found
the actual root cause (rather than just masking the symptom), say so explicitly rather
than reporting it as fixed.
================================================================================
