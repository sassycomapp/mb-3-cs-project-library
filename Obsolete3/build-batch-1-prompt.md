# /build Prompt — Batch 1: Schema & Doc Alignment (Tasks 11-35)

**Session:** Single pass, 25 tasks, all file edits in `project-library/`
**Source of truth:** anvil.yaml (mb-3-cs), ADR-034 through ADR-040, DESIGN.md
**Rule:** No loose ends. Every edit must be traceable to an ADR, the authoritative schema, or anvil.yaml.

---

## Context

The CEO review audit found 18 gaps between the live app database (anvil.yaml), the authoritative schema, and the ADRs. This batch handles the 25 tasks that are file edits in `project-library/`. The remaining 11 tasks (Anvil Designer changes) are out of scope for this session.

The authoritative-schema.md currently documents 38 tables but the footer says "36 tables". After this session it will document 43 tables (38 existing + 5 new ADR-040/036 summary tables). All stale references to "36 tables" across the docs must be updated to "43 tables".

---

## Task List

### Group A: Authoritative Schema Additions (Tasks 11-21)

**File:** `docs/authoritative-schema.md`

#### Task 11 — Add `blog_comments` table
Add a new table section after `blog_posts` (§5.2). Source: anvil.yaml `blog_comments` definition.

```markdown
### [DONE] 5.2A `blog_comments`
Blog post comments.

| Field | Type | Notes |
|-------|------|-------|
| `instance_id` | link → users | Client identifier |
| `post_id` | link → blog_posts | Blog post |
| `commenter_name` | string | |
| `commenter_email` | string | |
| `comment` | string | Comment text |
| `status` | string | "pending", "approved", "rejected" |
| `created_at` | datetime | UTC |
| `updated_at` | datetime | UTC |
```

Update the Content Tables group count from 4 to 5.

#### Task 12 — Add `daily_summary` table
Add a new section under a new group "7. Summary Tables (5)" after Finance Tables. Source: ADR-040 §Summary Tables.

```markdown
## 7. Summary Tables (5)

### 7.1 `daily_summary`
Pre-computed daily analytics. Populated by daily background task (ADR-036, ADR-040).

| Field | Type | Notes |
|-------|------|-------|
| `instance_id` | link → users | Client identifier |
| `date` | date | Summary date |
| `total_revenue` | number | Revenue for the day |
| `revenue_by_service` | simpleObject | JSON breakdown by service |
| `booking_count` | number | Confirmed bookings |
| `new_contacts` | number | New contacts created |
| `created_at` | datetime | UTC |
```

#### Task 13 — Add `service_performance` table

```markdown
### 7.2 `service_performance`
Pre-computed per-service analytics. Populated by daily background task (ADR-040).

| Field | Type | Notes |
|-------|------|-------|
| `instance_id` | link → users | Client identifier |
| `service_id` | link → services | |
| `date` | date | Summary date |
| `revenue` | number | Revenue for the service |
| `booking_count` | number | Bookings for the service |
| `created_at` | datetime | UTC |
```

#### Task 14 — Add `provider_performance` table

```markdown
### 7.3 `provider_performance`
Pre-computed per-provider analytics. Populated by daily background task (ADR-040).

| Field | Type | Notes |
|-------|------|-------|
| `instance_id` | link → users | Client identifier |
| `provider_id` | link → users | Staff provider |
| `date` | date | Summary date |
| `revenue` | number | Revenue for the provider |
| `booking_count` | number | Bookings for the provider |
| `created_at` | datetime | UTC |
```

#### Task 15 — Add `campaign_metrics` table

```markdown
### 7.4 `campaign_metrics`
Pre-computed campaign analytics. Populated by hourly background task (ADR-040).

| Field | Type | Notes |
|-------|------|-------|
| `instance_id` | link → users | Client identifier |
| `campaign_id` | link → email_campaigns | |
| `date` | date | Summary date |
| `opens` | number | Email opens |
| `clicks` | number | Email clicks |
| `conversions` | number | Conversions |
| `created_at` | datetime | UTC |
```

#### Task 16 — Add `health_log` table

```markdown
### 7.5 `health_log`
Instance health metrics. Written every 15 minutes by health heartbeat background task (ADR-036).

| Field | Type | Notes |
|-------|------|-------|
| `instance_id` | link → users | Client identifier |
| `timestamp` | datetime | UTC — when health check ran |
| `status` | string | "healthy", "degraded", "unhealthy" |
| `metrics` | simpleObject | JSON health metrics |
| `created_at` | datetime | UTC |
```

