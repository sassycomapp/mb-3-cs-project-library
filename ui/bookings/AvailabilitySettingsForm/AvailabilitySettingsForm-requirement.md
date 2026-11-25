---
title: "AvailabilitySettingsForm-requirement"
status: draft
doc-id: "mb-b1ffc43b"
updated: "2026-11-25"
---
# AvailabilitySettingsForm — Screen Requirement

## 1. Form

| Item | Value |
|---|---|
| Module path | `bookings.AvailabilitySettingsForm` |
| Purpose | Business hours, provider schedules, blocked dates and exceptions (ticket 24, F-11) |
| Screen type and width | Editor, viewer, form — 900 px, sectioned panels |
| Layout | `layouts.AdminLayout` |
| Route | None — opened from the admin navigation (Services → Availability) |

## 2. Elements

| Element | Theme component | Why this component | Role | `wrap_on` | Tooltip phrase key |
|---|---|---|---|---|---|
| Page title | `m3.Heading` (`hdg_availability`) | "Availability", `headline-large` | `page-title` | — | `availability.title` |
| Business hours panel | `m3.Card` (`card_business_hours`, outlined) | The business's weekly hours (`availability_rules` rows with no provider) | `surface-card` | mobile | — |
| Day rule row | Anvil `FlowPanel` (`flow_hours_day_*`) | One row per weekday: day, start, end, active | — | mobile | — |
| Day label | `m3.Text` (`text_day_*`) | Monday to Sunday | `body-medium` | — | `availability.day.monday` … `availability.day.sunday` |
| Start time field | `m3.TimePicker` (`tp_start_*`, outlined) | The day's start | `outlined-input` | — | `services.session.start` |
| End time field | `m3.TimePicker` (`tp_end_*`, outlined) | The day's end | `outlined-input` | — | `services.session.end` |
| Day active switch | `m3.Switch` (`sw_day_*`) | The day's availability (`availability_rules.is_active`) | — | — | `availability.day.active` |
| Provider panel | `m3.Card` (`card_provider_hours`, outlined) | Each provider's schedule (`availability_rules` rows with a provider) | `surface-card` | mobile | — |
| Provider selector | `m3.DropdownMenu` (`dd_provider`) | The provider whose schedule is edited | `outlined-select` | — | `services.session.provider` |
| Provider day rule rows | as the business hours rows | The provider's weekly hours | — | mobile | — |
| Exceptions panel | `m3.Card` (`card_exceptions`, outlined) | Blocked dates and one-off changes (`availability_exceptions`) | `surface-card` | mobile | — |
| Exception date field | `m3.DatePicker` (`dp_exception_date`, outlined) | The blocked or changed date | `outlined-date` | — | `availability.exception.date` |
| Exception scope selector | `m3.DropdownMenu` (`dd_exception_provider`, `allow_none=True`) | Empty = the whole business; a provider = that provider | `outlined-select` | — | `availability.exception.scope` |
| Whole-day switch | `m3.Switch` (`sw_exception_all_day`) | Empty times = the whole day (`availability_exceptions` start/end empty) | — | — | `availability.exception.all_day` |
| Exception times fields | `m3.TimePicker` (`tp_exception_start`, `tp_exception_end`, outlined) | A partial block or one-off change | `outlined-input` | — | `services.session.start` |
| Available switch | `m3.Switch` (`sw_exception_available`) | False = blocked; True = one-off availability | — | — | `availability.exception.available` |
| Reason field | `m3.TextBox` (`txt_exception_reason`, outlined) | Shown on the calendar as the block's title (`availability_exceptions.reason`) | `outlined-input` | — | `availability.exception.reason` |
| Add exception button | `m3.Button` (`btn_add_exception`, filled) | Adds the exception | `primary-button` | — | `availability.exception.add` |
| Exception list | Anvil `ColumnPanel` (`col_exception_rows`) | The exceptions in date order with delete actions | — | mobile | — |
| Delete exception button | `m3.IconButton` (`ibtn_delete_exception_*`, icon `mi:delete`) | Removes the exception | `icon-button` | — | `availability.exception.delete` |
| Save button | `m3.Button` (`btn_save_availability`, filled) | Saves the weekly hours | `primary-button` | — | `availability.save` |
| Help icon | `components.HelpTooltipComponent` (help entry `bookings.availability`) | The feature's tooltip and help video | — | — | `help.bookings.availability.tooltip` |

## 3. Data

| Element | Table | Column |
|---|---|---|
| Weekly hours | `availability_rules` | `provider_id` (empty = the business), `day_of_week` (0 Monday – 6 Sunday), `start_time`, `end_time`, `is_active` |
| Exceptions | `availability_exceptions` | `provider_id`, `exception_date`, `start_time`, `end_time`, `is_available`, `reason` |

## 4. Actions

| Action | Event handler | Server function | Result |
|---|---|---|---|
| Load the form | `form_show` → `load_availability` | `availability_service` read (`server_bookings/availability_service`) | The rules and exceptions arrive |
| Save weekly hours | `btn_save_availability_click` | `availability_service` save | The weekly rules are saved; slots recompute from them |
| Add an exception | `btn_add_exception_click` | `availability_service` save | The exception is saved and shown on the calendar |
| Delete an exception | `ibtn_delete_exception_*_click` | `availability_service` delete | The exception is removed |

## 5. Empty State

| Part | Content |
|---|---|
| Icon | Schedule icon (`mi:schedule`) |
| Heading | `availability.empty.title` |
| Body text | `availability.empty.body` |
| Primary action | `availability.save` |

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
| `availability.title` | Availability |
| `availability.day.monday` | Monday |
| `availability.day.tuesday` | Tuesday |
| `availability.day.wednesday` | Wednesday |
| `availability.day.thursday` | Thursday |
| `availability.day.friday` | Friday |
| `availability.day.saturday` | Saturday |
| `availability.day.sunday` | Sunday |
| `availability.day.active` | Open |
| `availability.exception.date` | Date |
| `availability.exception.scope` | Who it applies to |
| `availability.exception.all_day` | All day |
| `availability.exception.available` | Available |
| `availability.exception.reason` | Reason |
| `availability.exception.add` | + Add blocked date |
| `availability.exception.delete` | Remove |
| `availability.save` | Save hours |
| `availability.empty.title` | No hours set yet |
| `availability.empty.body` | Set your opening hours; your providers get their own schedules too. |

## 8. Help Videos

| Process | Help video |
|---|---|
| Setting opening hours and provider schedules | `master_template` help entry `bookings.availability` — played in the app language |
| Blocking dates | `master_template` help entry `bookings.exceptions` |

## 9. User Flows

| Flow | Part this form plays |
|---|---|
| UF-05–UF-08 Bookings | The rules the slot calculation runs from |
| UF-03 Staff invitation | The provider's schedule is set here (UF-03 step 2) |
| UF-11 Admin daily operations | The calendar's blocked periods come from here |
