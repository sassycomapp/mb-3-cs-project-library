---
title: "EmailBroadcastForm-requirement"
status: draft
doc-id: "mb-72c198f6"
updated: "2026-11-25"
---
# EmailBroadcastForm — Screen Requirement

## 1. Form

| Item | Value |
|---|---|
| Module path | `crm.EmailBroadcastForm` |
| Purpose | The broadcast editor: recipients, sender, the email editor or a template, preview, test, send or schedule, and the results (ticket 29, F-18; UF-24) |
| Screen type and width | Editor, viewer, form — 900 px, sectioned panels |
| Layout | `layouts.AdminLayout` |
| Route | None — opened from `crm.EmailCampaignListForm` |

## 2. Elements

| Element | Theme component | Why this component | Role | `wrap_on` | Tooltip phrase key |
|---|---|---|---|---|---|
| Page title | `m3.Heading` (`hdg_broadcast_editor`) | "New broadcast" / the broadcast's name, `headline-large` | `page-title` | — | `broadcasts.new` |
| Name field | `m3.TextBox` (`txt_broadcast_name`, outlined) | The broadcast's name | `outlined-input` | — | `campaigns.name` |
| Recipients selector | `m3.DropdownMenu` (`dd_broadcast_segment`) | A segment, or all contacts who have not unsubscribed (UF-24 step 1) | `outlined-select` | — | `broadcasts.recipients` |
| Sender selector | `m3.DropdownMenu` (`dd_broadcast_sender`) | The sending address, e.g. `marketing@` | `outlined-select` | — | `inbox.compose.from` |
| Subject field | `m3.TextBox` (`txt_broadcast_subject`, outlined) | The subject | `outlined-input` | — | `inbox.compose.subject` |
| Body editor | `m3.TextArea` (`txt_broadcast_body`) | Write the email in the email editor or start from a template (UF-24 step 2) | `outlined-input` | mobile | `inbox.compose.body` |
| Template selector | `m3.DropdownMenu` (`dd_broadcast_template`, `allow_none=True`) | The ready-made templates (`email_templates`) | `outlined-select` | — | `broadcasts.template` |
| Preview action | `m3.Button` (`btn_preview_broadcast`, outlined) | Preview (UF-24 step 2) | `secondary-button` | — | `broadcasts.preview` |
| Test action | `m3.Button` (`btn_test_broadcast`, text) | Send a test | `tertiary-button` | — | `broadcasts.test` |
| Send now button | `m3.Button` (`btn_send_broadcast`, filled) | Send now (UF-24 step 3) | `primary-button` | — | `broadcasts.send_now` |
| Schedule action | `m3.DatePicker` (`dp_broadcast_when`) + `m3.TimePicker` (`tp_broadcast_time`) + `m3.Button` (`btn_schedule_broadcast`, outlined) | Or schedule | `outlined-date` | mobile | `broadcasts.schedule` |
| Results panel | `m3.Card` (`card_broadcast_results`, outlined) | Sent, opens, clicks, unsubscribes (UF-24 step 4) | `surface-card` | mobile | — |
| Help icon | `components.HelpTooltipComponent` (help entry `crm.broadcast`) | The feature's tooltip and help video | — | — | `help.crm.broadcast.tooltip` |

## 3. Data

| Element | Table | Column |
|---|---|---|
| Broadcast | `email_campaigns` | `campaign_name`, `campaign_type` (`broadcast`), `campaign_settings`, `segment_id`, `from_address_id`, `status` |
| Results | `email_campaigns` | `emails_sent`, `opens`, `clicks`, `unsubscribes` |
| Suppressions | `email_suppressions` | honoured within scope (marketing email only for unsubscribes) |

## 4. Actions

| Action | Event handler | Server function | Result |
|---|---|---|---|
| Load the broadcast | `form_show` → `load_broadcast` | the campaign service read | The fields fill |
| Preview | `btn_preview_broadcast_click` | the campaign service preview | The email's preview shows |
| Send a test | `btn_test_broadcast_click` | `send_email` (`server_shared/email_service`) | The test goes to the sender |
| Send now | `btn_send_broadcast_click` | the campaign service send | Each recipient receives the email individually, in batches within the SES rate; every marketing email carries the unsubscribe link and headers (spec-team-inbox section 8) |
| Schedule | `btn_schedule_broadcast_click` | the campaign service schedule | The broadcast is scheduled |

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
| `broadcasts.recipients` | Recipients |
| `broadcasts.recipients.all` | All contacts who haven't unsubscribed |
| `broadcasts.template` | Start from a template |
| `broadcasts.preview` | Preview |
| `broadcasts.test` | Send a test |
| `broadcasts.send_now` | Send now |
| `broadcasts.schedule` | Schedule |
| `broadcasts.sent_to` | Sent to {count} contacts |
| `broadcasts.results.title` | Results |

## 8. Help Videos

| Process | Help video |
|---|---|
| Sending a broadcast | `master_template` help entry `crm.broadcast` — played in the app language |

## 9. User Flows

| Flow | Part this form plays |
|---|---|
| UF-24 Email broadcast | The whole flow: recipients, editor, preview, test, send or schedule, results |
| UF-46 Sharing the referral kit | The referral banner rides the client's campaign emails when `referral_banner_in_emails` is set |