Update the Table Groups Overview table to include row: `| **Summary** | 5 | Pre-computed analytics and health |`
Update the Total from 38 to 43.

#### Task 17 — Add 7 missing `business_profile` columns
Add these columns to the existing `business_profile` table section (§1.2), after the onboarding fields block:

```markdown
#### Design and website fields (added per DESIGN.md)

| Field | Type | Notes |
|-------|------|-------|
| `palette_updated_at` | datetime | UTC — when palette was last changed |
| `blog_list_view` | string | "tiled" or "list" — blog listing style |
| `enable_comments` | bool | Enable blog comments |
| `homepage_template` | string | "classic", "services", "booking", "minimalist" |
| `business_logo` | media | Separate from logo — used in specific contexts |
| `accreditations` | string | Business accreditations text |
| `google_maps_embed_url` | string | Google Maps embed URL for contact page |
```

#### Task 18 — Add 5 missing `blog_posts` columns
Add these columns to the existing `blog_posts` table section (§5.2), after the existing columns:

```markdown
| `template` | string | "classic_centered", "two_column", "full_width" |
| `meta_title` | string | SEO title override |
| `meta_description` | string | SEO meta description |
| `keywords` | string | SEO keywords (comma-separated) |
| `pullquote` | string | Featured pullquote text |
| `video_url` | string | YouTube URL for embedded video (V1: YouTube only) |
| `video_type` | string | Video source type (V1: "youtube") |
```

#### Task 19 — Update footer
Change line 692 from:
```
*End of file — authoritative-schema.md (V1, 36 tables)*
```
to:
```
*End of file — authoritative-schema.md (V1, 43 tables)*
```

#### Task 20 — Reconcile ADR-038 webhook_log columns
The current `webhook_log` table in authoritative-schema.md has: `gateway`, `event_type`, `payload`, `signature`, `verified`, `processed`, `error_message`, `received_at`.

ADR-038 defines: `id`, `source`, `payload`, `status`, `received_at`, `processed_at`, `error_message`, `idempotency_key`.

**Resolution:** The authoritative schema is the source of truth for the actual database. ADR-038 is a design document. Keep the authoritative schema columns as-is, but add a note to the `webhook_log` section:

```markdown
**Note:** ADR-038 (Webhook Architecture) defines an alternative column set for this table. The columns above represent the actual implementation. The ADR's `source` maps to `gateway`, `status` maps to `verified` + `processed`, and `idempotency_key` is handled via `event_type` + `gateway` dedup. See ADR-038 for the design rationale.
```

#### Task 21 — Mark `time_entries` as V2
Add a note to the `time_entries` table section (§6.3):

```markdown
**Note:** Time tracking is deferred to V2 per build-plan.md. This table exists as V2 infrastructure. No V1 UI or server code references it.
```

---

### Group B: Doc Maintenance (Tasks 22-27)

#### Task 22 — architecture.md
Replace all instances of "36 tables" with "43 tables" in `docs/architecture.md`.

#### Task 23 — platform-overview.md
Replace "36 tables" with "43 tables" in `docs/platform-overview.md` §13.1 (line 297).

#### Task 24 — clarifications.md
Replace "36 Tables Total" with "43 Tables Total" in `docs/clarifications.md` (line 51).

#### Task 25 — design-direction.md duplicate removal
In `docs/design-direction.md`, delete lines 794 through 1012. These are exact duplicates of lines 572-791 (Interactive States, Responsive Behavior, Accessibility, Layout Specifications sections).

#### Task 26 — architecture.md duplicate line
In `docs/architecture.md`, delete the duplicate line 135:
```
**Note:** The CustomerLayout is deferred to V2. Customers book via the public site and the business manages all bookings from the admin panel in V1.
```
(Keep line 133, delete line 135.)

#### Task 27 — integration-architecture.md background task inventory
In `docs/integration-architecture.md` §4 (Background Tasks), replace the current 3-task table with the full 7-task inventory from ADR-036:

