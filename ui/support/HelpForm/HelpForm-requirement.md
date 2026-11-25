---
title: "HelpForm-requirement"
status: draft
doc-id: "mb-fdfa8737"
updated: "2026-11-25"
---
# HelpForm — Screen Requirement

## 1. Form

| Item | Value |
|---|---|
| Module path | `support.HelpForm` |
| Purpose | Problem or feedback: subject, message, optional screenshot; the reference number shown on send; replies appear in the Help view with an email when Mybizz replies (ticket 49, F-32; UF-34) |
| Screen type and width | Editor, viewer, form — 900 px, sectioned panels |
| Layout | `layouts.AdminLayout` (the Help item in the admin navigation) |
| Route | None — opened from the admin navigation |

## 2. Elements

| Element | Theme component | Why this component | Role | `wrap_on` | Tooltip phrase key |
|---|---|---|---|---|---|
| Page title | `m3.Heading` (`hdg_help`) | "Help", `headline-large` | `page-title` | — | `help.title` |
| Type selector | `m3.ButtonGroup` (`bgrp_help_type`, connected, single) | Problem or feedback (UF-34 step 1) | — | mobile | `help.type.problem`, `help.type.feedback` |
| Subject field | `m3.TextBox` (`txt_help_subject`, outlined) | The subject | `outlined-input` | — | `inbox.compose.subject` |
| Message field | `m3.TextArea` (`txt_help_message`) | The message | `outlined-input` | mobile | `inbox.compose.body` |
| Screenshot field | `m3.FileLoader` (`fu_help_screenshot`, `file_types="image/*"`) | An optional screenshot (UF-34 step 2) | — | — | `help.screenshot` |
| Send button | `m3.Button` (`btn_send_help`, filled) | The message goes to App 5 and a reference number is shown (UF-34 step 3) | `primary-button` | — | `help.send` |
| Reference display | `m3.Text` (`text_help_reference`, hidden) | The reference number | `body-medium` | — | `help.reference` |
| Replies panel | `m3.Card` (`card_help_replies`, outlined) | The replies shown in the Help view (UF-34 step 4) | `surface-card` | mobile | — |
| Reply row | `m3.ListItem` (`li_help_reply_*`) | One reply per row: from Mybizz, when, what | — | — | — |
| Help icon | `components.HelpTooltipComponent` (help entry `support.help`) | The feature's tooltip and help video | — | — | `help.support.help.tooltip` |

## 3. Data

| Element | Table | Column |
|---|---|---|
| The message | — | Held by App 5 (adr-schema-scope-reduction: no `tickets` tables in the instance) |
| Replies | — | Read through the management endpoints (ticket 49) |

## 4. Actions

| Action | Event handler | Server function | Result |
|---|---|---|---|
| Send | `btn_send_help_click` | `help_service` send (`server_support/help_service`) | The message goes to App 5 with the instance's Bearer token; the reference number is shown; a problem that stops the business is acknowledged within 4 business hours, every other ticket within 24 hours on business days |
| Read the replies | `form_show` → `load_replies` | `help_service` replies read | The replies arrive; the user is emailed when Mybizz replies |

## 5. Empty State

| Part | Content |
|---|---|
| Icon | Support icon (`mi:support_agent`) |
| Heading | `help.replies.empty.title` |
| Body text | `help.replies.empty.body` |
| Primary action | `help.send` (Send) |

## 6. Access

| Role | Scope |
|---|---|
| Owner | all |
| Manager | all |
| Admin | all |
| Staff | all |
| Customer | none |
| SignedOut | none |

Every team member reaches Help (architecture reference section 9).

## 7. Text

| Phrase key | English text |
|---|---|
| `help.title` | Help |
| `help.type.problem` | Problem |
| `help.type.feedback` | Feedback |
| `help.send` | Send |
| `help.reference` | Your reference: {reference} |
| `help.screenshot` | Attach a screenshot |
| `help.replies.title` | Replies from Mybizz |
| `help.replies.empty.title` | No replies yet |
| `help.replies.empty.body` | When Mybizz replies, it appears here and you get an email. |

## 8. Help Videos

| Process | Help video |
|---|---|
| Getting help | `master_template` help entry `support.help` — played in the app language |

## 9. User Flows

| Flow | Part this form plays |
|---|---|
| UF-34 Help: support tickets and feedback | The whole flow |
| UF-38 Error recovery | The last resort when a screen cannot recover |
