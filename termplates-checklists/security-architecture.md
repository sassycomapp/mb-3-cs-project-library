# Mybizz CS — Security & Secrets Architecture

**Related ADRs:** ADR-003 (Vault), ADR-028 (Client Instance Architecture), ADR-033
(Mybizz_management Visibility)

**Note:** Reusable security patterns (the two-level secrets model, secret enforcement/masking,
RBAC/data access pattern, rate limiting) live in `specifications/spec-security.md`. The
regulatory baseline (POPIA, GDPR, PCI DSS, standard retention periods) is Mybizz-wide — see
`specifications/regulatory-compliance-baseline.md`. This document contains only what is specific
to this app's actual architecture.

---

## 1. RBAC — This App's Role Limits

Five roles, per the standard RBAC pattern (see `specifications/spec-security.md`). This app's
specific per-instance limits:

| Role | Access | Instance Limit |
|---|---|---|
| Owner | Full access including Vault, financial data, user management | 1 per instance |
| Manager | Operational management — bookings, customers, marketing, reporting. No Vault or financial config | 0–3 per instance |
| Admin | Booking and customer management. No financial reporting, settings, or Vault | 0–10 per instance |
| Staff | Own calendar and bookings only | 0–20 per instance |
| Customer | Own bookings, invoices, account in the customer portal | Unlimited |

## 2. Mybizz_management Token Security (ADR-033)

Mybizz_management communicates with client instances via bidirectional HTTP endpoints secured
with per-client Bearer tokens. This entire section is specific to this app's multi-tenant
architecture — there is no reusable pattern here beyond the general "use Bearer tokens for
service-to-service auth" idea, which isn't specific enough to be worth extracting.

### Token Types

| Direction | Token Type | Storage |
|---|---|---|
| Mybizz → Client | Per-client Bearer token (unique per instance) | Mybizz_management client registry (encrypted); client instance management config |
| Client → Mybizz | Shared event push token | Each client instance's Vault |

### Security Properties

- **Per-client isolation:** a unique management Bearer token per client instance — compromise of
  one instance does not affect others.
- **Encrypted storage:** management tokens stored encrypted in Mybizz_management's client
  registry.
- **Server-side validation:** all management endpoints validate the Bearer token before
  processing; invalid tokens receive HTTP 403.
- **Non-blocking event push:** client-to-Mybizz event push calls are non-blocking — failure to
  reach Mybizz_management does not block client instance processing.

### Token Lifecycle

- **Generation:** unique token generated at provisioning (see `client-activation-runbook.md` in
  `specifications/`).
- **Storage:** stored in both the Mybizz_management registry (encrypted) and the client
  instance's own config.
- **Rotation:** not automated in V1 — manual rotation via re-provisioning if compromise is
  suspected.
- **Revocation:** delete from both the Mybizz_management registry and the client instance config.

## 3. Regulatory Compliance

The regulatory baseline (POPIA, GDPR, PCI DSS, standard retention periods) is Mybizz-wide — see
`specifications/regulatory-compliance-baseline.md`. This app's specific facts:

- **PCI DSS applies to this app** — `mb-3-cs` processes payments via Stripe and Paystack, placing
  it in SAQ A scope per the Mybizz-wide standard.
- **GDPR applies conditionally** — if this app serves EU customers (determined by actual client
  base, not fixed at build time).
- No data categories beyond the Mybizz-wide retention defaults are tracked by this app.

---

*Security & Secrets Architecture v3.0 — Regulatory content extracted to
`specifications/regulatory-compliance-baseline.md`. This document contains only this app's RBAC
limits and Mybizz_management token architecture.*
