---
title: "TimeEntryEditorForm-requirement"
status: draft
doc-id: "mb-6ac62919"
updated: "2026-11-25"
---
# TimeEntryEditorForm — Screen Requirement

## 1. Form

| Item | Value |
|---|---|
| Module path | `time_tracking.TimeEntryEditorForm` |
| Purpose | Manual entry and the start/stop timer (ticket 28, F-14; UF-13) |
| Screen type and width | Editor, viewer, form — 900 px, sectioned panels |
| Layout | `layouts.AdminLayout` |
| Route | None — opened from `time_tracking.TimeEntryListForm` |

## 2. Elements

| Element | Theme component | Why this component | Role | `wrap_on` | Tooltip phrase key |
|---|---|---|---|---|---|
| Page title | `m3.Heading` (`hdg_time_editor`) | "New entry" / the entry's date, `headline-large` | `page-title` | — | `time.new` |
| Start timer button | `m3.Button` (`btn_start_timer`, filled, icon `mi:play_arrow`) | Start the timer on a customer and service (UF-13 step 1) | `primary-button` | — | `time.start_timer` |
| Stop timer button | `m3.Button` (`btn_stop_timer`, outlined, hidden) | Stop the timer; duration and total are calculated (UF-13 step 3) | `secondary-button` | — | `time.stop_timer` |
| Running time display | `m3.Text` (`text_timer_running`) | The running timer's elapsed time | `body-medium` | — | — |
| Timer interval | Anvil `Timer` (`tmr_timer`) | Ticks the running display every second while the timer runs | — | — | — |
| Customer selector | `m3.DropdownMenu` (`dd_entry_customer`) | The customer the work is for | `outlined-select` | — | `invoices.customer` |
| Service selector | `m3.DropdownMenu` (`dd_entry_service`) | The service; its hourly rate is the default and can be changed on the entry (UF-13 step 2) | `outlined-select` | — | `services.name` |
| Booking selector | `m3.DropdownMenu` (`dd_entry_booking`, `allow_none=True`) | The optional booking the entry is from (`time_entries.booking_id`) | `outlined-select` | — | `bookings.title` |
| Start time field | `m3.TimePicker` (`tp_entry_start`, outlined) | The work's start (manual entry) | `outlined-input` | — | `services.session.start` |
| End time field | `m3.TimePicker` (`tp_entry_end`, outlined) | The work's end; empty while the timer runs | `outlined-input` | — | `services.session.end` |
| Hourly rate field | `m3.TextBox` (`txt_hourly`, type `number`, outlined) | The rate, changeable per entry | `outlined-input` | — | `services.hourly_rate` |
| Billable switch | `m3.Switch` (`sw_billable`) | Mark the entry billable or not (UF-13 step 2) | — | — | `time.billable` |
| Description field | `m3.TextBox` (`txt_entry_description`, outlined) | What the work was | `outlined-input` | — | `time.description` |
| Totals display | `m3.Text` (`text_entry_totals`) | Duration and total, calculated on save | `body-medium` | — | `time.duration`, `time.total` |
| Save button | `m3.Button` (`btn_save_entry`, filled) | Saves the entry | `primary-button` | — | `services.session.save` |
| Cancel button | `m3.Button` (`btn_cancel_entry`, text) | Returns without saving | `tertiary-button` | — | `editmode.bar.cancel` |
| Help icon | `components.HelpTooltipComponent` (help entry `time.editor`) | The feature's tooltip and help video | — | — | `help.time.editor.tooltip` |

## 3. Data

| Element | Table | Column |
|---|---|---|
| Fields | `time_entries` | `contact_id`, `service_id`, `booking_id`, `start_time`, `end_time`, `duration_hours`, `hourly_rate`, `total_amount`, `description`, `is_billable` |

## 4. Actions

| Action | Event handler | Server function | Result |
|---|---|---|---|
| Start the timer | `btn_start_timer_click` | `time_entry_service` start | An entry opens with its start set and end empty |
| Stop the timer | `btn_stop_timer_click` | `time_entry_service` stop | The end is set; duration and total are calculated on save |
| Save | `btn_save_entry_click` | `time_entry_service` save | The entry is saved with duration and total calculated on save |
| Cancel | `btn_cancel_entry_click` | None | The list opens without saving |

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
| Staff | all |
| Customer | none |
| SignedOut | none |

Staff own only.

## 7. Text

| Phrase key | English text |
|---|---|
| `time.new` | New entry |
| `time.start_timer` | Start timer |
| `time.stop_timer` | Stop timer |
| `time.billable` | Billable |
| `time.description` | What was the work? |
| `time.duration` | {hours} hours |
| `time.total` | {total} |
| `time.editor.timer_running` | Timer running — {elapsed} |

## 8. Help Videos

| Process | Help video |
|---|---|
| Logging time | `master_template` help entry `time.editor` — played in the app language |
| Using the timer | `master_template` help entry `time.timer` |

## 9. User Flows

| Flow | Part this form plays |
|---|---|
| UF-13 Time tracking and invoicing hours | The logging half: timer and manual entry |
