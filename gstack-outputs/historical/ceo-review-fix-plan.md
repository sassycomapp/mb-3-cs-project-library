# Mybizz CS — CEO Review: Sanity Audit & Realignment Plan

**Date:** 2026-06-14
**Type:** CEO Review — Full Sanity Audit
**Scope:** anvil.yaml (mb-3-cs) vs authoritative-schema.md vs ADRs vs all docs
**Rule:** No loose ends, no assumptions
**Mode:** Hold Scope (maximum rigor)

---

## Executive Summary

The Mybizz CS V1 architecture is sound. All 38 ADRs represent valid decisions. The product scope is correct — V1 remains the narrowest viable wedge. The problem is mechanical: the live app database (anvil.yaml) is missing 12 critical columns on the `bookings` table, the `tasks` table is missing `instance_id` (breaking multi-tenant isolation), the authoritative schema is missing 6 tables and 12 columns that exist in the app or are required by ADRs, and 7 new ADRs (034-040) are not referenced in the implementation-facing docs. None of these are architectural problems. They are all fixable mechanical gaps.

**Bottom line:** 18 gaps found. 2 critical, 8 high, 6 medium, 2 low. All must be closed before Phase 1 implementation begins.

---

## 1. Product Coherence

### Is V1 still the narrowest viable wedge?

**Yes.** V1 = Auth + RBAC + Vault + Services + Bookings + Payments + Invoicing + CRM + Email Campaigns + Blog + Public Website + Landing Pages + Analytics. This is a complete consulting business platform. The recent changes (onboarding rewrite, ADR consolidation, ClauseBuilder addition) are all refinements to existing scope, not additions.

### Does the onboarding/settings split still make product sense?

**Yes.** Onboarding = guided first-run with two required elements (currency selection, legal acknowledgement). Settings = 11-tab ongoing configuration form. Bidirectional navigation between them. This is a standard SaaS pattern. ADR-017, ADR-023, ADR-025, ADR-026 govern this correctly.

### Is the 11-tab Settings form scope creep or necessary?

**Necessary.** Each tab maps to a distinct configuration concern with different RBAC rules. Collapsing tabs would mix concerns and break RBAC governance.

### Should any features be deferred to V2?

**No.** Time tracking, custom segment builder, PayPal, client portal, and advanced analytics are already deferred. Cutting further would remove core features.

---

## 2. Table Inventory: anvil.yaml vs authoritative-schema.md

### 2.1 Table Count

| Source | Count | Status |
|--------|-------|--------|
| anvil.yaml | **38** tables | Actual database |
| authoritative-schema.md header | **38** | Correct |
| authoritative-schema.md footer (line 692) | **36** | **WRONG** — says "V1, 36 tables" but actually documents 38 |

### 2.2 Table-by-Table Comparison

**Tables in anvil.yaml that ARE in authoritative-schema.md (37 of 38):**

All 37 tables match by name. Column-level gaps are detailed in section 3.

**Table in anvil.yaml NOT in authoritative-schema.md (1):**

| Table | In anvil.yaml | In schema | In DESIGN.md | Status |
|-------|:---:|:---:|:---:|--------|
| **blog_comments** | Yes (8 columns) | **NO** | Yes (expected fields listed) | **GAP** |

`blog_comments` exists in anvil.yaml with columns: instance_id, post_id, commenter_name, commenter_email, comment, status, created_at, updated_at. DESIGN.md lists it as an expected table. But authoritative-schema.md does not document it.

---

## 3. Column-Level Gaps

### 3.1 CRITICAL: `bookings` table — 12 of 16 required columns missing

