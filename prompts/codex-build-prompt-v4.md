---
status: historical
---

Repo: simeonfab/ResolveOS-intake-frontend. Locate the existing public/index.html.

STANDING PRINCIPLES (carry over from previous prompts): modular, extensible code;
do not touch anything not listed below beyond what's required to fix this; ask if
context is missing rather than guessing.

================================================================================
Screen 0 greeting beat - fix sizing hierarchy + final subheading copy
================================================================================

Following the previous round of changes (slower 1.5s-paced reveal, larger text), the
greeting beat is now too large overall and the subheading is wrapping onto two lines
when it should read as one.

FIX:

1. Replace the subheading copy with this exact final text:
   "Turn a messy project into one clear move."

2. Establish a clear size hierarchy between "Hi [name]." and the subheading, rather
   than both being scaled equally large:
   - "Hi [name]." stays large/prominent (keep its current size - this part was
     confirmed correct).
   - The subheading should be NOTICEABLY SMALLER than "Hi [name]." - a clear visual
     secondary line, not competing at equal size. Reduce it enough that, combined
     with the shorter copy above, it comfortably fits on a single line at typical
     mobile and desktop widths.

3. Ensure the subheading does not wrap to a second line under normal viewport widths
   given the new shorter copy and reduced size - verify this holds on both a typical
   mobile width and desktop width, not just one.

4. Keep the existing fade-in timing/sequencing from the previous round (the ~1.5s
   staggered reveal of "Hi [name]." -> subheading -> "Get started" button) - this
   prompt only changes copy and font sizing, not the timing or transition mechanism.

================================================================================
After implementing: git add -A && git commit -m "Fix screen 0 greeting size hierarchy
and finalise subheading copy" && git push origin main. Confirm the push succeeded and
report the commit hash.
================================================================================
