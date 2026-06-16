# Batch 2: Anvil Designer Changes — Specification

**Date:** 2026-06-14
**Source:** CEO Review audit (ceo-review-fix-plan.md)
**Applies to:** mb-3-cs Anvil app (Anvil Designer)
**Rule:** No loose ends. Every column name, type, and link target is specified.

---

## Overview

The CEO review found 11 schema gaps in the live Anvil app database. These changes must be made in the Anvil Designer — they cannot be made by editing anvil.yaml directly.

- **5 existing tables** need columns added or fixed
- **5 new tables** need to be created
- **1 table** needs a type correction

All changes are additive. No columns are removed or renamed.

---

## Pre-Requisites

- Access to the mb-3-cs app in the Anvil IDE
- Current schema backed up (Anvil auto-saves, but confirm before starting)
- authoritative-schema.md available as reference (already updated by Batch 1)

---

## Change 1: `bookings` — Add 12 columns

**Priority:** CRITICAL
**Why:** The bookings table currently has only 4 columns (booking_id, contact_id, service_id, resource_id). It cannot store status, dates, pricing, payment info, or timestamps. The booking system is non-functional without these.

| # | Column name | Type | Link target | Notes |
|---|------------|------|-------------|-------|
| 1 | `booking_type` | string | — | "appointment" or "service" |
| 2 | `status` | string | — | "PENDING", "CONFIRMED", "COMPLETED", "CANCELLED" |
| 3 | `start_datetime` | datetime | — | UTC |
| 4 | `end_datetime` | datetime | — | UTC |
| 5 | `total_price` | number | — | |
| 6 | `payment_status` | string | — | "unpaid", "deposit_paid", "paid", "refunded" |
| 7 | `payment_id` | string | — | Gateway transaction ID |
| 8 | `special_requests` | string | — | |
| 9 | `metadata` | simpleObject | — | Per booking_type schema (ADR-035) |
| 10 | `booking_number` | string | — | Human-readable (e.g., "BK-2026-001") |
| 11 | `created_at` | datetime | — | UTC |
| 12 | `updated_at` | datetime | — | UTC |

**After adding:** Verify the table has 16 columns total.

---

## Change 2: `tasks` — Add 1 column

**Priority:** CRITICAL
**Why:** Multi-tenant isolation is broken. Without `instance_id`, tasks from different clients could leak across instances.

| # | Column name | Type | Link target | Notes |
|---|------------|------|-------------|-------|
| 1 | `instance_id` | link_single | users | Client identifier |

**After adding:** Verify the table has 12 columns total.

---

## Change 3: `email_campaigns` — Add 1 column

**Priority:** HIGH
**Why:** Campaign analytics cannot track email clicks without this field.

| # | Column name | Type | Link target | Notes |
|---|------------|------|-------------|-------|
| 1 | `clicks` | number | — | Email click count |

**After adding:** Verify the table has 13 columns total.

---

## Change 4: `email_templates` — Fix 1 column type

**Priority:** MEDIUM
**Why:** `is_active` is stored as string but should be bool. Filtering and conditional logic will fail on string comparison.

| Column | Current type | Correct type | Action |
|--------|-------------|-------------|--------|
| `is_active` | string | bool | Change type in Designer |

**Note:** If any existing rows have string values like "true" or "false", they will need to be converted to actual boolean values after the type change.

---

## Change 5: `blog_posts` — Add 2 columns

**Priority:** MEDIUM
**Why:** DESIGN.md expects video embed fields for blog posts. The VideoDisplayComponent requires these.

| # | Column name | Type | Link target | Notes |
|---|------------|------|-------------|-------|
| 1 | `video_url` | string | — | YouTube URL for embedded video (V1: YouTube only) |
| 2 | `video_type` | string | — | Video source type (V1: "youtube") |

**After adding:** Verify the table has 21 columns total.

---

## Change 6: Create `daily_summary` table

**Priority:** HIGH
**ADR source:** ADR-040 (Data Access Patterns)
**Why:** Pre-computed daily analytics for dashboard. Populated by daily background task.

| # | Column name | Type | Link target | Notes |
|---|------------|------|-------------|-------|
| 1 | `instance_id` | link_single | users | Client identifier |
| 2 | `date` | date | — | Summary date |
| 3 | `total_revenue` | number | — | Revenue for the day |
| 4 | `revenue_by_service` | simpleObject | — | JSON breakdown by service |
| 5 | `booking_count` | number | — | Confirmed bookings |
| 6 | `new_contacts` | number | — | New contacts created |
| 7 | `created_at` | datetime | — | UTC |

