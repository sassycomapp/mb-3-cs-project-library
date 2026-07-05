# PDLF Standards Library — Security Specification

**Location:** `C:\pdlf\standards-library\spec-security.md`
**Scope:** Project-agnostic security patterns. A project's specific role limits, regulatory
posture, and implementation details belong in that project's own canonical security document.

---

## 1. Two-Level Secrets Model

- **Platform Secrets** (e.g. Anvil Secrets) holds exactly one item: a master encryption key, set
  at provisioning, never changed, used only inside a dedicated vault-service module. No other
  code accesses the platform secrets facility directly.
- **The Vault** (an application-level encrypted Data Table) holds every other credential.
  Retrieved only via a `get_vault_secret(name)` function, never accessed directly by integration
  modules.
- **Access control:** restrict Vault access to the Owner role, require step-up authentication
  (e.g. TOTP) on every open with no grace period, and send a notification on every access.

## 2. Secret Enforcement and Masking

```python
_PAYMENT_SECRET_COLUMNS = frozenset({
    'stripe_secret_key', 'paystack_secret_key', 'paypal_secret'
})
assert not (_PAYMENT_SECRET_COLUMNS & set(data.keys())), (
    f"Secret key columns must not be saved here: "
    f"{_PAYMENT_SECRET_COLUMNS & set(data.keys())}"
)
```

`AssertionError` must always be re-raised before any broader exception handler. Any function
returning configuration containing a secret field returns `'***'`, never the real value.

## 3. RBAC and Data Access

- Every server function operating on application data carries an RBAC decorator.
- Role enforcement is always server-side. Client-side navigation visibility is a UX convenience
  only.
- All Data Tables are set to "No access" for client code — all data access goes through server
  functions.

## 4. Rate Limiting

Enforce via a Data Table, not an in-memory counter, so limits survive server restarts and work
across a multi-server environment.

## 5. Shared Numeric Security Standards

| Standard | Value |
|---|---|
| Password minimum length | 8 characters |
| Password complexity | At least one uppercase, one lowercase, one number |
| Rate limit — unauthenticated | 10 requests/minute/IP |
| Rate limit — authenticated | 100 requests/minute/user |
| Session inactivity timeout | 30 minutes |
| Audit log retention | 2 years |

---

*Security Specification v1.0. Moved from spec-anvil-platform-standards.md Sections 10–14 into
its own file. A project's Vault implementation details go in that project's own canonical Vault
document (e.g. vault-system.md); its role limits and regulatory posture go in its own canonical
security document.*
