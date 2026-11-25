---
title: "BookingCalendarWeekForm-requirement"
status: draft
doc-id: "mb-05f05e61"
updated: "2026-11-25"
---
# BookingCalendarWeekForm — Screen Requirement

## 1. Form

| Item | Value |
|---|---|
| Module path | `bookings.BookingCalendarForm.BookingCalendarWeekForm` |
| Purpose | The week view of the booking calendar: seven days across, times down (ticket 24, F-11; spec-calendar-feature section 4.2) |
| Screen type and width | List — 1200 px |
| Layout | Hosted by `bookings.BookingCalendarForm`'s content panel |
| Route | None — a view form of the calendar |

## 2. Elements

Per `spec-calendar-feature.md` section 4.2:

| Element | Theme component | Why this component | Role | `wrap_on` | Tooltip phrase key |
|---|---|---|---|---|---|
| Week container | Anvil `FlowPanel` (`flow_week`) | The spec's root; the days run across | — | mobile (the week view is not offered below 600 px — the day view is used instead) | — |
| Time labels column | Anvil `ColumnPanel` (`col_time_labels`) | The spec's time column | — | never | — |
| Time labels | Anvil `RepeatingPanel` (`rp_time_labels`) with item template `bookings.BookingCalendarForm.tmpl_week_timeslot` | One row per time slot, e.g. 07:00, 07:30 … | — | — | — |
| Day columns | Anvil `RepeatingPanel` (`rp_week_days`) with item template `bookings.BookingCalendarForm.tmpl_week_day_column` | Seven columns, Monday to Sunday | — | — | — |

The time rows run from the earliest business-hours start to the latest business-hours end in the week, in steps of the shortest service duration rounded to 15, 30 or 60 minutes (spec-calendar section 4.2).

## 3. Data

| Element | Table | Column |
|---|---|---|
| The week's items | `bookings`, `group_sessions`, `availability_exceptions` | via `get_calendar_items`, passed in `self.item` by the controller |

## 4. Actions

| Action | Event handler | Server function | Result |
|---|---|---|---|
| Render the week | the controller's render | None | The seven day columns render with bookings at their start times (spec-calendar acceptance) |
| Open a booking | the event card's click | None | `bookings.BookingViewerForm` opens |
| Open an empty slot | the empty cell's click | None | `bookings.BookingEditorForm` opens with provider, date and time filled in |

## 5. Empty State

| Part | Content |
|---|---|
| Icon | None — the week always shows its days and time rows; empty cells stay empty |
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

Same as the calendar; Staff see only their own bookings.

## 7. Text

| Phrase key | English text |
|---|---|
| (none) | The view's texts come from the calendar controller and its templates |

## 8. Help Videos

| Process | Help video |
|---|---|
| None | The calendar controller's video covers the views |

## 9. User Flows

| Flow | Part this form plays |
|---|---|
| UF-11 Admin daily operations | The week at a glance; the default view on screens 600 px wide or more |
