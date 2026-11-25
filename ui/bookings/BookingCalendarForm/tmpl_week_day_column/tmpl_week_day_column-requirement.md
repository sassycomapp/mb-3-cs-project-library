---
title: "tmpl_week_day_column-requirement"
status: draft
doc-id: "mb-ef657b0d"
updated: "2026-11-25"
---
# tmpl_week_day_column — Screen Requirement

## 1. Form

| Item | Value |
|---|---|
| Module path | `bookings.BookingCalendarForm.tmpl_week_day_column` |
| Purpose | One day column in the calendar's week view (ticket 24, F-11; spec-calendar-feature section 4.2) |
| Screen type and width | Card in a grid — 320 px, embedded |
| Layout | Item template of the week view's `rp_week_days` |
| Route | None — an item template |

## 2. Elements

Per `spec-calendar-feature.md` section 4.2:

| Element | Theme component | Why this component | Role | `wrap_on` | Tooltip phrase key |
|---|---|---|---|---|---|
| Column container | Anvil `ColumnPanel` (`col_day_column`) | The day's one column | — | never | — |
| Day name | `m3.Heading` (`hdg_day_name`) | Day name and date; today marked with the primary token | `headline-small` | — | — |
| Day slots | Anvil `ColumnPanel` (`col_day_slots`) | One cell per time row; a blocked period shows the blocked token; a cell with no booking is empty | — | — | — |
| Events | Anvil `RepeatingPanel` (`rp_day_events`) with item template `bookings.BookingCalendarForm.tmpl_week_event` | The day's bookings, each placed in the row of its start time | — | — | — |

## 3. Data

| Element | Table | Column |
|---|---|---|
| The column's items | the period's items | passed in `self.item` by the repeating panel |

## 4. Actions

| Action | Event handler | Server function | Result |
|---|---|---|---|
| Open a booking | the event card's click | None | `bookings.BookingViewerForm` opens |
| Open an empty slot | the empty cell's click | None | `bookings.BookingEditorForm` opens with provider, date and time filled in |

## 5. Empty State

| Part | Content |
|---|---|
| Icon | None — an empty day column shows its name and empty cells |
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
| (none) | The column's texts come from the item's data and the templates |

## 8. Help Videos

| Process | Help video |
|---|---|
| None | The calendar controller's video covers the views |

## 9. User Flows

| Flow | Part this form plays |
|---|---|
| UF-11 Admin daily operations | The week view's day columns |
