---
title: "16 — F-01 Accounts, roles and access"
status: open
labels: [ready-for-agent]
created: 2026-11-17
doc-id: "mb-dce32bf3"
updated: "2026-11-17"
---

## Description

**Feature:** F-01 Accounts, roles and access (order 2 — auth). Requirements: concept §7; architecture reference §12 F-01; UF-03, UF-38; `spec-security`, `spec-security-architecture`; SR-01–SR-05; FR-01.
**Depends on:** 01 (layouts), 02 (shared client), 03 (rbac).

### Forms

- `auth.LoginForm` — the startup form; routes by role before `init_components()`: team member → Dashboard (Owner → Onboarding while `onboarding_status` is `in_progress`), Customer → portal, public address → the route, signed out → sign-in; startup theming calls (`m3x_theming.apply_saved()`, `set_palette`, `set_mode("system")`) are completed at ticket 19
- `auth.PasswordResetForm` — request a reset link
- `auth.TwoFactorForm` — enrol a second factor; enter the code or use the security key at sign-in
- `auth.SetPasswordForm` — set a password from an emailed link (new team member, customer, reset)
- `auth.SignupForm` — DELETED at this step (C2: a placeholder form not in the scaffold; sign-up is off)

### Server functions

- `server_auth/auth_service` (renamed from `server_auth/service.py` at this step, per the divergence inventory) — invitations, set-password links, lockout, session ending (`end_user_sessions`)
- `server_auth/rbac` — from ticket 03

### Tables

- `users`, `rate_limits`, `activity_log`

### Acceptance tests

- `SC-02` — phishing-resistant sign-in: every account holder signs in with a second factor; the Owner's is FIDO2 only
- `SC-04`, `ST-12` — sessions: 30-minute idle enforced by Anvil's session mechanism, never overridden; return to the same place; the Owner can end any user's sessions
- `IN-02`, `IN-03` — the access matrix over every function and role; row-scoped reads
- `SR-01`–`SR-05`; test specification §3 RBAC matrices; UF-03, UF-38

### Security criteria

- B1, B2 — decorators match the five-role table exactly; the Owner's high-value functions refuse while any other factor is enrolled; navigation hides, never protects
- B7 — identical wording for unknown email and wrong password; reset by email only; no password ever sent by email (D7)
- AC-S1, AC-S2, AC-S6, AC-S8 (sign-in rate limits via `rate_limits`)

### Notes

- Users service settings (sign-up off, confirm email on, remember me 30 minutes, `require_mfa: true`, user table `users`) are Anvil-editor settings; the agent never writes `anvil.yaml`.

## Comments
