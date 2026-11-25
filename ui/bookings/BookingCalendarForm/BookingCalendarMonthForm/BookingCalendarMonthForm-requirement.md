---
title: "BookingCalendarMonthForm-requirement"
status: draft
doc-id: "mb-01b4401a"
updated: "2026-11-25"
---
# BookingCalendarMonthForm — Screen Requirement

## 1. Form

| Item | Value |
|---|---|
| Module path | `bookings.BookingCalendarForm.BookingCalendarMonthForm` |
| Purpose | The month view of the booking calendar (ticket 24, F-11; spec-calendar-feature section 4.1) |
| Screen type and width | List — 1200 px (the calendar's own width) |
| Layout | Hosted by `bookings.BookingCalendarForm`'s content panel |
| Route | None — a view form of the calendar |

## 2. Elements

Per `spec-calendar-feature.md` section 4.1:

| Element | Theme component | Why this component | Role | `wrap_on` | Tooltip phrase key |
|---|---|---|---|---|---|
| Month container | Anvil `ColumnPanel` (`col_month`) | The spec's root container | — | mobile | — |
| Month grid | Anvil `DataGrid` (`dg_month`) | Columns Mon–Sun, equal widths; rows_per_page 6, show_page_controls False (spec-calendar section 4.1) | — | mobile | — |
| Week row | Anvil `DataRowPanel` (`drp_week_*`) | One per week (up to six), each holding seven `bookings.BookingCalendarForm.tmpl_month_day_cell` | — | — | — |

Responsive: the grid keeps seven equal columns at every width (spec-calendar section 4.1). Below 600 px each cell shows its day number and a booking count instead of booking cards. Clicking a day opens the day view for that date.

## 3. Data

| Element | Table | Column |
|---|---|---|
| The month's items | `bookings`, `group_sessions`, `availability_exceptions` | via `get_calendar_items`, passed in `self.item` by the controller |

## 4. Actions

| Action | Event handler | Server function | Result |
|---|---|---|---|
| Render the month | the controller's render | None | The six week rows render from the period's items |
| Open a day | the cell's click | None | The day view for that date opens (spec-calendar section 6) |

## 5. Empty State

| Part | Content |
|---|---|
| Icon | None — the month grid always shows its days; a day with nothing shows an empty cell |
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
| `calendar.day.mon` | Mon |
| `calendar.day.tue` | Tue |
| `calendar.day.wed` | Wed |
| `calendar.day.thu` | Thu |
| `calendar.day.fri` | Fri |
| `calendar.day.sat` | Sat |
| `calendar.day.sun` | Sun |
| `calendar.month.more` | +{count} more |

## 8. Help Videos

| Process | Help video |
|---|---|
| None | The calendar controller's video covers the views |

## 9. User Flows

| Flow | Part this form plays |
|---|---|
| UF-11 Admin daily operations | The month's shape of the period |
