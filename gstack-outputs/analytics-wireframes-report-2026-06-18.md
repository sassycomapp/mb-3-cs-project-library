# Analytics Wireframes Report

**Date:** 2026-06-18
**Scope:** Analytics package wireframes rebuilt per ADR-27
**Agent:** Build
**Authority:** ADR-027, scaffold-spec.md, nomenclature.md, m3_component_mapping.md, DESIGN.md, ADR-040, build-plan.md Stage 5.1

---

## 1. Wireframes Created

### 1.1 DashboardAnalyticsForm

**File:** `wireframes/analytics/DashboardAnalyticsForm/wireframe-analytics-DashboardAnalyticsForm.html`

**Architectural backing:**
- `Clearly defined:` scaffold-spec.md lists `DashboardAnalyticsForm` in the `analytics/` package [Phase 5]
- `Clearly defined:` build-plan.md Stage 5.1 specifies enhanced DashboardForm with live analytics, charts, and period selector
- `Clearly defined:` ADR-040 specifies pre-computed summary tables (`daily_summary`, `service_performance`, `provider_performance`, `campaign_metrics`) read by dashboard forms
- `Clearly defined:` DESIGN.md Dashboard Forms pattern: Heading (headline-large), FlowPanel for metrics row with elevated Cards, Plot components for charts, DataGrid for summary tables

**Component structure:**
- Page title: `hdg_analytics_title` (Heading) "Analytics"
- Subtitle: `txt_analytics_subtitle` (Text)
- Period selector: `flow_period_selector` (FlowPanel) with 4 Links (Week, Month, Quarter, Custom)
- Metrics row: `flow_metrics_row` (FlowPanel) holding 4 Cards:
  - `card_revenue` — revenue metric (Heading + 2 Text)
  - `card_bookings` — booking metric (Heading + 2 Text)
  - `card_customers` — customer metric (Heading + 2 Text)
  - `card_avg_order` — avg order metric (Heading + 2 Text)
- Primary charts: `flow_charts_primary` (FlowPanel) holding 2 Cards:
  - `card_revenue_trend` — Heading + Plot
  - `card_bookings_by_service` — Heading + Plot
- Secondary charts: `flow_charts_secondary` (FlowPanel) holding 2 Cards:
  - `card_customer_growth` — Heading + Plot
  - `card_booking_summary` — Heading + DataGrid
- Empty state: `col_empty_state` (ColumnPanel, dashed = hidden on load) with 4 required components:
  1. `ico_empty_state` (Icon) "chart"
  2. `hdg_empty_state` (Heading) "No data yet"
  3. `txt_empty_state` (Text)
  4. `btn_empty_action` (Button) "Create Your First Booking"

**Properties table:** No Designer-only properties. All properties set programmatically.

**ADR-27 Section 7 Compliance:**

| Checklist Item | Result |
|---|---|
| Section 0 reading completed | PASS |
| Exactly three sections in correct order | PASS |
| Not formatted as Screen type | PASS |
| No legacy Label (lbl_) | PASS |
| All components approved M3 | PASS |
| Every visible text has explicit component | PASS |
| No sample data | PASS |
| No behaviour/explanatory text in diagram | PASS |
| Containers drawn as containers with names inset | PASS |
| No border on non-bordered components | PASS |
| Properties table: Designer-only only | PASS |
| writeback (W) specified where needed | PASS (none needed) |
| All names follow nomenclature.md | PASS |
| Containers used for intended purpose | PASS |
| Architecturally matches upstream | PASS |

**Result: PASS — all 15 items checked.**

---

### 1.2 RevenueReportForm

**File:** `wireframes/analytics/RevenueReportForm/wireframe-analytics-RevenueReportForm.html`

**Architectural backing:**
- `Clearly defined:` scaffold-spec.md lists `RevenueReportForm` in the `analytics/` package [Phase 5]
- `Clearly defined:` build-plan.md Stage 5.1 includes revenue reporting with date range selection
- `Clearly defined:` ADR-040 specifies pre-computed summary tables for revenue data
- `Clearly defined:` DESIGN.md pattern for report forms: date range filters, summary cards, chart, data grid

**Component structure:**
- Page title: `hdg_report_title` (Heading) "Revenue Report"
- Subtitle: `txt_report_subtitle` (Text)
- Filters bar: `flow_filters` (FlowPanel) with:
  - `dp_date_from` (DatePicker) (W)
  - `dp_date_to` (DatePicker) (W)
  - `btn_generate` (Button) "Generate Report"
- Summary row: `flow_summary_row` (FlowPanel) holding 3 Cards:
  - `card_total_revenue` — Heading + Text
  - `card_total_bookings` — Heading + Text
  - `card_avg_order` — Heading + Text
- Revenue chart: `card_revenue_chart` (Card) with Heading + Plot
- Revenue details table: `card_revenue_details` (Card) with Heading + DataGrid
- Empty state: `col_empty_state` (ColumnPanel, dashed = hidden on load) with 4 required components:
  1. `ico_empty_state` (Icon) "receipt"
  2. `hdg_empty_state` (Heading) "No revenue data"
  3. `txt_empty_state` (Text)
  4. `btn_empty_action` (Button) "Create Your First Booking"

**Properties table:** Two Designer-only properties:
- `dp_date_from` — writeback = W
- `dp_date_to` — writeback = W

**ADR-27 Section 7 Compliance:**

| Checklist Item | Result |
|---|---|
| Section 0 reading completed | PASS |
| Exactly three sections in correct order | PASS |
| Not formatted as Screen type | PASS |
| No legacy Label (lbl_) | PASS |
| All components approved M3 | PASS |
| Every visible text has explicit component | PASS |
| No sample data | PASS |
| No behaviour/explanatory text in diagram | PASS |
| Containers drawn as containers with names inset | PASS |
| No border on non-bordered components | PASS |
| Properties table: Designer-only only | PASS |
| writeback (W) specified where needed | PASS (both DatePickers) |
| All names follow nomenclature.md | PASS |
| Containers used for intended purpose | PASS |
| Architecturally matches upstream | PASS |

**Result: PASS — all 15 items checked.**

---

## 2. Forms/Sub-Templates Skipped (No Architectural Backing)

### 2.1 RevenueReportForm-RowTemplate

**Path:** `wireframes/analytics/RevenueReportForm/RevenueReportForm-RowTemplate/`

**Architectural reason:** The scaffold-spec.md treats `dg_revenue_details` as a standard DataGrid component with column definitions configured in the Designer. DataGrids in Anvil do not use row templates in the same way as RepeatingPanels — they render rows from column definitions. The scaffold-spec does not specify a row template sub-form for RevenueReportForm. The existing placeholder file at this path is empty (0 bytes) and has no architectural backing.

**Action taken:** No wireframe created. The empty placeholder file remains as-is.

---

## 3. Specification Gaps

No specification gaps were found. Both analytics forms have clear architectural backing from:
- scaffold-spec.md (package structure and form listing)
- build-plan.md Stage 5.1 (feature requirements)
- ADR-040 (data access patterns and summary tables)
- DESIGN.md (Dashboard form component pattern)
- authoritative-schema.md (summary table definitions: daily_summary, service_performance, provider_performance, campaign_metrics)

---

## 4. Summary

| Item | Status |
|---|---|
| DashboardAnalyticsForm wireframe | Created, ADR-27 compliant |
| RevenueReportForm wireframe | Created, ADR-27 compliant |
| RevenueReportForm-RowTemplate | Skipped — no architectural backing |
| Specification gaps | None found |
| ADR-27 compliance | Both wireframes pass all 15 checklist items |

---

*End of report*
