---
title: "EmailCampaignListForm-requirement"
status: draft
doc-id: "mb-7df82c78"
updated: "2026-11-25"
---
# EmailCampaignListForm — Screen Requirement

## 1. Form

| Item | Value |
|---|---|
| Module path | `crm.EmailCampaignListForm` |
| Purpose | The campaigns dashboard: active campaigns with enrolled count and performance; entry to the campaign editor and the broadcast editor (ticket 29, F-18; UF-23; added to ticket 29 by David's Step 22 decision) |
| Screen type and width | List — 1200 px, full width: header, filters, data grid |
| Layout | `layouts.AdminLayout` |
| Route | None — opened from the admin navigation (Customers & Marketing → Campaigns) |

## 2. Elements

| Element | Theme component | Why this component | Role | `wrap_on` | Tooltip phrase key |
|---|---|---|---|---|---|
| Page title | `m3.Heading` (`hdg_campaigns`) | "Campaigns", `headline-large` | `page-title` | — | `campaigns.title` |
| New Campaign button | `m3.Button` (`btn_new_campaign`, filled) | New Campaign → campaign editor (UF-23 step 2) | `primary-button` | — | `campaigns.new` |
| New Broadcast button | `m3.Button` (`btn_new_broadcast`, outlined) | New Broadcast → the broadcast editor (UF-24 step 1) | `secondary-button` | — | `broadcasts.new` |
| Campaigns grid | Anvil `DataGrid` (`dg_campaigns`) | Name, type, status, enrolled, sent, opens, clicks, conversions | — | mobile | — |
| Campaign row | Anvil `DataRowPanel` (`drp_campaign_*`) | One campaign per row | — | — | — |
| Status badge | `m3.Chip` (`chip_campaign_status_*`, type `input`, removable=False) | Draft, Scheduled, Active, Paused, Completed as outlined pills with the word | — | — | — | — | — |
| Help icon | `components.HelpTooltipComponent` (help entry `crm.campaigns`) | The feature's tooltip and help video | — | — | `help.crm.campaigns.tooltip` |

## 3. Data

| Element | Table | Column |
|---|---|---|
| Grid | `email_campaigns` | `campaign_name`, `campaign_type` (`sequence`/`broadcast`), `status`, `emails_sent`, `opens`, `clicks`, `conversions`, `last_run_date` |
| Enrolled count | `contact_campaigns` | rows per campaign |

## 4. Actions

| Action | Event handler | Server function | Result |
|---|---|---|---|
| Load the list | `form_show` → `load_campaigns` | `marketing` campaign read (`server_marketing`; the campaign service of ticket 29's server side) | The grid arrives |
| New campaign | `btn_new_campaign_click` | None | `crm.EmailCampaignEditorForm` opens in create mode |
| New broadcast | `btn_new_broadcast_click` | None | `crm.EmailBroadcastForm` opens |
| Open a campaign | the row click | None | `crm.EmailCampaignEditorForm` opens |

## 5. Empty State

| Part | Content |
|---|---|
| Icon | Mail icon (`mi:campaign`) |
| Heading | `campaigns.empty.title` (DESIGN.md: "No campaigns yet") |
| Body text | `campaigns.empty.body` (DESIGN.md: "Email campaigns help you stay in touch with your clients. Create your first campaign to get started.") |
| Primary action | `campaigns.new` (+ Create Campaign) |

## 6. Access

| Role | Scope |
|---|---|
| Owner | all |
| Manager | all |
| Admin | none |
| Staff | none |
| Customer | none |
| SignedOut | none |

Per the access matrix; visible only while `config.marketing_enabled` is True (AdminLayout).

## 7. Text

| Phrase key | English text |
|---|---|
| `campaigns.title` | Campaigns |
| `campaigns.new` | + New Campaign |
| `broadcasts.new` | + New Broadcast |
| `campaigns.type.sequence` | Sequence |
| `campaigns.type.broadcast` | Broadcast |
| `campaigns.status.draft` | Draft |
| `campaigns.status.scheduled` | Scheduled |
| `campaigns.status.active` | Active |
| `campaigns.status.paused` | Paused |
| `campaigns.status.completed` | Completed |
| `campaigns.empty.title` | No campaigns yet |
| `campaigns.empty.body` | Email campaigns help you stay in touch with your clients. Create your first campaign to get started. |

## 8. Help Videos

| Process | Help video |
|---|---|
| Email campaigns | `master_template` help entry `crm.campaigns` — played in the app language |

## 9. User Flows

| Flow | Part this form plays |
|---|---|
| UF-23 Email campaign setup | The dashboard and the entry to the editor |
| UF-24 Email broadcast | The entry to the broadcast editor |
| UF-25 Marketing dashboard | The campaigns' results roll up to the marketing dashboard |
