---
title: "ComposeForm-requirement"
status: draft
doc-id: "mb-302109d8"
updated: "2026-11-25"
---
# ComposeForm — Screen Requirement

## 1. Form

| Item | Value |
|---|---|
| Module path | `inbox.ComposeForm` |
| Purpose | New email, reply and forward: the compose side sheet on desktop and full screen on a phone, with drafts saved automatically and the AI draft reply (ticket 29, F-18; UF-35; DESIGN.md Team Inbox Design) |
| Screen type and width | Editor, viewer, form — 900 px, sectioned panels (as the side sheet's content) |
| Layout | Hosted by `inbox.TeamInboxForm`'s sidesheet (the layout's sidesheet per spec-ui-standards section 8.7) |
| Route | None — opened from the Team Inbox |

## 2. Elements

| Element | Theme component | Why this component | Role | `wrap_on` | Tooltip phrase key |
|---|---|---|---|---|---|
| From selector | `m3.DropdownMenu` (`dd_from`) | The sending address, from the user's own addresses and desks | `outlined-select` | — | `inbox.compose.from` |
| To field | `m3.TextBox` (`txt_to`, outlined) | Recipients; contacts are suggested while typing (UF-35 step 4) | `outlined-input` | — | `inbox.compose.to` |
| Cc field | `m3.TextBox` (`txt_cc`, outlined) | Cc | `outlined-input` | — | `inbox.compose.cc` |
| Bcc field | `m3.TextBox` (`txt_bcc`, outlined) | Bcc | `outlined-input` | — | `inbox.compose.bcc` |
| Subject field | `m3.TextBox` (`txt_subject`, outlined) | The subject | `outlined-input` | — | `inbox.compose.subject` |
| Body editor | `m3.TextArea` (`txt_body`) | The rich text body (RichText is for user-formatted text, spec-ui-standards section 3.9) | `outlined-input` | mobile | `inbox.compose.body` |
| Attachments | `m3.FileLoader` (`fu_attachments`, `multiple=True`) | The attachments | — | — | `orders.brief_files` |
| Signature display | `m3.Text` (`text_signature`) | The address's signature is added (UF-35 step 4) | `body-small` | — | — |
| AI draft button | `m3.Button` (`btn_ai_draft`, outlined, icon `mi:auto_awesome`) | Ask the AI helper for a draft reply, in the app language, and edit it (UF-35 step 5; AI assist 6) | `secondary-button` | — | `inbox.compose.ai_draft` |
| Send button | `m3.Button` (`btn_send_email`, filled) | The compose's primary action | `primary-button` | — | `inbox.compose.send` |
| Discard button | `m3.Button` (`btn_discard`, text) | Discards the draft (DESIGN.md Team Inbox Design) | `tertiary-button` | — | `inbox.compose.discard` |
| Draft autosave notice | `m3.Text` (`text_draft_saved`, hidden) | A message being written is saved to Drafts every few seconds (spec-team-inbox section 6) | `body-small` | — | `inbox.compose.draft_saved` |
| Help icon | `components.HelpTooltipComponent` (help entry `inbox.compose`) | The feature's tooltip and help video | — | — | `help.inbox.compose.tooltip` |

## 3. Data

| Element | Table | Column |
|---|---|---|
| The message | `email_messages` | `address_id`, `folder` (`sent`/`drafts`), `direction` (`outgoing`), `to_addresses`, `cc_addresses`, `subject`, `body_text`, `body_html`, `attachments`, `in_reply_to`, `thread_id` |
| Signature | `email_addresses` | `signature` |
| Draft autosave | `email_messages` | the drafts row updated as written |

## 4. Actions

| Action | Event handler | Server function | Result |
|---|---|---|---|
| Draft autosave | `tmr_draft_save` (Timer, every few seconds) | `inbox_service` draft save | The draft is saved to Drafts while it is written |
| AI draft reply | `btn_ai_draft_click` | `request(assist="email_response", ...)` (`server_ai/ai_request_layer`) | The draft reply appears for editing, in the app language |
| Send | `btn_send_email_click` | `send_email` (`server_shared/email_service`; from own addresses only) | The email leaves from the client's own domain and is saved in Sent |
| Discard | `btn_discard_click` | `inbox_service` discard | The draft is discarded |

## 5. Empty State

| Part | Content |
|---|---|
| Icon | None — the compose always shows its fields |
| Heading | None — not applicable |
| Body text | None — not applicable |
| Primary action | None — not applicable |

## 6. Access

| Role | Scope |
|---|---|
| Owner | all |
| Manager | all |
| Admin | all |
| Staff | all |
| Customer | none |
| SignedOut | none |

Sending is from own addresses only (`send_email`: Owner all, Manager all, Admin none, Staff own).

## 7. Text

| Phrase key | English text |
|---|---|
| `inbox.compose.from` | From |
| `inbox.compose.to` | To |
| `inbox.compose.cc` | Cc |
| `inbox.compose.bcc` | Bcc |
| `inbox.compose.subject` | Subject |
| `inbox.compose.body` | Write your email |
| `inbox.compose.ai_draft` | Draft with AI |
| `inbox.compose.send` | Send |
| `inbox.compose.discard` | Discard |
| `inbox.compose.draft_saved` | Draft saved. |

## 8. Help Videos

| Process | Help video |
|---|---|
| Writing and sending email | `master_template` help entry `inbox.compose` — played in the app language |
| The AI draft reply | `master_template` help entry `inbox.ai_draft` |

## 9. User Flows

| Flow | Part this form plays |
|---|---|
| UF-35 Team Inbox | Reply, Reply All, Forward, New Email; the AI draft |
| UF-30 Using the AI helper | The email response assist (assist 6) |