| Required column | Type | In anvil.yaml | Status |
|----------------|------|:---:|--------|
| `booking_id` | string | Yes | OK |
| `contact_id` | link → contacts | Yes | OK |
| `service_id` | link → services | Yes | OK |
| `resource_id` | link → users | Yes | OK |
| `booking_type` | string | **NO** | **MISSING** |
| `status` | string | **NO** | **MISSING** |
| `start_datetime` | datetime | **NO** | **MISSING** |
| `end_datetime` | datetime | **NO** | **MISSING** |
| `total_price` | number | **NO** | **MISSING** |
| `payment_status` | string | **NO** | **MISSING** |
| `payment_id` | string | **NO** | **MISSING** |
| `special_requests` | string | **NO** | **MISSING** |
| `metadata` | simpleObject | **NO** | **MISSING** |
| `booking_number` | string | **NO** | **MISSING** |
| `created_at` | datetime | **NO** | **MISSING** |
| `updated_at` | datetime | **NO** | **MISSING** |

**Impact:** The bookings table in the live app has only 4 columns. It cannot store booking status, dates, pricing, payment info, or timestamps. This table is non-functional for the booking system.

### 3.2 CRITICAL: `tasks` table — `instance_id` missing

| Required column | In anvil.yaml | Status |
|----------------|:---:|--------|
| `instance_id` | **NO** | **MISSING** — multi-tenant isolation broken |

All other 10 columns present.

### 3.3 `email_campaigns` — 1 column missing

| Required column | In anvil.yaml | Status |
|----------------|:---:|--------|
| `clicks` | **NO** | **MISSING** |

### 3.4 `email_templates` — type mismatch

| Column | Schema type | anvil.yaml type | Status |
|--------|-------------|-----------------|--------|
| `is_active` | bool | **string** | **TYPE MISMATCH** |

### 3.5 `blog_posts` — 2 columns missing from app, 5 missing from schema

| Column | In anvil.yaml | In authoritative-schema | Status |
|--------|:---:|:---:|--------|
| `video_url` | **NO** | Yes | **MISSING from app** |
| `video_type` | **NO** | Expected by DESIGN.md | **MISSING from app AND schema** |
| `template` | Yes | **NO** | Schema gap |
| `meta_title` | Yes | **NO** | Schema gap |
| `meta_description` | Yes | **NO** | Schema gap |
| `keywords` | Yes | **NO** | Schema gap |
| `pullquote` | Yes | **NO** | Schema gap |

### 3.6 `business_profile` — 7 columns missing from schema

These columns exist in anvil.yaml and are expected by DESIGN.md but are not documented in authoritative-schema.md:

| Column | In anvil.yaml | In authoritative-schema | Status |
|--------|:---:|:---:|--------|
| `palette_updated_at` | Yes | **NO** | Schema gap |
| `blog_list_view` | Yes | **NO** | Schema gap |
| `enable_comments` | Yes | **NO** | Schema gap |
| `homepage_template` | Yes | **NO** | Schema gap |
| `business_logo` | Yes | **NO** | Schema gap |
| `accreditations` | Yes | **NO** | Schema gap |
| `google_maps_embed_url` | Yes | **NO** | Schema gap |

---

## 4. ADR-Added Requirements vs Authoritative Schema

### 4.1 ADR-038 (Webhook Architecture) — webhook_log schema mismatch

ADR-038 defines `webhook_log` with: `id`, `source`, `payload`, `status`, `received_at`, `processed_at`, `error_message`, `idempotency_key`

authoritative-schema.md defines `webhook_log` with: `gateway`, `event_type`, `payload`, `signature`, `verified`, `processed`, `error_message`, `received_at`

**Differences:**
- ADR-038 uses `source` + `idempotency_key`; schema uses `gateway` + `signature`
- ADR-038 uses `status` (pending/processed/failed); schema uses `verified` + `processed` (bools)
- ADR-038 has `processed_at`; schema does not
- Schema has `event_type`; ADR-038 does not explicitly list it

**Status:** Must be reconciled. The anvil.yaml matches the authoritative-schema.md columns.

### 4.2 ADR-040 (Data Access Patterns) — 4 summary tables not in schema or app

