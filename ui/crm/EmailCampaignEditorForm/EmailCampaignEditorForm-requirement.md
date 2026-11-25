---
title: "EmailCampaignEditorForm-requirement"
status: draft
doc-id: "mb-76e03c26"
updated: "2026-11-25"
---
# EmailCampaignEditorForm — Screen Requirement

## 1. Form

| Item | Value |
|---|---|
| Module path | `crm.EmailCampaignEditorForm` |
| Purpose | The campaign editor: type from the ready-made sequences or blank, the sequence preview with delays, the trigger, and activation (ticket 29, F-18; UF-23) |
| Screen type and width | Editor, viewer, form — 900 px, sectioned panels |
| Layout | `layouts.AdminLayout` |
| Route | None — opened from `crm.EmailCampaignListForm` |

## 2. Elements

| Element | Theme component | Why this component | Role | `wrap_on` | Tooltip phrase key |
|---|---|---|---|---|---|
| Page title | `m3.Heading` (`hdg_campaign_editor`) | "New campaign" / the campaign's name, `headline-large` | `page-title` | — | `campaigns.new` |
| Name field | `m3.TextBox` (`txt_campaign_name`, outlined) | The campaign's name | `outlined-input` | — | `campaigns.name` |
| Type selector | `m3.DropdownMenu` (`dd_campaign_type`) | New Client Welcome, Appointment Follow-Up, Re-engagement, Referral Request, or blank (UF-23 step 3) | `outlined-select` | — | `campaigns.type.ready` |
| Sequence preview | Anvil `ColumnPanel` (`col_sequence_preview`) | Review each email and its delay (UF-23 step 4) | — | mobile | — |
| Sequence email row | `m3.ListItem` (`li_sequence_email_*`) | One email per row: its delay day, subject, preview | — | — | — |
| Edit email action | `m3.IconButton` (`ibtn_edit_email_*`, icon `mi:edit`) | Opens the email for editing | `icon-button` | — | `users.row.edit` |
| Sender selector | `m3.DropdownMenu` (`dd_campaign_sender`) | The sending address, e.g. `marketing@` (schema `from_address_id`) | `outlined-select` | — | `inbox.compose.from` |
| Trigger panel | `m3.Card` (`card_trigger`, outlined) | Event type, segment and delay before the first email (UF-23 step 5) | `surface-card` | mobile | — |
| Trigger event selector | `m3.DropdownMenu` (`dd_trigger_event`) | The trigger event type | `outlined-select` | — | `campaigns.trigger.event` |
| Segment selector | `m3.DropdownMenu` (`dd_campaign_segment`, `allow_none=True`) | The segment; empty = all contacts not opted out (schema) | `outlined-select` | — | `crm.segments.title` |
| Delay field | `m3.TextBox` (`txt_trigger_delay`, type `number`, outlined) | Days before the first email | `outlined-input` | — | `campaigns.trigger.delay` |
| Activate button | `m3.Button` (`btn_activate_campaign`, filled) | Confirm settings → Activate Campaign (UF-23 step 6) | `primary-button` | — | `campaigns.activate` |
| Save draft button | `m3.Button` (`btn_save_campaign`, outlined) | Saves without activating | `secondary-button` | — | `invoices.save_draft` |
| Cancel button | `m3.Button` (`btn_cancel_campaign`, text) | Returns to the list | `tertiary-button` | — | `editmode.bar.cancel` |
| Help icon | `components.HelpTooltipComponent` (help entry `crm.campaign_editor`) | The feature's tooltip and help video | — | — | `help.crm.campaign_editor.tooltip` |

## 3. Data

| Element | Table | Column |
|---|---|---|
| Campaign | `email_campaigns` | `campaign_name`, `campaign_type`, `campaign_settings` (emails, delays, trigger), `segment_id`, `from_address_id`, `status` |
| Enrolments | `contact_campaigns` | created as the campaign runs |

## 4. Actions

| Action | Event handler | Server function | Result |
|---|---|---|---|
| Load the campaign | `form_show` → `load_campaign` | the campaign service read | The type's emails and delays load (the ready-made sequences) |
| Choose the type | `dd_campaign_type_change` | the campaign service read | The ready-made emails fill the sequence preview |
| Edit an email | `ibtn_edit_email_*_click` | the campaign service save | The email's subject and body are edited |
| Activate | `btn_activate_campaign_click` | the campaign service save | The campaign runs automatically: contacts are enrolled, emails are sent on schedule from the campaign's sender address, and opens, clicks, bounces and unsubscribes are reported by Amazon SES (UF-23 step 7) |
| Save draft | `btn_save_campaign_click` | the campaign service save | The campaign is saved as `draft` |

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
| `campaigns.name` | Campaign name |
| `campaigns.type.ready` | Start from |
| `campaigns.trigger.event` | Trigger |
| `campaigns.trigger.delay` | Days before the first email |
| `campaigns.activate` | Activate Campaign |
| `campaigns.activated` | Campaign active. Contacts are enrolled as they qualify. |
| `campaigns.trigger.enquiry` | Enquiry received |
| `campaigns.trigger.booking` | Booking completed |

## 8. Help Videos

| Process | Help video |
|---|---|
| Setting up a sequence | `master_template` help entry `crm.campaign_editor` — played in the app language |

## 9. User Flows

| Flow | Part this form plays |
|---|---|
| UF-23 Email campaign setup | The whole flow: type, review, trigger, activate |
| UF-20 Lead capture | The welcome sequence new leads are enrolled in |
