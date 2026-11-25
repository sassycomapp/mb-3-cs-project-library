---
title: "BookingCalendarDayForm-requirement"
status: draft
doc-id: "mb-b09d9136"
updated: "2026-11-25"
---
# BookingCalendarDayForm — Screen Requirement

## 1. Form

| Item | Value |
|---|---|
| Module path | `bookings.BookingCalendarForm.BookingCalendarDayForm` |
| Purpose | The day view of the booking calendar: the day's bookings and blocked periods in time order (ticket 24, F-11; spec-calendar-feature section 4.3) |
| Screen type and width | List — 1200 px |
| Layout | Hosted by `bookings.BookingCalendarForm`'s content panel |
| Route | None — a view form of the calendar |

## 2. Elements

Per `spec-calendar-feature.md` section 4.3:

| Element | Theme component | Why this component | Role | `wrap_on` | Tooltip phrase key |
|---|---|---|---|---|---|
| Day container | Anvil `ColumnPanel` (`col_day`) | The spec's root container | — | mobile | — |
| Day summary | `m3.Text` (`text_day_summary`) | e.g. "6 bookings · 1 group session · closed 13:00–14:00" | `body-medium` | — | — |
| Events list | Anvil `RepeatingPanel` (`rp_day_events`) with item template `bookings.BookingCalendarForm.tmpl_day_event` | Bookings and blocked periods in time order | — | — | — |

## 3. Data

| Element | Table | Column |
|---|---|---|
| The day's items | `bookings`, `group_sessions`, `availability_exceptions` | via `get_calendar_items`, passed in `self.item` by the controller |

## 4. Actions

| Action | Event handler | Server function | Result |
|---|---|---|---|
| Render the day | the controller's render | None | The day's items render in time order |
| Open a booking | the event card's click | None | `bookings.BookingViewerForm` opens |
| Open an empty slot | the day's click on a free stretch | None | `bookings.BookingEditorForm` opens with provider, date and time filled in |

## 5. Empty State

| Part | Content |
|---|---|
| Icon | Calendar icon (`mi:event_busy`) |
| Heading | `calendar.empty.title` |
| Body text | `calendar.empty.body` — the day view's empty state when the day holds nothing |
| Primary action | `bookings.add` |

## 6. Access

| Role | Scope |
|---|---|
| Owner | all |
| Manager | all |
| Admin | all |
| Staff | all |
| Customer | none |
| SignedOut | none |

Same as the calendar; Staff see only their own bookings.

## 7. Text

| Phrase key | English text |
|---|---|
| `calendar.day.summary.bookings` | {count} bookings |
| `calendar.day.summary.sessions` | {count} group session |
| `calendar.day.summary.closed` | closed {from}–{to} |

## 8. Help Videos

| Process | Help video |
|---|---|
| None | The calendar controller's video covers the views |

## 9. User Flows

| Flow | Part this form plays |
|---|---|
| UF-11 Admin daily operations | The day's detail; the default view below 600 px |
