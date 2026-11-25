---
title: "BookingCalendarForm-requirement"
status: draft
doc-id: "mb-5924c3d7"
updated: "2026-11-25"
---
# BookingCalendarForm — Screen Requirement

## 1. Form

| Item | Value |
|---|---|
| Module path | `bookings.BookingCalendarForm` |
| Purpose | The booking calendar's controller: toolbar, view switching, date navigation, filters, data loading and the 60-second refresh, per `spec-calendar-feature.md` in full (ticket 24, F-11) |
| Screen type and width | List — 1200 px, full width: header, filters, data grid |
| Layout | `layouts.AdminLayout` |
| Route | None — opened from the admin navigation (Bookings → Calendar) and `bookings.BookingListForm`'s Calendar View |

## 2. Elements

Per `spec-calendar-feature.md` section 3.1 (the component tree is the specification's):

| Element | Theme component | Why this component | Role | `wrap_on` | Tooltip phrase key |
|---|---|---|---|---|---|
| Calendar container | Anvil `ColumnPanel` (`col_calendar`) | The spec's root container | — | mobile | — |
| Toolbar | Anvil `FlowPanel` (`flow_toolbar`) | The spec's toolbar row; wraps on a phone | — | mobile | — |
| Calendar title | `m3.Heading` (`hdg_calendar_title`) | "Calendar" | `headline-small` | — | `calendar.title` |
| Today button | `m3.Button` (`btn_today`, outlined) | The spec's Today control | `secondary-button` | — | `calendar.today` |
| Previous button | `m3.IconButton` (`btn_prev`, icon `mi:chevron_left`) | The spec's previous control | `icon-button` | — | `calendar.prev` |
| Next button | `m3.IconButton` (`btn_next`, icon `mi:chevron_right`) | The spec's next control | `icon-button` | — | `calendar.next` |
| Period title | `m3.Heading` (`hdg_period_title`) | e.g. "March 2026", "3–9 March 2026", "Tuesday 4 March 2026" | `headline-small` | — | — |
| Go-to-date field | `m3.DatePicker` (`dp_go_to_date`, outlined) | The spec's go-to-date control | `outlined-date` | — | `calendar.go_to_date` |
| View selector | `m3.ButtonGroup` (`bgrp_views`, connected, single, items Month/Week/Day) | One segmented button: the selected view's button is filled, the others outlined, roles set in code on view change (spec-calendar section 3.1) | — | mobile | `calendar.view.month`, `calendar.view.week`, `calendar.view.day` |
| Provider filter | `m3.DropdownMenu` (`dd_provider`) | "All providers" + each provider; hidden for Staff (spec-calendar section 2) | `outlined-select` | — | `calendar.filter.provider` |
| Resource filter | `m3.DropdownMenu` (`dd_resource`) | "All resources" + each room/equipment; hidden when the client has no resources | `outlined-select` | — | `calendar.filter.resource` |
| Status legend | Anvil `FlowPanel` (`flow_legend`) | Status legend: pending, confirmed, completed, no-show, cancelled, blocked — small outlined cards, not chips (spec-calendar section 9: chips are interactive, the legend is not) | — | mobile | `calendar.legend.pending`, `calendar.legend.confirmed`, `calendar.legend.completed`, `calendar.legend.no_show`, `calendar.legend.cancelled`, `calendar.legend.blocked` |
| Content panel | Anvil `ColumnPanel` (`col_content`) | The active view form is added here; switching view clears it and adds the new one — views are never kept loaded and hidden (spec-calendar section 3) | — | mobile | — |
| Refresh timer | Anvil `Timer` (`tmr_refresh`, 60 s) | A timer refreshes the current period every 60 seconds while the calendar is open; it stops when the user leaves (spec-calendar section 7.3) | — | — | — |
| Loading indicator | `m3.LoadingIndicator` (`load_calendar`, hidden) | Shown while a period loads | — | — | — |
| Help icon | `components.HelpTooltipComponent` (help entry `bookings.calendar`) | The feature's tooltip and help video | — | — | `help.bookings.calendar.tooltip` |

The default view is week on screens 600 px wide or more and day below; the last view used is remembered for the user's session (spec-calendar section 3.1).

## 3. Data

| Element | Table | Column |
|---|---|---|
| The period's items | `bookings`, `group_sessions`, `availability_exceptions` | via `get_calendar_items` (spec-calendar section 7.1): the bookings, sessions and blocked periods overlapping the visible period, never the whole table |
| Filters | `users` (`is_provider`), `resources` | the filter lists |

## 4. Actions

| Action | Event handler | Server function | Result |
|---|---|---|---|
| Load the period | `load_view(view_name)` | `get_calendar_items(start_utc, end_utc, provider_id, resource_id, show_cancelled)` (`server_bookings/calendar_service`) | One server call per navigation, returning only the visible period; for a Staff user the provider filter is ignored and their own provider record is used (spec-calendar section 7.1) |
| Switch view | `bgrp_views_change` | the same call | The new view form is added to the content panel |
| Today / prev / next / go to date | `btn_today_click`, `btn_prev_click`, `btn_next_click`, `dp_go_to_date_change` | the same call | The period moves and reloads |
| Change a filter | `dd_provider_change`, `dd_resource_change` | the same call | The calendar reloads with the filter (Owner, Manager, Admin) |
| Open a booking | the view's item click | None | `bookings.BookingViewerForm` opens (UF-11) |
| Open an empty slot | the view's empty-cell click | None | `bookings.BookingEditorForm` opens with the provider, date and time filled in (spec-calendar section 6) |
| Open a day (month view) | the cell click | None | The day view for that date opens |
| Refresh | `tmr_refresh_tick` | `get_calendar_items` | The current period reloads every 60 seconds |

Changes made in BookingViewerForm or BookingEditorForm refresh the calendar on return (spec-calendar section 6).

## 5. Empty State

| Part | Content |
|---|---|
| Icon | Calendar icon (`mi:event_busy`) |
| Heading | `calendar.empty.title` |
| Body text | `calendar.empty.body` |
| Primary action | `bookings.add` (+ Add Booking) |

## 6. Access

| Role | Scope |
|---|---|
| Owner | all |
| Manager | all |
| Admin | all |
| Staff | all |
| Customer | none |
| SignedOut | none |

Staff see only their own bookings, enforced on the server (spec-calendar section 2); a Staff user's browser never receives another provider's bookings.

## 7. Text

| Phrase key | English text |
|---|---|
| `calendar.title` | Calendar |
| `calendar.today` | Today |
| `calendar.prev` | Previous |
| `calendar.next` | Next |
| `calendar.go_to_date` | Go to date |
| `calendar.view.month` | Month |
| `calendar.view.week` | Week |
| `calendar.view.day` | Day |
| `calendar.filter.provider` | Provider |
| `calendar.filter.provider_all` | All providers |
| `calendar.filter.resource` | Room or equipment |
| `calendar.filter.resource_all` | All resources |
| `calendar.legend.pending` | Pending |
| `calendar.legend.confirmed` | Confirmed |
| `calendar.legend.completed` | Completed |
| `calendar.legend.no_show` | No-show |
| `calendar.legend.cancelled` | Cancelled |
| `calendar.legend.blocked` | Blocked |
| `calendar.empty.title` | Nothing booked |
| `calendar.empty.body` | This period has no bookings. |

## 8. Help Videos

| Process | Help video |
|---|---|
| Using the calendar | `master_template` help entry `bookings.calendar` — played in the app language |
| The three views and the filters | `master_template` help entry `calendar.views` |

## 9. User Flows

| Flow | Part this form plays |
|---|---|
| UF-11 Admin daily operations | The calendar: month, week or day view; Today/previous/next; filters; the 60-second refresh; the booking and empty-slot clicks |
| UF-05–UF-08 Bookings | The bookings and sessions shown on the calendar |