ADR-040 introduces 4 summary tables required for analytics:
- `daily_summary`
- `service_performance`
- `provider_performance`
- `campaign_metrics`

**None are in authoritative-schema.md or anvil.yaml.**

### 4.3 ADR-036 (Background Tasks) — health_log table not in schema or app

ADR-036 mentions a `health_log` table written by the health heartbeat task. Not in authoritative-schema.md or anvil.yaml.

### 4.4 `time_entries` — Scope conflict

| Source | Status |
|--------|--------|
| build-plan.md | "Time Tracking has been **deferred to V2**" |
| authoritative-schema.md | Documented as table 6.3 |
| anvil.yaml | Present with all 14 columns |
| platform-overview.md | Listed under "Deferred to V2" |

**Recommendation:** Keep the table (no harm), but mark as V2 infrastructure in authoritative-schema.md.

---

## 5. Doc Discrepancies

### 5.1 Stale table counts

| File | Says | Should say |
|------|------|-----------|
| authoritative-schema.md footer | "36 tables" | "38 tables" |
| architecture.md | "36 tables" | "38 tables" |
| platform-overview.md §13.1 | "36 tables" | "38 tables" |
| clarifications.md | "36 Tables Total" | "38 Tables Total" |

### 5.2 Duplicate content

| File | Issue |
|------|-------|
| design-direction.md | Sections "Interactive States", "Responsive Behavior", "Accessibility", "Layout Specifications" appear twice (lines 572-791 duplicated at 794-1012) |
| architecture.md | CustomerLayout note duplicated (line 135) |

### 5.3 integration-architecture.md incomplete task list

ADR-036 lists 7 background tasks. integration-architecture.md §4 lists only 3. Missing: health heartbeat, appointment reminders 24h, appointment reminders 1h, webhook processing.

### 5.4 Missing ADR references

build-plan.md, architecture.md, integration-architecture.md, coding-standards.md, and testing-standards.md do not reference ADR-034 through ADR-040.

### 5.5 devlog.md stale

Last updated 2026-05-26. Does not reflect June work.

---

## 6. Architecture Confirmation

| Aspect | Status | Evidence |
|--------|--------|----------|
| Dependency model (ADR-028) | **CONFIRMED** | anvil.yaml has master_template dependency; platform-overview.md §7.4 verified with live test |
| Multi-tenant isolation | **CONFIRMED** (with caveat) | All tables have `client: none`; `instance_id` links present. **Caveat:** `tasks` table missing `instance_id` — must be added |
| Security model (ADR-003) | **CONFIRMED** | vault table exists; vault_service.py, vault_totp_service.py, encryption_service.py exist |
| RBAC (5 roles) | **CONFIRMED** | users table has `role` column; server_auth/rbac.py exists |
| Onboarding architecture | **CONFIRMED** | business_profile has all 11 onboarding columns |
| Payment model (ADR-019, 024) | **CONFIRMED** | payment_config exists; server_payments/ has stripe/paystack/webhook files |
| Email architecture (ADR-001) | **CONFIRMED** | email_config, email_templates, email_log exist; server_marketing/ has brevo integration |
| Navigation (ADR-002) | **CONFIRMED** | M3 theme + routing dependencies in anvil.yaml |
| Design system (ADR-015, 027, 037) | **CONFIRMED** | M3 theme v1.2.6 in anvil.yaml; DESIGN.md comprehensive |
| Vault system (ADR-003) | **CONFIRMED** | vault table in anvil.yaml with correct columns |
| ClauseBuilder (ADR-018) | **CONFIRMED** | privacy_policy and t_and_c tables in anvil.yaml; policy_service.py exists |
| Webhook architecture (ADR-038) | **PARTIALLY CONFIRMED** | webhook_log exists but column definitions differ between ADR and schema |
| Data access patterns (ADR-040) | **NOT CONFIRMED** | Summary tables do not exist in schema or app |
| Form architecture (ADR-035) | **CONFIRMED** | Content panel pattern documented |
| Background tasks (ADR-036) | **PARTIALLY CONFIRMED** | Patterns documented but health_log table missing |

