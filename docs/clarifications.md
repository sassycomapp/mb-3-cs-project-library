# Mybizz CS — Schema & Scaffolding Clarifications

**Date:** 2026-05-07  
**Status:** Anomalies Resolved

---

## Summary of Changes

### 1. Authoritative Schema Consolidated ✅

**Problem:** Schema was fragmented across `database-schema.md`, `scaffold-spec.md`, and `build-plan.md` with inconsistent details.

**Solution:** Created `authoritative-schema.md` — single source of truth with all 43 tables fully specified.

**Location:** `docs/authoritative-schema.md`

---

### 2. PayPal Removed from V1 ✅

**Problem:** `scaffold-spec.md` and `database-schema.md` included PayPal references despite V1/V2 split decision.

**Solution:** 
- Removed `paypal_service.py` from server packages
- Removed `paypal_client_id`, `paypal_connected`, `paypal_connected_at` from `payment_config` table
- Added V2-only notes to all PayPal references

**Status:** V1 includes Stripe + Paystack only. PayPal deferred to V2.

---

### 3. .py Requirement Removed ✅

**Problem:** `scaffold-workflow.md` required server files to end with `.py`, but Anvil automatically appends `.py` — developers should NOT include it when creating folders in Anvil Designer.

**Solution:** 
- Updated `scaffold-spec.md` server package structure to show folder names WITHOUT `.py`
- Added explicit note: "Anvil automatically appends `.py` to server modules. Do NOT include `.py` in folder names when creating in Anvil Designer."

**Correct workflow:**
1. In Anvil Designer: Create folder `server_payments` (NOT `server_payments.py`)
2. Anvil treats it as Python module
3. Inside folder: Create file `stripe_service` (NOT `stripe_service.py`)
4. Anvil saves as `stripe_service.py`

---

## Authoritative Schema Reference

**43 Tables Total:**

| Group | Tables | Files |
|-------|--------|-------|
| **Core & Configuration** | 10 | `authoritative-schema.md` sections 1.1-1.10 |
| **Bookings** | 6 | `authoritative-schema.md` sections 2.1-2.6 |
| **CRM** | 9 | `authoritative-schema.md` sections 3.1-3.9 |
| **Email** | 3 | `authoritative-schema.md` sections 4.1-4.3 |
| **Content** | 4 | `authoritative-schema.md` sections 5.1-5.4 |
| **Finance** | 3 | `authoritative-schema.md` sections 6.1-6.3 |

**Use this document for all schema work.** Do not reference fragmented sources.

---

## Scaffolding Workflow (Updated)

### Client Code (Forms)
1. Create folder in `client_code/` (e.g., `auth/`)
2. Inside folder, create form folder (e.g., `LoginForm/`)
3. In Designer: `LoginForm/form_template.yaml` (drag/drop components)
4. In Code: `LoginForm/__init__.py` (write logic)
5. **No `.py` needed for folders** — folders are containers

### Server Code (Modules)
1. Create folder in `server_code/` (e.g., `server_payments/`)
2. **Do NOT add `.py` to folder name** — Anvil adds it automatically
3. Inside folder, create module (e.g., `stripe_service`)
4. **Do NOT add `.py` to module name** — Anvil adds it automatically
5. Anvil saves as `server_payments/stripe_service.py`

---

## Files to Reference

| Task | Document |
|------|----------|
| **Database schema** | `docs/authoritative-schema.md` |
| **Package structure** | `docs/scaffold-spec.md` (updated) |
| **Scaffolding workflow** | `docs/scaffold-workflow.md` (updated) |
| **Build plan** | `implementation/build-plan.md` |
| **Architecture** | `docs/architecture.md` |

---

## Anomalies Resolved

| Issue | Status | Resolution |
|-------|--------|------------|
| Fragmented schema | ✅ Fixed | `authoritative-schema.md` created |
| PayPal in V1 | ✅ Fixed | Removed from all V1 docs |
| .py file extension confusion | ✅ Fixed | Removed requirement, added clear guidance |

---

## Next Steps

1. **Use `authoritative-schema.md`** as your single source of truth for all table definitions
2. **Follow updated scaffolding workflow** — no `.py` in Anvil Designer
3. **Verify PayPal removed** — all server packages should only have `stripe_service` and `paystack_service`
4. **Proceed with Phase 1 implementation** — Foundation (Auth, Vault, Navigation, Settings)

---

*End of file — clarifications.md*
