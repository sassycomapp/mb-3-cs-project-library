# Mybizz CS — Security & Secrets Architecture

**Authority:** Mandatory — all security implementation must conform  
**Related ADRs:** ADR-003 (Vault), ADR-028 (Client Instance Architecture), ADR-033 (Mybizz_management Visibility)

---

## 1. Two-Level Secrets Model

### Anvil Secrets — One Item Only
Holds exactly one item: `encryption_key`. This is a Fernet symmetric encryption key set by Mybizz at provisioning via the Anvil IDE. It never changes after provisioning. Used exclusively inside `vault_service.py` to encrypt and decrypt Vault entries.

No other code ever calls `anvil.secrets.get_secret()` directly. No client credential, no API key, no integration token ever goes in Anvil Secrets.

### The Vault — All Client Credentials
An application-level encrypted secrets store built into every client instance. Holds all client API keys and credentials. Values are encrypted using Fernet symmetric encryption before storage in the `vault` Data Table.

All integration server modules retrieve client credentials by calling `get_vault_secret(name)` from `vault_service.py`. No integration module ever calls `anvil.secrets.get_secret()` for a client credential.

Access to the Vault is restricted to the Owner role and requires TOTP step-up authentication (Google Authenticator) on every open. No grace period. An email notification is sent to the Owner on every Vault access.

**See:** `3-payment-security-boundary-vault.md`

---

## 2. Payment Security Boundary

The SettingsForm does not persist payment gateway secret keys to the `payment_config` table. Secret keys are handled entirely by the Vault.

The enforcement pattern uses `assert` to hard-fail if any payment secret column appears in the data being saved:

```python
_PAYMENT_SECRET_COLUMNS = frozenset({
    'stripe_secret_key', 'paystack_secret_key', 'paypal_secret'
})
assert not (_PAYMENT_SECRET_COLUMNS & set(data.keys())), (
    f"Secret key columns must not be saved here: "
    f"{_PAYMENT_SECRET_COLUMNS & set(data.keys())}"
)
```

The `AssertionError` must be re-raised before any broader exception handler.

### Secret Masking
`get_payment_config()` returns `'***'` for any secret key field. `get_email_config()` returns `'***'` for SMTP credentials. Client code never receives the actual secret value. Client save methods must not include secret key fields in the outbound dictionary even with masked `'***'` values.

---

## 3. RBAC Architecture

Five roles govern all access:

| Role | Access |
|---|---|
| Owner | Full access including Vault, financial data, user management. One per instance. |
| Manager | Operational management — bookings, customers, marketing, reporting. No Vault or financial config. 0–3 per instance. |
| Admin | Booking and customer management. No financial reporting, settings, or Vault. 0–10 per instance. |
| Staff | Own calendar and bookings only. No access to other staff data or settings. 0–20 per instance. |
| Customer | Own bookings, invoices, and account in the customer portal. Unlimited per instance. |

Role enforcement is server-side via `@require_role` or `@require_permission` decorators. Client-side navigation visibility is a UX convenience only.

---

## 4. Data Table Permissions

All Data Tables are set to "No access" for client code without exception. All data access is through server functions. Direct client access bypasses `instance_id` filtering, authentication checks, and input validation.

---

## 5. Rate Limiting

Rate limits are enforced via the `rate_limits` Data Table, not in-memory. The Data Table implementation survives server restarts and works across Anvil's multi-server environment.

- Unauthenticated requests: 10 per minute per IP
- Authenticated requests: 100 per minute per user
- Background task cleans up rate limit records older than 24 hours

---

## 6. Authentication Security

Password requirements: minimum 8 characters, at least one uppercase, one lowercase, one number. Rate limiting applies to all unauthenticated requests. Login lockout applies after repeated failures. Session inactivity timeout: 30 minutes.

---

## 7. Audit Logging

All significant actions are logged with actor, timestamp, and description:
- Authentication events (login, logout, failed login, lockout)
- Data modification events (create, update, delete) with before/after values
- Payment events
- Permission changes
- All Vault access

The audit log is visible to Owner and Manager only. Retention: two years.

---

## 8. Management Token Security (ADR-033)

Mybizz_management communicates with client instances via bidirectional HTTP endpoints secured with per-client Bearer tokens.

### Token Types

| Direction | Token type | Storage |
|---|---|---|
| Mybizz → Client | Per-client Bearer token (unique per instance) | Mybizz_management client registry (encrypted); client instance management config |
| Client → Mybizz | Shared event push token | Each client instance Vault |

### Security Properties

- **Per-client isolation:** Each client instance has a unique management Bearer token. Compromise of one instance does not affect others.
- **Encrypted storage:** Management tokens are stored encrypted in Mybizz_management's client registry.
- **Server-side validation:** All management endpoints validate the Bearer token before processing. Invalid tokens receive HTTP 403.
- **Non-blocking event push:** Client-to-Mybizz event push calls are non-blocking — failure to reach Mybizz_management does not block client instance processing.

### Token Lifecycle

- **Generation:** Unique token generated at provisioning (see `client-activation-runbook.md`)
- **Storage:** Stored in both Mybizz_management registry (encrypted) and client instance config
- **Rotation:** Not automated in V1 — manual rotation via re-provisioning if compromise suspected
- **Revocation:** Delete from Mybizz_management registry and client instance config

---

## 8. Regulatory Compliance

### POPIA (South Africa) — Mandatory
Mybizz is based in South Africa. Registration with the Information Regulator as a Responsible Party is required after launch. Data subject rights (access, correction, deletion, objection) must be implemented.

### GDPR (European Union)
Applies if the platform serves EU customers. Data subject rights implementation (export and anonymisation functions) satisfies both GDPR and POPIA.

### PCI DSS
Mybizz stores only payment tokens — never raw card numbers. Falls into SAQ A (lowest scope). Annual Self-Assessment Questionnaire must be completed.

### Data Retention
- Contacts: retained indefinitely unless deletion requested
- Transactions and invoices: 7 years for tax compliance
- Audit logs: 2 years
- Email logs: 90 days
- Completed tasks: auto-deleted after 90 days

---

*End of file*
