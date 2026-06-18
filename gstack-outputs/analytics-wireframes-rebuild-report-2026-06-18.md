# Analytics Wireframes Rebuild Report — 2026-06-18

**Task:** Rebuild DashboardAnalyticsForm and RevenueReportForm wireframes per ADR-27.
**Rule:** No existing wireframe was trusted as a reference. All wireframes rebuilt from scratch using ADR-27 (format) and project-library specs (content).

---

## 1. What Was Created

### 1.1 DashboardAnalyticsForm

**File:** `wireframes/analytics/DashboardAnalyticsForm/wireframe-analytics-DashboardAnalyticsForm.html`

**Structure derived from:**
- `scaffold-spec.md` — analytics/ package, Phase 5, server_analytics/reporting_service
- `DESIGN.md` — Empty State spec (Chart icon, "No data yet", no primary action)
- `authoritative-schema.md` — daily_summary, service_performance tables (data sources for metrics/charts)
- `test-specifications.md` — RBAC: Owner, Manager, Admin, Staff all have access; Customer does not

**Components:**
- `col_analytics` (ColumnPanel) — root container
- `hdg_analytics_title` (Heading) — "Analytics"
- `txt_analytics_subtitle` (Text) — subtitle (replaced legacy Label)
- `flow_period_selector` (FlowPanel) — period toggle with 4 Links (Week, Month, Quarter, Custom)
- `flow_metrics_row` (FlowPanel) — 4 Cards: card_revenue, card_bookings, card_customers, card_avg_order
  - Each card has: Heading (label), Text (value), Text (change indicator)
- `flow_charts_primary` (FlowPanel) — 2 Cards with Plots:
  - card_revenue_trend / plot_revenue_trend (line chart)
  - card_bookings_by_service / plot_bookings_by_service (bar chart)
- `flow_charts_secondary` (FlowPanel) — 2 Cards:
  - card_customer_growth / plot_customer_growth (line chart)
  - card_booking_summary / dg_booking_summary (DataGrid)
- `col_empty_state` (ColumnPanel, dashed = hidden on load) — icon + heading + body text

**Properties table:** Empty — this is a read-only dashboard with no writeback components. All properties are set programmatically.

### 1.2 RevenueReportForm

**File:** `wireframes/analytics/RevenueReportForm/wireframe-analytics-RevenueReportForm.html`

**Structure derived from:**
- `scaffold-spec.md` — analytics/ package, Phase 5
- `DESIGN.md` — Empty State pattern (general)
- `authoritative-schema.md` — daily_summary table (revenue data source)
- `m3_component_mapping.md` — DatePicker (partially supported, Anvil classic)

**Components:**
- `col_revenue` (ColumnPanel) — root container
- `hdg_report_title` (Heading) — "Revenue Report"
- `txt_report_subtitle` (Text) — subtitle (replaced legacy Label)
- `flow_filters` (FlowPanel) — date range selector:
  - `dp_date_from` (DatePicker) with writeback (W)
  - `dp_date_to` (DatePicker) with writeback (W)
  - `btn_generate` (Button) — "Generate Report"
- `flow_summary_row` (FlowPanel) — 3 Cards: card_total_revenue, card_total_bookings, card_avg_order
  - Each card has: Heading (label), Text (value)
- `card_revenue_chart` (Card) — plot_revenue_over_time (Plot) + companion Heading
- `card_revenue_details` (Card) — dg_revenue_details (DataGrid) + companion Heading
- `col_empty_state` (ColumnPanel, dashed = hidden on load) — icon + heading + body text

**Properties table:** writeback = W for dp_date_from and dp_date_to (DatePicker components).

### 1.3 RevenueReportForm-RowTemplate

**File:** `wireframes/analytics/RevenueReportForm/RevenueReportForm-RowTemplate/wireframe-analytics-RevenueReportForm-RowTemplate.html`

**Status:** Left as empty file. See Specification Gaps section below.

---

## 2. ADR-27 Section 7 Compliance Checklist

### 2.1 DashboardAnalyticsForm

