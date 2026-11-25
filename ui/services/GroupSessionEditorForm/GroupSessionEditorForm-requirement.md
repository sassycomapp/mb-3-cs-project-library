---
title: "GroupSessionEditorForm-requirement"
status: draft
doc-id: "mb-035a25d1"
updated: "2026-11-25"
---
# GroupSessionEditorForm — Screen Requirement

## 1. Form

| Item | Value |
|---|---|
| Module path | `services.GroupSessionEditorForm` |
| Purpose | Sessions of a group service: date, time, provider, place limit, location or call link (ticket 21, F-11; UF-07 set-up) |
| Screen type and width | Editor, viewer, form — 900 px, sectioned panels |
| Layout | `layouts.AdminLayout` |
| Route | None — opened from the group service's record |

## 2. Elements

| Element | Theme component | Why this component | Role | `wrap_on` | Tooltip phrase key |
|---|---|---|---|---|---|
| Page title | `m3.Heading` (`hdg_group_session`) | The group service's name and "sessions", `headline-large` | `page-title` | — | `services.group_sessions.title` |
| Sessions list | Anvil `ColumnPanel` (`col_sessions`) | The service's upcoming sessions in time order | — | mobile | — |
| Session row | `m3.ListItem` (`li_session_*`, clickable) | One session per row: date and time, provider, places taken against the limit, status | — | — | — |
| Add session button | `m3.Button` (`btn_add_session`, filled) | Opens the session fields | `primary-button` | — | `services.group_sessions.add` |
| Session fields panel | `m3.Card` (`card_session_fields`, outlined) | The session's fields | `surface-card` | mobile | — |
| Date field | `m3.DatePicker` (`dp_session_date`, outlined) | The session's date | `outlined-date` | — | `services.session.date` |
| Start time field | `m3.TimePicker` (`tp_start`, outlined) | The session's start | `outlined-input` | — | `services.session.start` |
| End time field | `m3.TimePicker` (`tp_end`, outlined) | The session's end | `outlined-input` | — | `services.session.end` |
| Provider selector | `m3.DropdownMenu` (`dd_session_provider`) | The provider delivering the session | `outlined-select` | — | `services.session.provider` |
| Resource selector | `m3.DropdownMenu` (`dd_session_resource`, `allow_none=True`) | An optional room or equipment | `outlined-select` | — | `services.session.resource` |
| Place limit field | `m3.TextBox` (`txt_place_limit`, type `number`, outlined) | The session's place limit | `outlined-input` | — | `services.session.limit` |
| Location field | `m3.TextBox` (`txt_session_location`, outlined) | For an in-person session | `outlined-input` | — | `services.session.location` |
| Call link field | `m3.TextBox` (`txt_call_link`) | For an online session | `outlined-input` | — | `services.session.call_link` |
| Save session button | `m3.Button` (`btn_save_session`, filled) | Saves the session | `primary-button` | — | `services.session.save` |
| Cancel session button | `m3.Button` (`btn_cancel_session`, text) | Discards the session fields | `tertiary-button` | — | `editmode.bar.cancel` |
| Cancel session action | `m3.Button` (`btn_cancel_booked_session`, outlined, on a row) | Cancels a scheduled session; booked places are handled with the bookings | `secondary-button` | — | `services.session.cancel` |
| Help icon | `components.HelpTooltipComponent` (help entry `services.group_sessions`) | The feature's tooltip and help video | — | — | `help.services.group_sessions.tooltip` |

## 3. Data

| Element | Table | Column |
|---|---|---|
| Sessions | `group_sessions` | `service_id`, `provider_id`, `resource_id`, `start_datetime`, `end_datetime`, `place_limit`, `places_taken`, `location`, `call_link`, `status` |

## 4. Actions

| Action | Event handler | Server function | Result |
|---|---|---|---|
| Load the sessions | `form_show` → `load_sessions` | `group_session_service` list read (`server_services/group_session_service`) | The sessions arrive |
| Save a session | `btn_save_session_click` | `group_session_service` save | The session is saved; `places_taken` starts at 0 |
| Cancel a session | `btn_cancel_booked_session_click` | `group_session_service` cancel | The session is cancelled; its bookings are cancelled in the same call, and package sessions are returned where the window allows |
| Open a session | `li_session_*_click` | None | The session's fields open for editing |

## 5. Empty State

| Part | Content |
|---|---|
| Icon | Group icon (`mi:groups`) |
| Heading | `services.group_sessions.empty.title` |
| Body text | `services.group_sessions.empty.body` |
| Primary action | `services.group_sessions.add` |

## 6. Access

| Role | Scope |
|---|---|
| Owner | all |
| Manager | all |
| Admin | all |
| Staff | none |
| Customer | none |
| SignedOut | none |

Per the access matrix (`services.GroupSessionEditorForm`: Owner, Manager, Admin).

## 7. Text

| Phrase key | English text |
|---|---|
| `services.group_sessions.title` | Group sessions |
| `services.group_sessions.add` | + Add session |
| `services.session.date` | Date |
| `services.session.start` | Start |
| `services.session.end` | End |
| `services.session.provider` | Provider |
| `services.session.resource` | Room or equipment |
| `services.session.limit` | Places |
| `services.session.location` | Location |
| `services.session.call_link` | Call link |
| `services.session.save` | Save session |
| `services.session.cancel` | Cancel session |
| `services.group_sessions.empty.title` | No sessions yet |
| `services.group_sessions.empty.body` | Add the times this group service runs. Customers book places in them. |

## 8. Help Videos

| Process | Help video |
|---|---|
| Setting up group sessions | `master_template` help entry `services.group_sessions` — played in the app language |

## 9. User Flows

| Flow | Part this form plays |
|---|---|
| UF-07 Group session booking | The set-up half: add the session; the customer books places in it |
| UF-11 Admin daily operations | The calendar shows the sessions |
