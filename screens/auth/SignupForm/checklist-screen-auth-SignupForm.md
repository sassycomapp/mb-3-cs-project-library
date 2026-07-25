---
document: Compliance Checklist — screen-auth-SignupForm
doc-id: mb-3-cs-0049
state: Live
date-created: 2026-07-02
---

# Compliance Checklist — screen-auth-SignupForm

**Screen:** `screens/auth/SignupForm/screen-auth-SignupForm.html`
**Date:** 2026-07-02 | **Revised:** 2026-07-02 — 2 errors fixed

## Errors found and fixed
1. **Hardcoded rgba** — `rgba(73,62,243,0.92)` and `rgba(0,0,0,0.3)` on `.btn-filled:hover` → replaced with CSS custom properties using `color-mix()`
2. **Missing dark mode** → added

## Verification
- [x] Wireframe fidelity — all components match
- [x] Page chrome — none
- [x] No DataGrid/RP
- [x] MUI overlay — 8px radius, shadow, 600 heading
- [x] Token compliance — no hardcoded rgba (verified)
- [x] Dark mode present (verified)
- [x] Max-width 400px centred (auth form)
- [x] Writeback: txt_full_name, txt_email, txt_password, txt_confirm_password (all W)
