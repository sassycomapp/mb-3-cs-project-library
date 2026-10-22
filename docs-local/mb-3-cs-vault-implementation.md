---
title: "mb-3-cs-vault-implementation"
doc-id: "mb-fc24acf4"
updated: "2026-10-22"
---
# Mybizz CS — Vault Implementation

**Scope:** `mb-3-cs` only. The Vault pattern every Mybizz application follows is [[[default:specifications-global/spec-vault-system]]](C:/dev/project-library-global/specifications-global/spec-vault-system.md); this document holds what `mb-3-cs` stores in its Vault, its VaultForm and its build sequence.

## 1. What's Stored in the Vault

| Secret | Used by |
|---|---|
| `stripe_secret_key` | Payment Gateway (Stripe) |
| `paystack_secret_key` | Payment Gateway (Paystack) |
| `paypal_secret` | Payment Gateway (deferred to V2) |
| SMTP credentials / `smtp_password` | Email Configuration (Brevo SMTP) |
| Brevo API key (`brevo_api_key`) | Brevo CRM integration |
| Shared event push token | Mybizz_management event forwarding |

Publishable keys (e.g. Stripe publishable key) are safe for client-side use and are stored in the `payment_config` table, not the Vault.

**SMTP constants** (hardcoded — identical for all clients, not stored in Vault):
- Host: `smtp-relay.brevo.com`
- Port: `587` (STARTTLS)

## 2. VaultForm UI

Standalone form accessed from Settings, visible to Owner only:
- Second-factor notice, displayed prominently at the top: the Owner signs in again with password and FIDO2 key on every open, as [[[default:specifications-global/spec-security-architecture]]](C:/dev/project-library-global/specifications-global/spec-security-architecture.md) sets out.
- Stored Secrets list — name, last updated date, usage context. View (masked reveal) and Delete actions per item.
- Add New Secret form — name + password input (`hide_text=True`) at the bottom. Value never displayed again after saving.

Visual references: `wireframes/wireframe-settings-VaultForm.html`, `screens/screen-settings-VaultForm.html`.

## 3. Build Sequence

Built in Phase 1, Stage 1.5 of `build-plan.md`, after authentication and settings (Stages 1.0–1.4). Phase 2 (payments, bookings, services) depends on the Vault being functional first.

### Deliverables
- `vault` Data Table with encrypted secrets storage
- `encryption_key` set in Anvil Secrets
- `server_shared/encryption_service.py`
- `server_shared/vault_service.py`
- `server_shared/vault_step_up_service.py`
- `settings/VaultForm`

### Completion Criteria
- `encryption_key` set in Anvil Secrets (only item there)
- VaultForm requires a fresh second factor on every open, no grace period
- Email notification sent to Owner on every Vault access
- Secret keys stored encrypted in `vault` table
- `get_vault_secret()` retrieves secrets correctly
- Payment config uses Vault for secret keys (not stored in `payment_config` table)
- Secret masking in `get_payment_config()` returns `'***'`
