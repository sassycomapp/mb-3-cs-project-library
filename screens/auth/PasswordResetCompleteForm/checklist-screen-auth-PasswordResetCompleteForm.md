---
document: Compliance Checklist — screen-auth-PasswordResetCompleteForm
doc-id: mb-3-cs-0047
state: Live
date-created: 2026-07-02
---

# Compliance Checklist — screen-auth-PasswordResetCompleteForm

**Screen:** `screens/auth/PasswordResetCompleteForm/screen-auth-PasswordResetCompleteForm.html`
**Date:** 2026-07-02 | **Revised:** 2026-07-02 — 2 errors fixed

## Errors found and fixed
1. **Hardcoded rgba** — `rgba(73,62,243,0.92)` and `rgba(0,0,0,0.3)` on `.btn-filled:hover` → replaced with CSS custom properties using `color-mix()`
2. **Missing dark mode** → added

## Verification
- [x] Wireframe fidelity — all components match
- [x] Token compliance — no hardcoded rgba (verified)
- [x] Dark mode present (verified)
- [x] Max-width 400px centred (auth form)
- [x] Writeback: txt_new_password, txt_confirm_new_password (both W, hide_text)