**Table settings:** `client: none`, `server: full`

---

## Change 7: Create `service_performance` table

**Priority:** HIGH
**ADR source:** ADR-040 (Data Access Patterns)
**Why:** Pre-computed per-service analytics for reporting.

| # | Column name | Type | Link target | Notes |
|---|------------|------|-------------|-------|
| 1 | `instance_id` | link_single | users | Client identifier |
| 2 | `service_id` | link_single | services | |
| 3 | `date` | date | — | Summary date |
| 4 | `revenue` | number | — | Revenue for the service |
| 5 | `booking_count` | number | — | Bookings for the service |
| 6 | `created_at` | datetime | — | UTC |

**Table settings:** `client: none`, `server: full`

---

## Change 8: Create `provider_performance` table

**Priority:** MEDIUM
**ADR source:** ADR-040 (Data Access Patterns)
**Why:** Pre-computed per-provider analytics for reporting.

| # | Column name | Type | Link target | Notes |
|---|------------|------|-------------|-------|
| 1 | `instance_id` | link_single | users | Client identifier |
| 2 | `provider_id` | link_single | users | Staff provider |
| 3 | `date` | date | — | Summary date |
| 4 | `revenue` | number | — | Revenue for the provider |
| 5 | `booking_count` | number | — | Bookings for the provider |
| 6 | `created_at` | datetime | — | UTC |

**Table settings:** `client: none`, `server: full`

---

## Change 9: Create `campaign_metrics` table

**Priority:** MEDIUM
**ADR source:** ADR-040 (Data Access Patterns)
**Why:** Pre-computed campaign analytics for marketing dashboard.

| # | Column name | Type | Link target | Notes |
|---|------------|------|-------------|-------|
| 1 | `instance_id` | link_single | users | Client identifier |
| 2 | `campaign_id` | link_single | email_campaigns | |
| 3 | `date` | date | — | Summary date |
| 4 | `opens` | number | — | Email opens |
| 5 | `clicks` | number | — | Email clicks |
| 6 | `conversions` | number | — | Conversions |
| 7 | `created_at` | datetime | — | UTC |

**Table settings:** `client: none`, `server: full`

---

## Change 10: Create `health_log` table

**Priority:** MEDIUM
**ADR source:** ADR-036 (Real-Time and Background Tasks)
**Why:** Instance health monitoring. Written every 15 minutes by health heartbeat background task.

| # | Column name | Type | Link target | Notes |
|---|------------|------|-------------|-------|
| 1 | `instance_id` | link_single | users | Client identifier |
| 2 | `timestamp` | datetime | — | UTC — when health check ran |
| 3 | `status` | string | — | "healthy", "degraded", "unhealthy" |
| 4 | `metrics` | simpleObject | — | JSON health metrics |
| 5 | `created_at` | datetime | — | UTC |

**Table settings:** `client: none`, `server: full`

---

## Verification Checklist

After all changes are applied in Anvil Designer:

- [ ] `bookings` has 16 columns (4 existing + 12 new)
- [ ] `tasks` has 12 columns (11 existing + 1 new `instance_id`)
- [ ] `email_campaigns` has 13 columns (12 existing + 1 new `clicks`)
- [ ] `email_templates.is_active` is type bool (not string)
- [ ] `blog_posts` has 21 columns (19 existing + 2 new)
- [ ] `daily_summary` exists with 7 columns
- [ ] `service_performance` exists with 6 columns
- [ ] `provider_performance` exists with 6 columns
- [ ] `campaign_metrics` exists with 7 columns
- [ ] `health_log` exists with 5 columns
- [ ] All new tables have `client: none` and `server: full`
- [ ] All link columns point to correct target tables
- [ ] Total table count is 43 (38 existing + 5 new)

---

## Post-Change Sync

After applying these changes in Anvil Designer:

1. Pull the updated anvil.yaml into the local repo (Anvil → GitHub sync)
2. Verify anvil.yaml reflects all new tables and columns
3. Run Batch 1 `/build` prompt (if not already done) to align authoritative-schema.md
4. Run `/plan-eng-review` to validate schema consistency

---

*End of Batch 2 — Anvil Designer Specification*