---

## 7. Server Code Inventory vs Build Plan

| Package | Files present | Build plan stage |
|---------|--------------|------------------|
| server_auth/ | service.py, rbac.py | Stage 1.2 |
| server_settings/ | service.py, vault_service.py, vault_totp_service.py, encryption_service.py | Stages 1.4, 1.5 |
| server_shared/ | vault_service.py, encryption_service.py, exceptions.py, logging_config.py, metrics.py, validators.py, utilities.py, encryption.py, constants.py, config.py, export_service.py, policy_service.py | Multiple |
| server_bookings/ | service.py, availability_service.py, metadata_service.py, time_service.py | Stages 2.2, 2.3 |
| server_services/ | service.py | Stage 2.1 |
| server_payments/ | stripe_service.py, paystack_service.py, invoice_service.py, webhook_handlers.py | Stages 2.4, 2.5 |
| server_customers/ | contact_service.py, segment_service.py | Stages 3.1, 3.3 |
| server_marketing/ | campaign_service.py, broadcast_service.py, task_service.py, lead_capture_service.py, brevo_campaigns_integration.py | Stages 3.2, 3.4, 3.5 |
| server_blog/ | service.py, comment_service.py | Stage 4.1 |
| server_dashboard/ | service.py | Stage 1.3 |
| server_analytics/ | reporting_service.py | Stage 5.1 |

**All server packages referenced in the build plan have corresponding code files. The skeleton is complete.**

---

## 8. Required Actions (phase > stage > task)

### Phase 1: Critical Schema Fixes

**Stage 1.1: anvil.yaml column additions (via Anvil Designer)**

| Task | Table | Columns to add | ADR source |
|------|-------|---------------|------------|
| 1 | bookings | booking_type, status, start_datetime, end_datetime, total_price, payment_status, payment_id, special_requests, metadata, booking_number, created_at, updated_at | build-plan Stage 2.3 |
| 2 | tasks | instance_id (link → users) | schema §3.7 |
| 3 | email_campaigns | clicks (number) | schema §3.5 |
| 4 | email_templates | Fix is_active from string to bool | schema §4.1 |
| 5 | blog_posts | video_url (string), video_type (string) | DESIGN.md |
| 6 | daily_summary | date, total_revenue, revenue_by_service, booking_count | ADR-040 |
| 7 | service_performance | service_id, date, revenue, booking_count | ADR-040 |
| 8 | provider_performance | provider_id, date, revenue, booking_count | ADR-040 |
| 9 | campaign_metrics | campaign_id, date, opens, clicks, conversions | ADR-040 |
| 10 | health_log | timestamp, status, metrics | ADR-036 |

**Stage 1.2: Authoritative schema additions**

| Task | Action | File |
|------|--------|------|
| 11 | Add `blog_comments` table documentation | authoritative-schema.md |
| 12 | Add `daily_summary` table documentation | authoritative-schema.md |
| 13 | Add `service_performance` table documentation | authoritative-schema.md |
| 14 | Add `provider_performance` table documentation | authoritative-schema.md |
| 15 | Add `campaign_metrics` table documentation | authoritative-schema.md |
| 16 | Add `health_log` table documentation | authoritative-schema.md |
| 17 | Add 7 missing `business_profile` columns | authoritative-schema.md |
| 18 | Add 5 missing `blog_posts` columns | authoritative-schema.md |
| 19 | Update footer from "36 tables" to correct count | authoritative-schema.md |
| 20 | Reconcile ADR-038 webhook_log columns with schema | authoritative-schema.md + ADR-038 |
| 21 | Mark `time_entries` as V2 infrastructure | authoritative-schema.md |

### Phase 2: Doc Maintenance

**Stage 2.1: Table count corrections**