```markdown
| Task | Schedule | Purpose |
|------|----------|---------|
| Health heartbeat | Every 15 minutes | Write health metrics to `health_log` table |
| Campaign enrollment | Hourly | Process active campaign enrollments, send next email |
| Appointment reminders (24h) | Hourly check | Send reminders for appointments in next 24 hours |
| Appointment reminders (1h) | Hourly check | Send reminders for appointments in next 1 hour |
| Lifecycle stage updates | Daily 02:00 | Recalculate contact lifecycle stages |
| Automated task generation | Daily 03:00 | Create follow-up tasks based on upcoming bookings |
| Webhook processing | On event | Defer webhook payload processing from HTTP handler |
```

---

### Group C: ADR Integration (Tasks 28-34)

For each file, add "See:" references to the relevant ADRs in the appropriate sections. Use the format:
```
**See:** ADR-0XX (Title)
```

#### Task 28 — build-plan.md
Add ADR references to relevant phase/stage sections:

- **Phase 1 (Foundation):** Add to Stage 1.3 notes: `**See:** ADR-035 (Form Architecture and State Management)`
- **Phase 1:** Add to Stage 1.4 notes: `**See:** ADR-037 (UI Customization Approach)`
- **Phase 2 (Transactional Core):** Add to Stage 2.3 notes: `**See:** ADR-035 (Form Architecture — booking flow latency)`
- **Phase 2:** Add to Stage 2.4 notes: `**See:** ADR-038 (Webhook Architecture), ADR-036 (Real-Time and Background Tasks)`
- **Phase 2:** Add to Stage 2.5 notes: `**See:** ADR-039 (PDF Invoice Generation)`
- **Phase 3:** Add to Stage 3.1 notes: `**See:** ADR-040 (Data Access Patterns — pagination, denormalization)`
- **Phase 3:** Add to Stage 3.2 notes: `**See:** ADR-036 (Real-Time and Background Tasks — campaign enrollment task)`
- **Phase 5:** Add to Stage 5.1 notes: `**See:** ADR-040 (Data Access Patterns — pre-computed aggregations)`
- **Cross-Phase Dependencies** section: Add `**See:** ADR-034 (Anvil Platform Constraints) for platform-specific implementation guidance.`
- **Risk Mitigation** section: Add row for ADR-034: `| **Anvil constraints** | All phases | ADR-034 documents Skulpt limits, lazy rows, server latency, background tasks |`

#### Task 29 — architecture.md
- **§5 Three-Tier Module Pattern:** Add `**See:** ADR-035 (Form Architecture and State Management)`
- **§6 Navigation Architecture:** Add `**See:** ADR-037 (UI Customization Approach)`
- **§7 Startup and Authentication Routing:** Add `**See:** ADR-034 (Anvil Platform Constraints — server call latency)`

#### Task 30 — integration-architecture.md
- **§1 Payment Gateway Architecture:** Add `**See:** ADR-038 (Webhook Architecture), ADR-034 (Anvil Platform Constraints)`
- **§4 Background Tasks:** Add `**See:** ADR-036 (Real-Time and Background Task Architecture)`
- Add new section after §4: `**See:** ADR-039 (PDF Invoice Generation) for invoice PDF approach.`
- Add: `**See:** ADR-040 (Data Access Patterns) for denormalization and pre-computed aggregation patterns.`

#### Task 31 — coding-standards.md
- **§2 Server Function Standards:** Add `**See:** ADR-034 (Anvil Platform Constraints — server call latency)`
- **§5 Data Tables Standards:** Add `**See:** ADR-040 (Data Access Patterns — denormalization, bounded pagination, pre-computed aggregations)`
- **§5 Query Patterns:** Add `**See:** ADR-035 (Form Architecture — state management via globals.py)`

#### Task 32 — testing-standards.md
- **§1 Testing Levels:** Add `**See:** ADR-034 (Anvil Platform Constraints — Skulpt runtime limitations affect client-side test approach)`
- **§3 Integration Testing via Uplink:** Add `**See:** ADR-036 (Real-Time and Background Tasks — background task testing patterns)`

#### Task 33 — user-flows.md
- **§2 Services Appointment Booking:** Already has ADR-035 ref. Add: `**See:** ADR-034 (Anvil Platform Constraints — Skulpt limits on date/money logic)`
- **§3 Admin Daily Operations:** Already has ADR-038 ref. Add: `**See:** ADR-036 (Real-Time and Background Tasks — polling intervals)`
- **§3 Processing a booking:** Add `**See:** ADR-039 (PDF Invoice Generation) for invoice creation on booking completion.`