| # | Checklist Item | Result |
|---|---------------|--------|
| 1 | Section 0 reading completed | PASS — ADR-27, nomenclature.md, m3_component_mapping.md, scaffold-spec.md, DESIGN.md, authoritative-schema.md, gstack-outputs design review + remediation reports all read |
| 2 | Document contains exactly three sections in correct order | PASS — Introduction, Wireframe Diagram, Properties Table |
| 3 | Document is NOT formatted as a "Screen" type document | PASS |
| 4 | No legacy Label (lbl_) component used anywhere | PASS — subtitle uses Text component |
| 5 | Every component is an approved M3 component | PASS — ColumnPanel, FlowPanel, Card, Heading, Text, Link, Plot, DataGrid, Icon, Button all approved |
| 6 | Every visible text element has its own explicit component | PASS — all text represented by Heading, Text, or Link components |
| 7 | No sample data appears anywhere | PASS — metric values and chart data are runtime-only, not shown |
| 8 | No behaviour/data/explanatory text in wireframe | PASS — no responsive notes, no chart type descriptions, no behavioural annotations |
| 9 | Containers drawn as containers with names inset | PASS — all containers have inset labels |
| 10 | No border drawn around non-bordered component types | PASS |
| 11 | Properties table contains ONLY Designer-only properties | PASS — table is empty with note (all properties programmatic) |
| 12 | writeback (W) specified for DatePicker/text-input | N/A — no DatePicker or text-input components in this form |
| 13 | All component names follow nomenclature.md | PASS — col_, flow_, card_, hdg_, txt_, nav_, plot_, dg_, ico_, btn_ all correct |
| 14 | FlowPanel/ColumnPanel used only for intended purpose | PASS |
| 15 | Wireframe matches upstream architecture | PASS — matches scaffold-spec analytics/ package and DESIGN.md empty state spec |

**Result: 15/15 PASS (1 N/A)**

### 2.2 RevenueReportForm

| # | Checklist Item | Result |
|---|---------------|--------|
| 1 | Section 0 reading completed | PASS |
| 2 | Document contains exactly three sections in correct order | PASS |
| 3 | Document is NOT formatted as a "Screen" type document | PASS |
| 4 | No legacy Label (lbl_) component used anywhere | PASS — all former Label uses replaced with Text or Heading |
| 5 | Every component is an approved M3 component | PASS |
| 6 | Every visible text element has its own explicit component | PASS |
| 7 | No sample data appears anywhere | PASS |
| 8 | No behaviour/data/explanatory text in wireframe | PASS |
| 9 | Containers drawn as containers with names inset | PASS |
| 10 | No border drawn around non-bordered component types | PASS |
| 11 | Properties table contains ONLY Designer-only properties | PASS — only writeback (W) for DatePickers listed |
| 12 | writeback (W) specified for DatePicker/text-input | PASS — dp_date_from and dp_date_to both listed |
| 13 | All component names follow nomenclature.md | PASS |
| 14 | FlowPanel/ColumnPanel used only for intended purpose | PASS |
| 15 | Wireframe matches upstream architecture | PASS |

**Result: 15/15 PASS**

---

## 3. Prior Existing Wireframe Diagnostic

### 3.1 DashboardAnalyticsForm (old version)

**File:** `wireframes/analytics/DashboardAnalyticsForm/wireframe-analytics-DashboardAnalyticsForm.html`

**Violations found:**
1. **Legacy Label used** — `lbl_subtitle (Label)` on line 63. ADR-27 Section 3.2 prohibits legacy Label in M3 wireframes.
2. **Sample data present** — Metric cards show hardcoded values: "$12,480", "148", "34", "$84.32" with percentage changes. DataGrid table shows "Consultation", "Full Service", "Follow-up", "Express" with booking counts and revenue. ADR-27 Section 4.2.
3. **Explanatory/behavioural text** — Multiple violations of ADR-27 Section 4.3:
   - "Segmented button group — one active at a time. Active link uses primary-button role; inactive use tertiary-button role."
   - "4-column grid on desktop, 2-column on tablet, 1-column on mobile."
   - "2:1 grid on desktop, stacked on mobile."
   - "1:1 grid on desktop, stacked on mobile."
   - Chart type descriptions: "Line chart — revenue over selected period", "Bar chart — bookings grouped by service type"
4. **Properties table lists non-Designer-only properties** — Responsive layout column counts ("layout = column(4) on desktop", "layout = column(2fr, 1fr)") are not Designer-only properties. ADR-27 Section 6.1.
5. **Missing introduction section** — No clear "Section 1: Introduction" heading or developer message as required by ADR-27 Section 2.1.

**Root cause:** The wireframe was likely created by copying structure from an existing wireframe that itself had these issues, or by an agent that did not read ADR-27 Section 4 (Text and Content Rules) and Section 6 (Properties Table Rules) carefully.

### 3.2 RevenueReportForm (old version)

**File:** `wireframes/analytics/RevenueReportForm/wireframe-analytics-RevenueReportForm.html`