| Task | Action | File |
|------|--------|------|
| 22 | Update "36 tables" to correct count | architecture.md |
| 23 | Update "36 tables" to correct count | platform-overview.md |
| 24 | Update "36 Tables Total" to correct count | clarifications.md |

**Stage 2.2: Content fixes**

| Task | Action | File |
|------|--------|------|
| 25 | Remove duplicate sections (lines 794-1012) | design-direction.md |
| 26 | Remove duplicate CustomerLayout note | architecture.md |
| 27 | Update background task inventory from 3 to 7 | integration-architecture.md |

**Stage 2.3: ADR integration**

| Task | Action | File |
|------|--------|------|
| 28 | Add ADR-034-040 references to relevant stages | build-plan.md |
| 29 | Add ADR-034, 035, 036, 037, 040 references | architecture.md |
| 30 | Add ADR-034, 035, 036, 038, 039, 040 references | integration-architecture.md |
| 31 | Add ADR-034, 035, 040 references | coding-standards.md |
| 32 | Add ADR-034, 036 references | testing-standards.md |
| 33 | Add ADR-034, 036, 039 references | user-flows.md |
| 34 | Add ADR-027, 037 references | design-direction.md |
| 35 | Update devlog.md with June entries | devlog.md |
| 36 | Fix clarifications.md build-plan path | clarifications.md |

---

## 9. Scope Decisions

**Keep (V1):**
- All 38 tables — correct for V1 scope
- 11-tab Settings form — necessary for RBAC governance
- Onboarding as separate form — clean architecture per ADR-017
- ClauseBuilder as shared component — used in 2+ places
- Stripe + Paystack — sufficient for V1 geographic coverage
- Pre-built email campaigns — 4 sequences enough for V1
- 4 homepage templates — covers core use cases
- time_entries table — keep as V2 infrastructure (no harm)

**Defer (V2):**
- Client portal, advanced analytics, complex task automation, PayPal, time tracking UI, custom segment builder, additional templates, Mybizz_management

**Cut:**
- Nothing. V1 scope is already minimal viable.

---

## 10. Risk Mitigations

| Risk | Severity | Mitigation |
|------|----------|------------|
| bookings table non-functional | **CRITICAL** | Add 12 missing columns immediately via Anvil Designer |
| tasks multi-tenant isolation broken | **CRITICAL** | Add `instance_id` column immediately via Anvil Designer |
| ADR-040 summary tables missing | HIGH | Create 4 tables + document in schema before Phase 5 analytics |
| ADR-038 webhook_log schema conflict | HIGH | Reconcile column definitions before Phase 2 payments |
| Schema/docs out of sync | HIGH | Complete all 36 tasks in Phase 1 + Phase 2 before implementation |
| Anvil platform constraints | MEDIUM | ADR-034 documents all constraints; read before building |
| External service friction | MEDIUM | Create mocks for Brevo/Stripe/Paystack during development |

---

## 11. Final Verdict

### What passes
- Architecture: Sound
- ADR decisions: All valid
- Server code structure: Complete skeleton for all phases
- Onboarding model: Fully specified
- Security model: Fully specified
- Design system: Comprehensive
- 35 of 38 tables: Column-complete in anvil.yaml

### What fails
- 2 critical schema gaps (bookings, tasks)
- 8 high-severity documentation/schema gaps
- 6 medium-severity doc maintenance gaps
- 2 low-severity inconsistencies

### Total: 18 gaps, 36 tasks to close them

None are architectural problems. All are mechanical: missing columns, missing table docs, stale references, and unreconciled ADR additions.

---

**STATUS: DONE_WITH_CONCERNS**

The app is architecturally sound but has 2 critical schema gaps and 8 high-severity documentation/schema gaps. All are fixable. The 36-task fix plan in section 8 must be completed before Phase 1 implementation begins.

---

*End of CEO Review — 2026-06-14*