#### Task 34 — design-direction.md
- **Design Tokens** section: Add `**See:** ADR-037 (UI Customization Approach)`
- **Anvil Design Rules** section: Add `**See:** ADR-027 (Design Rules)`

---

### Group D: Devlog Update (Task 35)

**File:** `implementation/devlog.md`

Add the following entries after the existing "Phase 5" section (after line 226):

```markdown
## June 2026 — Planning Realignment

### Status: Complete

**Completed:**
- ✅ Onboarding implementation plan rewritten (2026-06-01): Separate onboarding form, tabbed Settings, bidirectional navigation
- ✅ ADR-25 rewritten (2026-06-01): Onboarding finality, resumability, append-only logging
- ✅ ADR-12 superseded (2026-06-01): By build-plan.md
- ✅ ADR-13 superseded (2026-06-01): By consolidated ADR-16
- ✅ ADR-027 created (2026-06-10): Design Rules
- ✅ ADR-028 through ADR-033 created (2026-06-13): Client instance architecture, dependency update model, blank_client_template, free trial abandoned, observability architecture, Mybizz_management visibility
- ✅ ADR-034 through ADR-040 created (2026-06-14): Anvil platform constraints, form architecture, real-time/background tasks, UI customization, webhook architecture, PDF invoice generation, data access patterns
- ✅ DESIGN.md reconciled (2026-06-02): OnboardingPaletteForm removed, ClauseBuilder added, PaletteSettingsForm downgraded, 11-tab Settings, onboarding/settings boundary
- ✅ Sanity audit completed (2026-06-01): 21 issues found and resolved, 3-pass audit
- ✅ CEO review completed (2026-06-14): 18 gaps identified, 36 tasks to close

**Blockers:** None

**Decisions:**
- All 7 new ADRs (034-040) consolidate decisions from platform-overview.md into formal ADR format
- blog_comments table exists in anvil.yaml but was missing from authoritative-schema.md — added
- 5 summary tables (ADR-040, ADR-036) required for analytics — added to schema
- bookings table in anvil.yaml missing 12 of 16 columns — requires Anvil Designer fix
- tasks table missing instance_id — requires Anvil Designer fix
- All doc discrepancies (stale counts, duplicates, missing ADR refs) — resolved in this session
```

Also update the "Last Updated" at the top of devlog.md from "2026-05-26" to "2026-06-14".

---

### Group E: Fix clarifications.md path (Task 36)

**File:** `docs/clarifications.md`

In the "Files to Reference" table (around line 86-93), change:
```
| **Build plan** | `docs/build-plan.md` |
```
to:
```
| **Build plan** | `implementation/build-plan.md` |
```

---

## Execution Order

1. Read all source files first (ADR-034 through ADR-040, DESIGN.md schema sections, anvil.yaml blog_comments definition)
2. Edit authoritative-schema.md (Tasks 11-21) — largest edit, do first
3. Edit architecture.md (Tasks 22, 26, 29)
4. Edit platform-overview.md (Task 23)
5. Edit clarifications.md (Tasks 24, 36)
6. Edit design-direction.md (Tasks 25, 34)
7. Edit integration-architecture.md (Tasks 27, 30)
8. Edit coding-standards.md (Task 31)
9. Edit testing-standards.md (Task 32)
10. Edit user-flows.md (Task 33)
11. Edit build-plan.md (Task 28)
12. Edit devlog.md (Task 35)
13. Verify: grep for "36 tables" — should return zero results
14. Verify: grep for "36 Tables" — should return zero results

## Verification Checklist

After all edits:
- [ ] authoritative-schema.md documents 43 tables (38 existing + 5 new)
- [ ] authoritative-schema.md footer says "43 tables"
- [ ] No file in docs/ or implementation/ says "36 tables" or "36 Tables"
- [ ] architecture.md has no duplicate CustomerLayout line
- [ ] design-direction.md has no duplicate sections (only one Interactive States, one Responsive Behavior, one Accessibility, one Layout Specifications)
- [ ] integration-architecture.md lists 7 background tasks (not 3)
- [ ] build-plan.md has ADR-034 through ADR-040 references in relevant stages
- [ ] All 7 docs have appropriate ADR cross-references
- [ ] devlog.md has June 2026 entries and "Last Updated: 2026-06-14"
- [ ] clarifications.md references `implementation/build-plan.md` (not `docs/build-plan.md`)

---

*End of /build prompt — Batch 1*
