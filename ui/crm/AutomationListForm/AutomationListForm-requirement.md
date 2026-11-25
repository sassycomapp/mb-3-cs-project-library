---
title: "AutomationListForm-requirement"
status: draft
doc-id: "mb-2a666521"
updated: "2026-11-25"
---
# AutomationListForm — Screen Requirement

## 1. Form

| Item | Value |
|---|---|
| Module path | `crm.AutomationListForm` |
| Purpose | The automation rules list with their switches and last runs (ticket 32, F-20; UF-26) |
| Screen type and width | List — 1200 px, full width: header, filters, data grid |
| Layout | `layouts.AdminLayout` |
| Route | None — opened from the admin navigation (Customers & Marketing → Automations) |

## 2. Elements

| Element | Theme component | Why this component | Role | `wrap_on` | Tooltip phrase key |
|---|---|---|---|---|---|
| Page title | `m3.Heading` (`hdg_automations`) | "Automations", `headline-large` | `page-title` | — | `automations.title` |
| New Automation button | `m3.Button` (`btn_new_automation`, filled) | New Automation (UF-26 step 1) | `primary-button` | — | `automations.new` |
| Automations grid | Anvil `DataGrid` (`dg_automations`) | Name, trigger, actions, on/off, last run | — | mobile | — |
| Automation row | Anvil `DataRowPanel` (`drp_automation_*`) | One rule per row | — | — | — |
| Active switch | `m3.Switch` (`sw_automation_active_*`) | Switch the automation on (UF-26 step 4; `automation_rules.is_active`) | — | — | `automations.on` | — | — |
| Help icon | `components.HelpTooltipComponent` (help entry `crm.automations`) | The feature's tooltip and help video | — | — | `help.crm.automations.tooltip` |

## 3. Data

| Element | Table | Column |
|---|---|---|
| Grid | `automation_rules` | `name`, `trigger_event`, `conditions`, `actions`, `is_active`, `last_run_at` |
| Runs | `automation_runs` | `result`, `ran_at` |

## 4. Actions

| Action | Event handler | Server function | Result |
|---|---|---|---|
| Load the list | `form_show` → `load_automations` | `automation_service` list read (`server_automation/automation_service`) | The grid arrives |
| Switch an automation | `sw_automation_active_*_change` | `automation_service` save | The rule is switched on or off; a background task evaluates automations and records each run |
| New automation | `btn_new_automation_click` | None | `crm.AutomationEditorForm` opens |
| Open an automation | the row click | None | `crm.AutomationEditorForm` opens |

## 5. Empty State

| Part | Content |
|---|---|
| Icon | Automation icon (`mi:bolt`) |
| Heading | `automations.empty.title` |
| Body text | `automations.empty.body` |
| Primary action | `automations.new` |

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
| `automations.title` | Automations |
| `automations.new` | + New Automation |
| `automations.on` | On |
| `automations.trigger.booking_completed` | Booking completed |
| `automations.trigger.enquiry` | Enquiry received |
| `automations.trigger.invoice_overdue` | Invoice overdue |
| `automations.trigger.no_booking` | No booking for 90 days |
| `automations.trigger.lead_captured` | Lead captured |
| `automations.empty.title` | No automations yet |
| `automations.empty.body` | When something happens and your conditions are met, an automation creates a task, sends an email or enrols contacts. |

## 8. Help Videos

| Process | Help video |
|---|---|
| Setting up automations | `master_template` help entry `crm.automations` — played in the app language |

## 9. User Flows

| Flow | Part this form plays |
|---|---|
| UF-26 Tasks and automation rules | The automations half of the flow |
