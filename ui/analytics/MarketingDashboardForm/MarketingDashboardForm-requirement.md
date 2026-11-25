---
title: "MarketingDashboardForm-requirement"
status: draft
doc-id: "mb-dc3a50c4"
updated: "2026-11-25"
---
# MarketingDashboardForm — Screen Requirement

## 1. Form

| Item | Value |
|---|---|
| Module path | `analytics.MarketingDashboardForm` |
| Purpose | Marketing metrics, return on investment, alerts (ticket 25, F-04; UF-25) |
| Screen type and width | Dashboard — 1200 px, full width: metric cards |
| Layout | `layouts.AdminLayout` |
| Route | None — opened from the admin navigation (Finance & Reports → Analytics → Marketing) |

## 2. Elements

| Element | Theme component | Why this component | Role | `wrap_on` | Tooltip phrase key |
|---|---|---|---|---|---|
| Page title | `m3.Heading` (`hdg_marketing_dashboard`) | "Marketing", `headline-large` | `page-title` | — | `marketing.title` |
| Contacts metric card | `m3.Card` (`card_contacts_metric`, elevated) | Total contacts and new this month (UF-25 metrics) | `metric-card` | mobile | `marketing.metric.contacts` |
| Revenue metric card | `m3.Card` (`card_revenue_metric`, elevated) | Revenue, current against previous | `metric-card` | mobile | `marketing.metric.revenue` |
| Email performance card | `m3.Card` (`card_email_metric`, elevated) | Sent, open rate, click rate, conversions | `metric-card` | mobile | `marketing.metric.email` |
| Top sources card | `m3.Card` (`card_sources`, outlined) | Top lead sources by revenue | `surface-card` | mobile | — |
| ROI grid | Anvil `DataGrid` (`dg_roi`) | Return on investment per campaign and landing page (UF-25) | — | mobile | — |
| Alerts card | `m3.Card` (`card_marketing_alerts`, elevated) | Actionable alerts (UF-25); a nightly background task at 02:00 calculates the metrics and generates alerts | `elevated-card` | mobile | — |
| Alert actions | `m3.Button` (`btn_enrol_from_alert`, outlined) | Enrol contacts from an alert in one click (UF-25 actions) | `secondary-button` | — | `marketing.alert.enrol` |
| Export buttons | `m3.Button` (`btn_export_pdf`, outlined), `m3.Button` (`btn_export_csv`, outlined) | Export to PDF or CSV (UF-25 actions) | `secondary-button` | — | `analytics.export.pdf`, `analytics.export.csv` |
| Schedule report button | `m3.Button` (`btn_schedule_report`, text) | Schedule a report by email (UF-25 actions) | `tertiary-button` | — | `analytics.schedule` |
| Help icon | `components.HelpTooltipComponent` (help entry `analytics.marketing`) | The feature's tooltip and help video | — | — | `help.analytics.marketing.tooltip` |

## 3. Data

| Element | Table | Column |
|---|---|---|
| Metrics | `campaign_metrics`, `contacts`, `leads` | `campaign_metrics` figures; contact and lead counts |
| ROI | `campaign_metrics` | `emails_sent`, `opens`, `clicks`, `conversions`, `revenue_generated`, `unsubscribes` |
| Alerts | the nightly task's figures | the alert rules of UF-25 |

## 4. Actions

| Action | Event handler | Server function | Result |
|---|---|---|---|
| Load the figures | `form_show` → `load_marketing` | `reporting_service` read (`server_analytics/reporting_service`) | The period's figures arrive |
| Enrol from an alert | `btn_enrol_from_alert_click` | the campaigns' enrol function (ticket 29's server side) | The alert's contacts are enrolled in one click |
| Export | `btn_export_pdf_click` / `btn_export_csv_click` | `report_export_service` (`server_analytics/report_export_service`) | The PDF or CSV is produced; the export is audited |
| Schedule a report | `btn_schedule_report_click` | `reporting_service` schedule | The report is emailed by a background task at its schedule |

## 5. Empty State

| Part | Content |
|---|---|
| Icon | Mail icon (`mi:campaign`) |
| Heading | `marketing.empty.title` (DESIGN.md: "No campaigns yet") |
| Body text | `marketing.empty.body` (DESIGN.md: "Email campaigns help you stay in touch with your clients. Create your first campaign to get started.") |
| Primary action | `marketing.create_campaign` (+ Create Campaign) |

## 6. Access

| Role | Scope |
|---|---|
| Owner | all |
| Manager | all |
| Admin | none |
| Staff | none |
| Customer | none |
| SignedOut | none |

Per the access matrix rows added at this step.

## 7. Text

| Phrase key | English text |
|---|---|
| `marketing.title` | Marketing |
| `marketing.metric.contacts` | Contacts |
| `marketing.metric.revenue` | Revenue |
| `marketing.metric.email` | Email |
| `marketing.alert.enrol` | Enrol contacts |
| `analytics.export.pdf` | Export PDF |
| `analytics.export.csv` | Export CSV |
| `analytics.schedule` | Schedule a report |
| `marketing.create_campaign` | + Create Campaign |
| `marketing.empty.title` | No campaigns yet |
| `marketing.empty.body` | Email campaigns help you stay in touch with your clients. Create your first campaign to get started. |

## 8. Help Videos

| Process | Help video |
|---|---|
| Reading the marketing figures | `master_template` help entry `analytics.marketing` — played in the app language |

## 9. User Flows

| Flow | Part this form plays |
|---|---|
| UF-25 Marketing dashboard | The whole flow: metrics, ROI, alerts, actions |
| UF-23 / UF-24 Campaigns and broadcasts | The campaigns' results shown here |
| UF-20 Lead capture | The lead sources' contribution |
