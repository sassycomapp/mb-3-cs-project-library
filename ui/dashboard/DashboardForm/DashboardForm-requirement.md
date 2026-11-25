---
title: "DashboardForm-requirement"
status: draft
doc-id: "mb-5197a713"
updated: "2026-11-25"
---
# DashboardForm — Screen Requirement

## 1. Form

| Item | Value |
|---|---|
| Module path | `dashboard.DashboardForm` |
| Purpose | Today's metrics, alerts and activity feed; each role sees only what its role allows (ticket 25, F-04; UF-11) |
| Screen type and width | Dashboard — 1200 px, full width: metric cards |
| Layout | `layouts.AdminLayout` |
| Route | None — opened from the admin navigation and after onboarding |

## 2. Elements

| Element | Theme component | Why this component | Role | `wrap_on` | Tooltip phrase key |
|---|---|---|---|---|---|
| Page title | `m3.Heading` (`hdg_dashboard`) | "Dashboard", `headline-large` | `page-title` | — | `dashboard.title` |
| Period comparison selector | `m3.ButtonGroup` (`bgrp_period`, connected, single: Week/Month/Custom) | Period comparison — week, month, custom range (architecture reference F-04) | — | mobile | `dashboard.period.week`, `dashboard.period.month`, `dashboard.period.custom` |
| Custom range fields | `m3.DatePicker` (`dp_period_from`), `m3.DatePicker` (`dp_period_to`) | The custom range | `outlined-date` | mobile | `data.history.from`, `data.history.to` |
| Metric cards row | Anvil `FlowPanel` (`flow_metrics`) | The metric cards in a row that wraps | — | mobile | — |
| Revenue metric card | `dashboard.DashboardForm.MetricCard` (`metric_revenue`) | Revenue: current value, percentage change and trend (architecture reference F-04) | `metric-card` | mobile (320 px card in a grid) | `dashboard.metric.revenue` |
| Bookings metric card | `dashboard.DashboardForm.MetricCard` (`metric_bookings`) | Bookings today/period | `metric-card` | mobile | `dashboard.metric.bookings` |
| Customers metric card | `dashboard.DashboardForm.MetricCard` (`metric_customers`) | New customers | `metric-card` | mobile | `dashboard.metric.customers` |
| Trend chart | Anvil `Plot` (`plot_trend`) | The approved Anvil Plot draws the trend charts (DESIGN.md: "Complex charts… beyond what is operationally useful" are avoided; the trend is the useful one) | — | mobile | — |
| Alerts card | `m3.Card` (`card_alerts`, elevated) | The actionable alerts (pending items, overdue) | `elevated-card` | mobile | — |
| Alert row | `m3.ListItem` (`li_alert_*`, clickable) | One alert per row; the click opens the item | — | — | — |
| Activity feed | `dashboard.DashboardForm.ActivityFeed` (`feed_activity`) | The activity feed (architecture reference F-04) | `surface-card` | mobile | — |
| Return button | `m3.Button` (`btn_return`, outlined) | The dashboard has a clear Return button that takes the user back to the page the dashboard was opened from (spec-ui-standards section 12.5) | `secondary-button` | — | `dashboard.return` |
| Sample-data banner | the layout's banner | Shows while sample data exists (the layout carries it) | — | — | — |
| Help icon | `components.HelpTooltipComponent` (help entry `dashboard.home`) | The feature's tooltip and help video | — | — | `help.dashboard.home.tooltip` |

The dashboard figures are read from pre-computed summary tables, refreshed by background task, not computed on page load (architecture reference F-04; SR-10).

## 3. Data

| Element | Table | Column |
|---|---|---|
| Metric cards and trend | `daily_summary`, `service_performance`, `provider_performance` | `total_revenue`, `revenue_by_service`, `booking_count`, `new_contacts` (written by the tasks only) |
| Alerts | `bookings`, `invoice`, `tasks` | the pending/overdue items the dashboard's own functions read |
| Activity feed | `activity_log` | `action_type`, `description`, `created_at`, `user_id` |

## 4. Actions

| Action | Event handler | Server function | Result |
|---|---|---|---|
| Load the dashboard | `form_show` → `load_dashboard` | `dashboard_service` read (`server_dashboard/dashboard_service`) | The role's figures arrive — each role sees only what its role allows (spec-ui-standards section 12.4) |
| Change the period | `bgrp_period_change` | the same read | The cards and trend reload for the period |
| Open an alert's item | `li_alert_*_click` | None | The booking, invoice or task opens |
| Return | `btn_return_click` | None | The page the dashboard was opened from reopens |
| Refresh | `tmr_dashboard_tick` (30 s) | `dashboard_service` read | The figures refresh by polling (adr-real-time-and-background-tasks) |

## 5. Empty State

| Part | Content |
|---|---|
| Icon | Chart icon (`mi:monitoring`) |
| Heading | `dashboard.empty.title` (DESIGN.md: "No data yet") |
| Body text | `dashboard.empty.body` (DESIGN.md: "Analytics will appear here once you have bookings, payments, and contacts in the system." — no action, per DESIGN.md) |
| Primary action | None — the empty state explains that data appears as the business operates (DESIGN.md) |

## 6. Access

| Role | Scope |
|---|---|
| Owner | all |
| Manager | all |
| Admin | all |
| Staff | all |
| Customer | none |
| SignedOut | none |

Each user sees only what their role may see (spec-ui-standards section 12.4; the access matrix `dashboard.DashboardForm`).

## 7. Text

| Phrase key | English text |
|---|---|
| `dashboard.title` | Dashboard |
| `dashboard.period.week` | Week |
| `dashboard.period.month` | Month |
| `dashboard.period.custom` | Custom range |
| `dashboard.metric.revenue` | Revenue |
| `dashboard.metric.bookings` | Bookings |
| `dashboard.metric.customers` | New customers |
| `dashboard.return` | Return |
| `dashboard.alerts.title` | Needs your attention |
| `dashboard.activity.title` | Recent activity |
| `dashboard.empty.title` | No data yet |
| `dashboard.empty.body` | Your figures appear here as your business operates. |

## 8. Help Videos

| Process | Help video |
|---|---|
| Reading your dashboard | `master_template` help entry `dashboard.home` — played in the app language |
| The sample data banner | `master_template` help entry `admin.sample_data` |

## 9. User Flows

| Flow | Part this form plays |
|---|---|
| UF-02 Owner onboarding | The Owner lands here when onboarding completes, where the sample data banner shows |
| UF-11 Admin daily operations | The morning routine's first stop |
| UF-25 Marketing dashboard | The deeper marketing figures live in `analytics.MarketingDashboardForm` |
| UF-43 Clearing the sample data | The banner's Clear action; the figures recompute from the client's own data |
