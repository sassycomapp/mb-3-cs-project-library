---
title: "BookingViewerForm-requirement"
status: draft
doc-id: "mb-3e8a74d3"
updated: "2026-11-25"
---
# BookingViewerForm — Screen Requirement

## 1. Form

| Item | Value |
|---|---|
| Module path | `bookings.BookingViewerForm` |
| Purpose | The booking's record with the status-appropriate actions: edit, reschedule, mark completed, mark no-show, cancel, view invoice (ticket 24, F-11; DESIGN.md Booking Status table) |
| Screen type and width | Editor, viewer, form — 900 px, sectioned panels |
| Layout | `layouts.AdminLayout` |
| Route | None — opened from the calendar and `bookings.BookingListForm` |

## 2. Elements

| Element | Theme component | Why this component | Role | `wrap_on` | Tooltip phrase key |
|---|---|---|---|---|---|
| Page title | `m3.Heading` (`hdg_booking_viewer`) | The booking number, `headline-large` | `page-title` | — | — |
| Status badge | `m3.Chip` (`chip_status`, type `input`, removable=False) | The status per the lifecycle | — | — | — |
| Details panel | `m3.Card` (`card_details`, outlined) | Customer, service, provider, resource, meeting type, address, times, places, payment status, notes | `surface-card` | mobile | — |
| Action row | Anvil `FlowPanel` (`flow_actions`) | The status-appropriate buttons per DESIGN.md's status table | — | mobile | — |
| Edit button | `m3.Button` (`btn_edit_booking`, outlined) | Pending only | `secondary-button` | — | `invoices.row.edit` |
| Reschedule button | `m3.Button` (`btn_reschedule`, outlined) | Pending, Confirmed; reschedule without cancellation (F-11) | `secondary-button` | — | `bookings.reschedule` |
| Mark Completed button | `m3.Button` (`btn_mark_completed`, filled) | Confirmed; triggers the invoice (or balance invoice) and the review request (UF-11) | `primary-button` | — | `bookings.mark_completed` |
| Mark No-Show button | `m3.Button` (`btn_mark_no_show`, outlined) | Confirmed; closes the booking and records the no-show on the contact (UF-11) | `secondary-button` | — | `bookings.mark_no_show` |
| Cancel button | `m3.Button` (`btn_cancel_booking`, outlined) | Pending, Confirmed; frees the slot and any resource, returns a package session where the window allows | `secondary-button` | — | `bookings.row.cancel` |
| View Invoice link | `m3.Link` (`lnk_view_invoice`) | Confirmed, Completed, No-show | `tertiary-button` | — | `bookings.view_invoice` |
| Help icon | `components.HelpTooltipComponent` (help entry `bookings.viewer`) | The feature's tooltip and help video | — | — | `help.bookings.viewer.tooltip` |

## 3. Data

| Element | Table | Column |
|---|---|---|
| Details | `bookings` | all the booking's columns |
| Payment status | `bookings` | `payment_status` (`unpaid` / `deposit_paid` / `paid` / `pay_on_day` / `invoiced` / `package` / `refunded`) |

## 4. Actions

| Action | Event handler | Server function | Result |
|---|---|---|---|
| Load the booking | `form_show` → `load_booking` | `booking_service` read (`server_bookings/booking_service`) | The panels fill; the actions follow the status table |
| Reschedule | `btn_reschedule_click` | `booking_service` reschedule | The booking moves; the customer is emailed with a calendar invite |
| Mark completed | `btn_mark_completed_click` | `booking_service` complete | The invoice is generated (or the deposit balance invoiced) and the review request is sent (UF-11) |
| Mark no-show | `btn_mark_no_show_click` | `booking_service` no_show | The booking closes; the no-show is recorded on the contact's timeline |
| Cancel | `btn_cancel_booking_click` | `booking_service` cancel | The slot and any resource are freed; a package session is returned where the window allows |
| View the invoice | `lnk_view_invoice_click` | None | `invoices.InvoiceViewerForm` opens |

## 5. Empty State

| Part | Content |
|---|---|
| Icon | Calendar icon (`mi:event`) |
| Heading | `bookings.empty.title` |
| Body text | `bookings.empty.body` — the not-found state |
| Primary action | None — back to the list |

## 6. Access

| Role | Scope |
|---|---|
| Owner | all |
| Manager | all |
| Admin | all |
| Staff | all |
| Customer | none |
| SignedOut | none |

Staff own only.

## 7. Text

| Phrase key | English text |
|---|---|
| `bookings.reschedule` | Reschedule |
| `bookings.mark_completed` | Mark Completed |
| `bookings.mark_no_show` | Mark No-Show |
| `bookings.view_invoice` | View invoice |
| `bookings.payment.unpaid` | Unpaid |
| `bookings.payment.deposit_paid` | Deposit paid |
| `bookings.payment.paid` | Paid |
| `bookings.payment.pay_on_day` | Pay on the day |
| `bookings.payment.invoiced` | Invoiced |
| `bookings.payment.package` | Package session |
| `bookings.payment.refunded` | Refunded |

## 8. Help Videos

| Process | Help video |
|---|---|
| Working a booking through its life | `master_template` help entry `bookings.viewer` — played in the app language |

## 9. User Flows

| Flow | Part this form plays |
|---|---|
| UF-11 Admin daily operations | Processing a booking: confirm, reschedule or cancel; completing an appointment; no-show |
| UF-27 Reviews | Completing triggers the review request |
| UF-38 Error recovery | The late-payment path's reinstated booking |
