# Mybizz CS — The Vault System

**Authority:** ADR-003, security-architecture.md, build-plan.md
**Note:** The generic Vault pattern (two-level secrets model, enforcement/masking code) is now
in `spec-security.md` — not repeated here. This document contains only this app's specific
implementation.

---

## What's Stored in the Vault

| Secret | Used by |
|---|---|
| `stripe_secret_key` | Payment Gateway (Stripe) |
| `paystack_secret_key` | Payment Gateway (Paystack) |
| `paypal_secret` | Payment Gateway (deferred to V2) |
| SMTP credentials / `smtp_password` | Email Configuration (Brevo SMTP) |
| CRM tokens | CRM integrations |

Publishable keys (e.g. Stripe publishable key) are safe for client-side use and are stored in the
`payment_config` table, not the Vault.

## VaultForm UI

Standalone form accessed from Settings, visible to Owner only:
- TOTP verification notice, displayed prominently at the top.
- Stored Secrets list — name, last updated date, usage context. View (masked reveal) and Delete
  actions per item.
- Add New Secret form — name + password input (`hide_text=True`) at the bottom. Value never
  displayed again after saving.

Visual references: `wireframes/wireframe-settings-VaultForm.html`,
`screens/screen-settings-VaultForm.html`.

## Build Sequence

Built in Phase 1, Stage 1.5 of `build-plan.md`, after authentication and settings (Stages
1.0–1.4). Phase 2 (payments, bookings, services) depends on the Vault being functional first.

### Deliverables
- `vault` Data Table with encrypted secrets storage
- `encryption_key` set in Anvil Secrets
- `server_shared/encryption_service.py`
- `server_shared/vault_service.py`
- `server_shared/vault_totp_service.py`
- `settings/VaultForm`

### Completion Criteria
- `encryption_key` set in Anvil Secrets (only item there)
- VaultForm requires TOTP step-up on every open, no grace period
- Email notification sent to Owner on every Vault access
- Secret keys stored encrypted in `vault` table
- `get_vault_secret()` retrieves secrets correctly
- Payment config uses Vault for secret keys (not stored in `payment_config` table)
- Secret masking in `get_payment_config()` returns `'***'`

## Related Files

| File | Relationship |
|---|---|
| `adr/3-payment-security-boundary-vault.md` | Architectural decision — accepted 2026-03-15 |
| `security-architecture.md` | This app's RBAC limits, token security, regulatory posture |
| `spec-security.md` | The generic Vault/security pattern |
| `DESIGN.md` | VaultForm design specification |
| `build-plan.md` | Stage 1.5: The Vault |

---

*vault-system.md v2.0 — trimmed to this app's specifics; generic pattern moved to
spec-security.md.*