**Violations found:**
1. **Legacy Labels used** — `lbl_subtitle (Label)`, `lbl_date_from (Label)`, `lbl_date_to (Label)` on lines 62, 71, 75. ADR-27 Section 3.2.
2. **Sample data present** — DataGrid table shows dates ("2026-05-01", "2026-05-02", "2026-05-03"), revenue amounts ("$420", "$580", "$340"), booking counts ("5", "7", "4"), averages ("$84.00", "$82.86", "$85.00"). ADR-27 Section 4.2.
3. **Explanatory/behavioural text** — Multiple violations of ADR-27 Section 4.3:
   - "Horizontal flow: From label + DatePicker, To label + DatePicker, Generate Report button. Stacks vertically on mobile."
   - "3-column grid on desktop, 1-column on mobile. Each card is a tonal-card with summary-label and summary-value."
   - "Line chart — daily revenue for selected date range"
   - "4-column DataGrid: Date, Revenue, Bookings, Avg Order Value. One row per day in selected range. Sorted by date ascending."
4. **Properties table lists non-Designer-only properties** — Responsive layout column count. ADR-27 Section 6.1.
5. **Missing introduction section** — Same issue as DashboardAnalyticsForm.

**Root cause:** Same as 3.1 — likely created from the same template or by the same agent session.

### 3.3 RevenueReportForm-RowTemplate (old version)

**File:** `wireframes/analytics/RevenueReportForm/RevenueReportForm-RowTemplate/wireframe-analytics-RevenueReportForm-RowTemplate.html`

**Finding:** File is completely empty (0 lines). Cannot be evaluated against ADR-27.

---

## 4. Specification Gaps

### 4.1 RevenueReportForm-RowTemplate — Undocumented Component

The existing file structure includes a `RevenueReportForm-RowTemplate` subfolder, but:
- The scaffold spec (`scaffold-spec.md`) lists only `DashboardAnalyticsForm` and `RevenueReportForm` under `analytics/`. No row template is listed.
- The scaffold spec's DataGrid conventions don't mandate separate row template forms.
- The file was empty — no specification exists for this component.

**Decision:** Left the empty file in place. If a row template is needed for `dg_revenue_details`, it should be specified in the scaffold spec before a wireframe is created.

### 4.2 Analytics Empty State — RevenueReportForm Not Explicitly Listed

`DESIGN.md` Empty State Specifications list the "Analytics dashboard" empty state but do not explicitly list "Revenue report" as a separate screen. The revenue report empty state was derived from the general empty state pattern.

### 4.3 Loading States for Analytics Forms

The design review (`plan-design-review-2026-06-16.md`) identified that loading states for analytics are not specifically defined. The general loading patterns from `design-direction.md` (card skeleton, table skeleton rows) apply but are not wireframed — loading states are runtime behaviours, not structural components.

### 4.4 RBAC for Analytics Forms

The test specifications (`test-specifications.md`) list `DashboardForm` in the RBAC matrix but not `DashboardAnalyticsForm` or `RevenueReportForm` specifically. The navigation spec shows `nav_analytics — AnalyticsForm (always)` suggesting all authenticated roles can access it. The exact RBAC constraints for these specific forms should be confirmed during implementation.

### 4.5 Chart Types Not Specified

The authoritative schema defines the data tables (`daily_summary`, `service_performance`, `provider_performance`, `campaign_metrics`) but does not specify which chart types (line, bar, pie, etc.) should be used for each visualization. The wireframe uses Plot components without specifying chart type — this is a programmatic decision to be made during implementation.

---

## 5. Empty/Loading/Error State Identification

### Empty States (wireframed)

| Form | State | Wireframed? | Source |
|------|-------|-------------|--------|
| DashboardAnalyticsForm | No data | Yes — dashed col_empty_state | DESIGN.md Empty State spec |
| RevenueReportForm | No report generated | Yes — dashed col_empty_state | Derived from general pattern |

### Loading States (not wireframed — runtime behaviour)

| Form | Pattern | Source |
|------|---------|--------|
| DashboardAnalyticsForm | Card skeleton / metric placeholder | design-direction.md |
| DashboardAnalyticsForm | DataGrid skeleton rows | design-direction.md |
| RevenueReportForm | Card skeleton for summary | design-direction.md |
| RevenueReportForm | DataGrid skeleton rows | design-direction.md |
| RevenueReportForm | Button spinner on "Generate Report" | design-direction.md |

### Error States (not wireframed — runtime behaviour)

| Form | Pattern | Source |
|------|---------|--------|
| Both | Server call failure — retry button + error message | build-plan.md general pattern |
| Both | Network timeout — retry with preserved state | test-specifications.md |

---

*End of Analytics Wireframes Rebuild Report — 2026-06-18*
