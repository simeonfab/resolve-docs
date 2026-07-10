---
status: superseded
superseded_by: "briefs/03-R1-signin-otp-reskin.md"
note: "Earlier draft of the R1 brief. Renamed on import to free the canonical name for the current version."
---

# Ticket R1 — Sign-in (OTP) — Dark Re-theme

**Repo:** `cap-pm-cockpit-alaria`
**Type:** Visual re-theme only (light → dark). Revision of the original R1, which is **already live on `main` as a light screen**.
**Reference file (pixel-exact):** `ResolvePM Sign-in.dc.html` (updated dark version).
**Parallel-safe** to build with every other ticket, but see the continuity note below.

> **Why this changed.** The original R1 shipped light. The Intake→ResolvePM bridge (Ticket I2, other repo) is dark (Ink + amber), and hands directly into sign-in. A light sign-in caused a jarring dark→light flash right at the hand-off. Re-theming sign-in to dark removes that flash and makes the transition feel like one continuous product. This is the whole point of the change.

---

## Scope — visual only

Re-theme **all three states** to the dark treatment in the updated mockup. **The auth logic, copy, and behaviour are already working in production and must not change** — this is styling only.

### States to re-theme
1. **Email entry** — "Create your Resolve workspace" / email field / "Send code".
2. **Code entry** — 6-digit boxes / "Verify and continue" / resend cooldown.
3. **Error state** — six error-bordered boxes / inline error message / active "Resend code".

### Exact dark tokens (from the mockup)
- Page background: `#141311`
- Panel/pane: `#201F1C`, border `rgba(255,255,255,.07)`
- Body text: `#FBF8F2`; muted text `#8A8578` / `#B4AFA3`
- Eyebrow + primary buttons: amber `#C8A040` (eyebrow is now amber, not terracotta)
- Inputs: background `rgba(255,255,255,.05)`, border `rgba(255,255,255,.12)`, focus amber border + `0 0 0 3px rgba(200,160,64,.22)`
- OTP boxes: same input treatment; filled border `rgba(255,255,255,.22)`; active amber; error terracotta `#C97A4A`
- Error text/border: terracotta `#C97A4A`

---

## Do NOT touch (logic already works in production)
- `signInWithOtp` / `verifyOtp`.
- The resend cooldown timer and its gating.
- The inline error handling (wrong/expired code shows inline, no page reload).
- The copy — it already says "code", never "link", throughout. Keep it.

## File-touch discipline
Only the sign-in screen's own files. Do not modify shared nav/shell or anything another ticket owns.

## Process
- **Already live on `main` as light** → work on a **new branch off `main`**, do not touch `main` directly.
- Deploy a **Preview**, verify, then merge only after confirmation.
- Ephemeral container — **commit as you go and push.**

## Continuity note
The full dark-continuous flow (Intake bridge → dark sign-in) only verifies once **both** this and I2 are on preview. Build independently, but walk the seam end-to-end before either merges. Note this re-themes a screen that is currently live in production.

## Acceptance criteria
- All three sign-in states match the updated dark mockup pixel-for-pixel.
- No light sign-in screen remains anywhere in the flow.
- Zero behavioural changes: OTP send/verify, resend cooldown, inline error, and "code"-not-"link" copy all work exactly as before.
- Deployed to a preview branch off `main`, not committed straight to `main`.
