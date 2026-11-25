---
title: "BookingListForm-requirement"
status: draft
doc-id: "mb-5a68fb59"
updated: "2026-11-25"
---
# BookingListForm — Screen Requirement

## 1. Form

| Item | Value |
|---|---|
| Module path | `bookings.BookingListForm` |
| Purpose | The admin booking list: search and filters, the booking table per DESIGN.md's columns, and the two primary actions (ticket 24, F-11) |
| Screen type and width | List — 1200 px, full width: header, filters, data grid |
| Layout | `layouts.AdminLayout` |
| Route | None — opened from the admin navigation |

## 2. Elements

| Element | Theme component | Why this component | Role | `wrap_on` | Tooltip phrase key |
|---|---|---|---|---|---|
| Page title | `m3.Heading` (`hdg_bookings`) | "Bookings", `headline-large` | `page-title` | — | `bookings.title` |
| Add Booking button | `m3.Button` (`btn_add_booking`, filled) | "+ Add Booking" (DESIGN.md BookingListForm primary actions) | `primary-button` | — | `bookings.add` |
| Calendar View button | `m3.Button` (`btn_calendar_view`, outlined) | "Calendar View" opens BookingCalendarForm (DESIGN.md BookingListForm) | `secondary-button` | — | `bookings.calendar_view` |
| Search field | `m3.TextBox` (`txt_search`) | Search TextBox (DESIGN.md BookingListForm) | `outlined-input` | — | `bookings.search` |
| Status filter | `m3.DropdownMenu` (`dd_status_filter`) | Status DropdownMenu (DESIGN.md BookingListForm) | `outlined-select` | — | `bookings.filter.status` |
| Date range filter | `m3.DropdownMenu` (`dd_date_range`) | Date Range DropdownMenu (DESIGN.md BookingListForm) | `outlined-select` | — | `bookings.filter.date_range` |
| Booking grid | Anvil `DataGrid` (`dg_bookings`) | The columns per DESIGN.md: Booking #, Customer, Service, Date, Time, Status, Actions | — | mobile | — |
| Booking row | Anvil `DataRowPanel` (`drp_booking_*`) | One booking per row | — | — | — |
| Status badge | `m3.Chip` (`chip_booking_status_*`, type `input`, removable=False) | Status badge per the status lifecycle | — | — | — |
| Row actions | `m3.IconButton` (`ibtn_view_*`, `ibtn_edit_*`, `ibtn_cancel_*`) | View, Edit, Cancel icon buttons; Edit only for Pending; bookings are cancelled, never deleted (DESIGN.md BookingListForm) | `icon-button` | — | `invoices.row.view`, `invoices.row.edit`, `bookings.row.cancel` |
| Help icon | `components.HelpTooltipComponent` (help entry `bookings.list`) | The feature's tooltip and help video | — | — | `help.bookings.list.tooltip` |

## 3. Data

| Element | Table | Column |
|---|---|---|
| Grid | `bookings` | `booking_number`, `contact_id`, `service_id`, `start_datetime`, `end_datetime`, `status`, `total_price`, `payment_status` |

## 4. Actions

| Action | Event handler | Server function | Result |
|---|---|---|---|
| Load the list | `form_show` → `load_bookings` | `booking_service` list read (`server_bookings/booking_service`) | The grid arrives (Staff see only their own bookings — enforced on the server) |
| Search / filter | `txt_search_change`, `dd_status_filter_change`, `dd_date_range_change` | the same read | The grid reloads |
| Open a booking | `ibtn_view_*_click` | None | `bookings.BookingViewerForm` opens |
| Edit a pending booking | `ibtn_edit_*_click` | None | `bookings.BookingEditorForm` opens |
| Cancel a booking | `ibtn_cancel_*_click` | `booking_service` cancel | The booking is cancelled; the slot and any resource are freed; a package session is returned where the window allows |
| Add a booking | `btn_add_booking_click` | None | `bookings.BookingEditorForm` opens in create mode |
| Open the calendar | `btn_calendar_view_click` | None | `bookings.BookingCalendarForm` opens |

## 5. Empty State

| Part | Content |
|---|---|
| Icon | Calendar icon (`mi:event`) |
| Heading | `bookings.empty.title` |
| Body text | `bookings.empty.body` |
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

Staff own only (access matrix note; enforced on the server).

## 7. Text

| Phrase key | English text |
|---|---|
| `bookings.title` | Bookings |
| `bookings.add` | + Add Booking |
| `bookings.calendar_view` | Calendar View |
| `bookings.search` | Search bookings |
| `bookings.filter.status` | Status |
| `bookings.filter.date_range` | Date range |
| `bookings.row.cancel` | Cancel booking |
| `bookings.status.pending` | Pending |
| `bookings.status.confirmed` | Confirmed |
| `bookings.status.completed` | Completed |
| `bookings.status.no_show` | No-show |
| `bookings.status.cancelled` | Cancelled |
| `bookings.empty.title` | No bookings yet |
| `bookings.empty.body` | Bookings appear here as customers book your services. |

## 8. Help Videos

| Process | Help video |
|---|---|
| Managing bookings | `master_template` help entry `bookings.list` — played in the app language |

## 9. User Flows

| Flow | Part this form plays |
|---|---|
| UF-11 Admin daily operations | Processing a booking: confirm, reschedule, cancel |
| UF-05–UF-08 Bookings | The admin half of the bookings feature |
