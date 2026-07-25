---
document: Compliance Checklist — screen-auth-LoginForm
doc-id: mb-3-cs-0046
state: Live
date-created: 2026-07-02
---

# Compliance Checklist — screen-auth-LoginForm

**Screen:** `screens/auth/LoginForm/screen-auth-LoginForm.html`
**Date:** 2026-07-02 | **Revised:** 2026-07-02 — 2 errors fixed

## Errors found and fixed
1. **Hardcoded rgba** — `rgba(73,62,243,0.92)` and `rgba(0,0,0,0.3)` on `.btn-filled:hover` → replaced with `var(--btn-filled-hover)` and `var(--btn-shadow-hover)` using `color-mix()`
2. **Missing dark mode** → added `@media (prefers-color-scheme: dark)` with 7 token overrides

## Verification
- [x] Wireframe fidelity — all components match (card_login, hdg, txt, txt_email, txt_password, cb_remember_me, btn_login, nav_forgot_password, txt_signup_prompt, nav_sign_up)
- [x] Page chrome — none
- [x] No DataGrid/RP
- [x] MUI overlay — 8px radius, shadow, 600 heading
- [x] Token compliance — no hardcoded rgba (verified)
- [x] Dark mode present (verified)
- [x] Max-width 400px centred (auth form)
- [x] Writeback: txt_email (W), txt_password (W, hide_text) noted in wireframe
