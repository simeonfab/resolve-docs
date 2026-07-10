---
status: current
note: "Imported from download copy '03-R1-signin-otp-reskin-1.md' (the current version); the copy without suffix was an older draft, now in archive/."
---

# Ticket R1 — Sign-in (OTP) Reskin

**Repo:** `cap-pm-cockpit-alaria`
**Type:** Reskin. The OTP auth logic already exists and works in production (magic-link was replaced with OTP earlier this project). This ticket is visual only — do not touch `signInWithOtp`/`verifyOtp` logic itself unless something in this spec genuinely requires a behavioural change (flagged below if so).
**Reference file (pixel-exact):** `ResolvePM Sign-in.dc.html`
**Parallel-safe** with every other ticket in this repo.

## Screen 1 — Email entry
- Brand mark top-left, standalone (no nav shell — this screen exists before the app shell reveals).
- Content block pinned toward the bottom of the screen (not centered) — eyebrow "Operational clarity for product work." (sentence case, matching the rest of the system — **the live version currently shows this in ALL CAPS, which must be corrected to sentence case here**).
- Headline (serif): "Create your Resolve workspace."
- Subhead: "Enter your email and we'll send you a code to sign in."
- Email input, **"Send code" button** (not "Send secure link" — copy must say code, never link, throughout this entire flow).
- Footer text: "Already have a workspace? Sign in" (link).

## Screen 2 — Code entry
- "← Change email" link/back action at top.
- Same eyebrow/brand treatment as screen 1.
- Headline: "Check your email." Subhead: "We've sent a 6-digit code to **[user's actual email]**. Enter it below."
- **Six individual digit boxes**, not one plain text field. Active box gets an amber border/glow; filled boxes show a slightly muted border.
- **"Verify and continue" button.**
- **Resend cooldown, required behaviour:** "Didn't get it? Resend available in 0:42" (counting down) — the resend link/action must be disabled during the cooldown window and only become an active link once it expires. This directly respects the mailer rate limit already hit once this project (Resend's built-in limits) — do not let the UI invite rapid re-requests.

## Screen 2 — Error state
- Same layout, but all six digit boxes show a terracotta/error border and error-coloured text.
- Inline error message below the boxes (small icon + text): "That code is incorrect or expired. Try again." — **must appear inline, no page reload.**
- In the error state, "Resend code" is shown as an active link (not a cooldown state) — presumably because a wrong/expired code justifies an immediate retry option. Confirm this is intentional rather than assume; if the cooldown should still apply even after an error, flag it rather than silently picking either behaviour.

## Acceptance criteria
- All copy says "code," never "link," anywhere in this flow.
- Six-digit-box input pattern implemented, not a single text field.
- Resend cooldown timer works and correctly gates the resend action.
- Error state shows inline, no page reload, matches the visual treatment exactly.
- No change to the underlying OTP verification logic unless a real bug is found — if so, report it, don't silently fix and reskin at the same time.
