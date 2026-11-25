---
title: "AutomationEditorForm-requirement"
status: draft
doc-id: "mb-f84e1646"
updated: "2026-11-25"
---
# AutomationEditorForm — Screen Requirement

## 1. Form

| Item | Value |
|---|---|
| Module path | `crm.AutomationEditorForm` |
| Purpose | Create or edit an automation rule: trigger event, conditions, actions, switch on (ticket 32, F-20; UF-26) |
| Screen type and width | Editor, viewer, form — 900 px, sectioned panels |
| Layout | `layouts.AdminLayout` |
| Route | None — opened from `crm.AutomationListForm` |

## 2. Elements

| Element | Theme component | Why this component | Role | `wrap_on` | Tooltip phrase key |
|---|---|---|---|---|---|
| Page title | `m3.Heading` (`hdg_automation_editor`) | "New automation" / the rule's name, `headline-large` | `page-title` | — | `automations.new` |
| Name field | `m3.TextBox` (`txt_automation_name`, outlined) | The rule's name | `outlined-input` | — | `automations.name` |
| Trigger selector | `m3.DropdownMenu` (`dd_trigger_event`) | Booking completed, enquiry received, invoice overdue, no booking for 90 days, lead captured (schema `trigger_event`) | `outlined-select` | — | `automations.trigger` |
| Conditions panel | `m3.Card` (`card_conditions`, outlined) | Service, segment, tag (`automation_rules.conditions`) | `surface-card` | mobile | — |
| Condition row | Anvil `FlowPanel` (`flow_condition_*`) | One condition per row | — | mobile | — |
| Condition field selector | `m3.DropdownMenu` (`dd_condition_field_*`) | Service, segment, tag | `outlined-select` | — | `automations.condition.field` |
| Condition value selector | `m3.DropdownMenu` (`dd_condition_value_*`) | The field's value | `outlined-select` | — | `automations.condition.value` |
| Remove condition button | `m3.IconButton` (`ibtn_remove_condition_*`, icon `mi:close`) | Removes a condition | `icon-button` | — | `crm.segments.rule.remove` |
| Add condition button | `m3.Button` (`btn_add_condition`, outlined) | Adds a condition | `secondary-button` | — | `crm.segments.rule.add` |
| Actions panel | `m3.Card` (`card_actions`, outlined) | The actions with their settings (`automation_rules.actions`) | `surface-card` | mobile | — |
| Action row | Anvil `FlowPanel` (`flow_action_*`) | One action per row: kind and settings | — | mobile | — |
| Action kind selector | `m3.DropdownMenu` (`dd_action_kind_*`) | Create a task, send an email, enrol in a sequence, add a tag | `outlined-select` | — | `automations.action.kind` |
| Action settings fields | per kind | The action's settings (task title, email template, sequence, tag) | `outlined-input` | mobile | — |
| Remove action button | `m3.IconButton` (`ibtn_remove_action_*`, icon `mi:close`) | Removes an action | `icon-button` | — | `crm.segments.rule.remove` |
| Add action button | `m3.Button` (`btn_add_action`, outlined) | Adds an action | `secondary-button` | — | `crm.segments.rule.add` |
| Save button | `m3.Button` (`btn_save_automation`, filled) | Saves the rule | `primary-button` | — | `services.session.save` |
| Cancel button | `m3.Button` (`btn_cancel_automation`, text) | Returns without saving | `tertiary-button` | — | `editmode.bar.cancel` |
| Help icon | `components.HelpTooltipComponent` (help entry `crm.automation_editor`) | The feature's tooltip and help video | — | — | `help.crm.automation_editor.tooltip` |

## 3. Data

| Element | Table | Column |
|---|---|---|
| Rule | `automation_rules` | `name`, `trigger_event`, `conditions`, `actions`, `is_active` |
| Runs | `automation_runs` | one run per rule and trigger reference; success, skipped or failed |

## 4. Actions

| Action | Event handler | Server function | Result |
|---|---|---|---|
| Load the rule | `form_show` → `load_automation` | `automation_service` read (`server_automation/automation_service`) | The fields fill |
| Save | `btn_save_automation_click` | `automation_service` save | The rule is saved; the evaluation is a pure module beside the service; the background task records each run |
| Add / remove conditions and actions | the row buttons | None | The rows change |

## 5. Empty State

| Part | Content |
|---|---|
| Icon | None — the editor always shows its fields |
| Heading | None — not applicable |
| Body text | None — not applicable |
| Primary action | None — not applicable |

## 6. Access

| Role | Scope |
|---|---|
| Owner | all |
| Manager | all |
| Admin | none |
| Staff | none |
| Customer | none |
| SignedOut | none |

Per the access matrix.

## 7. Text

| Phrase key | English text |
|---|---|
| `automations.name` | Rule name |
| `automations.trigger` | When this happens |
| `automations.condition.field` | Where |
| `automations.condition.value` | Is |
| `automations.action.kind` | Then |
| `automations.action.create_task` | Create a task |
| `automations.action.send_email` | Send an email |
| `automations.action.enrol_sequence` | Enrol in a sequence |
| `automations.action.add_tag` | Add a tag |

## 8. Help Videos

| Process | Help video |
|---|---|
| Building an automation | `master_template` help entry `crm.automation_editor` — played in the app language |

## 9. User Flows

| Flow | Part this form plays |
|---|---|
| UF-26 Tasks and automation rules | The rule's build half |
