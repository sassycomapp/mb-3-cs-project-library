---
title: "tmpl_month_day_cell-requirement"
status: draft
doc-id: "mb-2762af7e"
updated: "2026-11-25"
---
# tmpl_month_day_cell — Screen Requirement

## 1. Form

| Item | Value |
|---|---|
| Module path | `bookings.BookingCalendarForm.tmpl_month_day_cell` |
| Purpose | One day cell in the calendar's month view (ticket 24, F-11; spec-calendar-feature section 4.1) |
| Screen type and width | Card in a grid — 320 px, embedded |
| Layout | Item template of the month grid's `DataRowPanel` |
| Route | None — an item template |

## 2. Elements

Per `spec-calendar-feature.md` section 4.1:

| Element | Theme component | Why this component | Role | `wrap_on` | Tooltip phrase key |
|---|---|---|---|---|---|
| Cell container | Anvil `ColumnPanel` (`col_cell`) | The cell's one column; clickable to open the day view | — | mobile | — |
| Day number | `m3.Heading` (`hdg_day_number`) | The day number; today and days outside the month use the tokens in spec-calendar section 7.2 (today: Primary Container fill; outside the month: On Surface Variant text) | `headline-small` | — | — |
| Events row | Anvil `FlowPanel` (`flow_events`) | Up to three `card_event` items (status colour, start time, customer or session name) | — | mobile | — |
| Booking count (phone) | `m3.Text` (`text_day_count`) | Below 600 px the cell shows its day number and a booking count instead of booking cards | `body-small` | — | `calendar.month.count` |
| Blocked fill | role `calendar-blocked` | A blocked date or closure shows the blocked token across the cell (Surface Variant fill with Outline border, per spec-calendar section 7.2) | — | — | — |

## 3. Data

| Element | Table | Column |
|---|---|---|
| The cell's items | the period's items | passed in `self.item` by the week row |

## 4. Actions

| Action | Event handler | Server function | Result |
|---|---|---|---|
| Open the day view | the cell's click | None | The day view for that date opens (spec-calendar section 6) |

## 5. Empty State

| Part | Content |
|---|---|
| Icon | None — an empty day shows its number and an empty events row |
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

Same as the calendar.

## 7. Text

| Phrase key | English text |
|---|---|
| `calendar.month.count` | {count} bookings |

## 8. Help Videos

| Process | Help video |
|---|---|
| None | The calendar controller's video covers the views |

## 9. User Flows

| Flow | Part this form plays |
|---|---|
| UF-11 Admin daily operations | The month view's day cells |
