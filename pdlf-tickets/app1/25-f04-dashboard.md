---
title: "25 — F-04 Dashboard and analytics"
status: open
labels: [ready-for-agent]
created: 2026-11-17
doc-id: "mb-666d1ef2"
updated: "2026-11-17"
---

## Description

**Feature:** F-04 Dashboard and analytics (UF-11, UF-25, UF-43; SR-10; FR-04; test specification §4.10, §4.11). Common feature reading trade tables through its own server functions (ADR `common-feature-reads-trade-data`).
**Depends on:** 23 (invoicing — revenue data), 24 (bookings), 29 (email — campaign metrics for the marketing dashboard).

### Forms

- `dashboard.DashboardForm` (+ local `MetricCard`, `ActivityFeed`) — today's metrics, alerts and activity feed; each role sees only what its role allows (spec-ui-standards §12)
- `analytics.DashboardAnalyticsForm` — revenue by service and provider
- `analytics.MarketingDashboardForm` — marketing metrics, return on investment, alerts (UF-25)
- `analytics.RevenueReportForm` — reports, PDF and CSV export, scheduled reports

### Server functions

- `server_dashboard/dashboard_service` (renamed from `server_dashboard/service.py` at this step, per the divergence inventory)
- `server_analytics/reporting_service`, `server_analytics/report_export_service`
- `server_analytics/summary_tasks` — the period-keyed upserts inside `@tables.in_transaction` (R5): for each period, update the row with the recalculated figures or add it — a retried or overlapping run can never duplicate a period's row
- The sample-data banner state (Clear sample data is ticket 43's `sample_data_service`)

### Tables

- `daily_summary`, `service_performance`, `provider_performance`, `campaign_metrics` (written by the tasks only; dashboards never aggregate source tables on load — SR-10), `sample_rows` (banner), reads of `bookings`, `invoice`, `contacts` through the dashboard's own functions

### Acceptance tests

- Test-plan §18 (summary tasks — a run twice leaves exactly one row per period with the recalculated figures), §17 row 7 interplay; `IN-11`; `SR-10`
- `SR-44a`, `SR-44b`; tests 4.10–4.11 — the banner and sample-data isolation on the dashboard
- UF-11, UF-25; UF-43's banner behaviour

### Security criteria

- B9 — the period-keyed upserts (R5)
- AC-S12 — summaries are Internal; no personal data beyond row ids in any dashboard payload
- The dashboard has a clear Return button to the page it was opened from (spec-ui-standards §12.5)

## Comments
