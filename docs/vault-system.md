# Mybizz CS — The Vault System

**Authority:** Derived from ADR-003, security-architecture.md, platform-overview.md, development-policy.md, and build-plan.md.

---

## Overview

The Vault is an application-level encrypted secrets store built into every client instance. It holds all client API keys and credentials — payment gateway keys, SMTP credentials, CRM tokens, etc.

### Why it exists

Anvil Secrets is the platform's built-in secrets facility, but it has a problem: giving clients access to Anvil Secrets would require giving them IDE access. The Vault solves this by providing an application-level encrypted store that the Owner can manage through the UI. The only item in Anvil Secrets is the `encryption_key` used by the Vault — nothing else ever goes there. (ADR-003, `development-policy.md`)

---

## Two-Level Secrets Model

| Level | Contents | Who writes it | How it's accessed |
|---|---|---|---|
| **Anvil Secrets** | Exactly one item: `encryption_key` (Fernet symmetric key) | Mybizz at provisioning via the Anvil IDE. Never changes after provisioning. | `vault_service.py` only — no other code calls `anvil.secrets.get_secret()` |
| **The Vault** (`vault` Data Table) | All client API keys and credentials (encrypted at rest) | Owner via VaultForm | `get_vault_secret(name)` from `vault_service.py` — server-side only |

---

## How Secrets Flow

### Storage

When the Owner adds a secret through VaultForm, the value is encrypted using Fernet symmetric encryption (key from Anvil Secrets) before being written to the `vault` Data Table. Plaintext never persists.

### Retrieval

All integration server modules (payments, email, CRM) call `get_vault_secret(name)` from `vault_service.py` at runtime. The function decrypts on read. Secrets are never returned to client code, never logged, never sent in masked form.

### Masking in Config Endpoints

Functions like `get_payment_config()` and `get_email_config()` return `'***'` for any secret key field. The client never receives the actual value, and client save methods must exclude secret key fields from outbound dicts entirely. (ADR-003)

---

## Security Enforcement

### Access Control

- Vault access is restricted to the **Owner role only** (one per instance).
- Every open of the VaultForm requires **TOTP step-up authentication** (Google Authenticator) — no grace period, no "remember me."
- An **email notification** is sent to the Owner on every Vault access.
- All Vault access is logged to the **audit log** (actor, timestamp, description).

### Architectural Guard (ADR-003)

Any server function that saves payment config must contain a hard-fail assertion to prevent secret keys from being saved to the wrong table:

```python
_PAYMENT_SECRET_COLUMNS = frozenset({
    'stripe_secret_key', 'paystack_secret_key', 'paypal_secret'
})
assert not (_PAYMENT_SECRET_COLUMNS & set(data.keys())), (
    f"Secret key columns must not be saved here: "
    f"{_PAYMENT_SECRET_COLUMNS & set(data.keys())}"
)
```

`AssertionError` must always be re-raised — never swallowed.

---

## What's Stored in the Vault

| Secret | Used by |
|---|---|
| `stripe_secret_key` | Payment Gateway (Stripe) |
| `paystack_secret_key` | Payment Gateway (Paystack) |
| `paypal_secret` | Payment Gateway (deferred to V2) |
| SMTP credentials / `smtp_password` | Email Configuration (Brevo SMTP) |
| CRM tokens | CRM integrations |

Publishable keys (like Stripe publishable key) are safe for client-side use and are stored in the `payment_config` table, not the Vault.

---

## VaultForm UI

The VaultForm is a standalone form accessed from Settings, visible to Owner only. It contains:

- **TOTP verification notice** — displayed prominently at the top.
- **Stored Secrets list** — shows each secret's name, last updated date, and usage context. Each item has View (masked reveal) and Delete actions.
- **Add New Secret form** — name input + password input at the bottom. The value input uses `hide_text=True`. After saving, the value is never displayed again.

Visual references:
- `wireframes/wireframe-settings-VaultForm.html` — construction guide wireframe
- `screens/screen-settings-VaultForm.html` — M3 screen mockup

---

## Build Sequence

The Vault is built in **Phase 1, Stage 1.5** of the build plan (`build-plan.md`), after authentication and settings (Stages 1.0–1.4) are complete. Phase 2 (payments, bookings, services) depends on the Vault being functional first.

### Deliverables

- `vault` Data Table with encrypted secrets storage
- `encryption_key` set in Anvil Secrets
- `server_shared/encryption_service.py` — encryption/decryption functions
- `server_shared/vault_service.py` — Vault CRUD functions
- `server_shared/vault_totp_service.py` — TOTP step-up authentication
- `settings/VaultForm` — Owner-only UI with TOTP gate

### Completion Criteria

- `encryption_key` set in Anvil Secrets (only item there)
- VaultForm requires TOTP step-up on every open
- No grace period — TOTP required each time
- Email notification sent to Owner on every Vault access
- Secret keys stored encrypted in `vault` table
- `get_vault_secret()` retrieves secrets correctly
- Payment config uses Vault for secret keys (not stored in `payment_config` table)
- Secret masking in `get_payment_config()` returns `'***'` for secrets

---

## Related Files

| File | Relationship |
|---|---|
| `adr/3-payment-security-boundary-vault.md` | Architectural decision — accepted 2026-03-15 |
| `docs/security-architecture.md` | Full security and secrets architecture |
| `docs/platform-overview.md` | Section 9: The Vault |
| `docs/development-policy.md` | Section 9: Security Standards — secrets and Vault |
| `docs/DESIGN.md` | VaultForm design specification |
| `implementation/build-plan.md` | Stage 1.5: The Vault |
| `wireframes/wireframe-settings-VaultForm.html` | Wireframe construction guide |
| `screens/screen-settings-VaultForm.html` | M3 screen mockup |

---

*End of vault-system.md*
