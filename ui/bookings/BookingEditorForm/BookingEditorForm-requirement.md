---
title: "BookingEditorForm-requirement"
status: draft
doc-id: "mb-3db71daa"
updated: "2026-11-25"
---
# BookingEditorForm — Screen Requirement

## 1. Form

| Item | Value |
|---|---|
| Module path | `bookings.BookingEditorForm` |
| Purpose | Create or edit a booking from the admin side: customer, service, provider, resource, times, meeting type, notes (ticket 24, F-11; UF-11) |
| Screen type and width | Editor, viewer, form — 900 px, sectioned panels |
| Layout | `layouts.AdminLayout` |
| Route | None — opened from the calendar's empty-slot click and `bookings.BookingListForm` |

## 2. Elements

| Element | Theme component | Why this component | Role | `wrap_on` | Tooltip phrase key |
|---|---|---|---|---|---|
| Page title | `m3.Heading` (`hdg_booking_editor`) | "New booking" / the booking number, `headline-large` | `page-title` | — | `bookings.new` |
| Customer selector | `m3.DropdownMenu` (`dd_customer`) | The contact (created or matched) | `outlined-select` | — | `invoices.customer` |
| Service selector | `m3.DropdownMenu` (`dd_service`) | The service | `outlined-select` | — | `services.name` |
| Provider selector | `m3.DropdownMenu` (`dd_provider`) | The provider | `outlined-select` | — | `services.session.provider` |
| Resource selector | `m3.DropdownMenu` (`dd_resource`, `allow_none=True`) | The room or equipment reserved | `outlined-select` | — | `services.session.resource` |
| Date field | `m3.DatePicker` (`dp_date`, outlined) | The booking's date | `outlined-date` | — | `booking.flow.date` |
| Start time field | `m3.TimePicker` (`tp_start`, outlined) | The start | `outlined-input` | — | `booking.flow.time` |
| End time field | `m3.TimePicker` (`tp_end`, outlined) | The end | `outlined-input` | — | `bookings.editor.end` |
| Meeting type selector | `m3.DropdownMenu` (`dd_meeting_type`) | The meeting types the service offers | `outlined-select` | — | `services.meeting.at_business` |
| Address fields | `m3.TextBox` (`txt_address_line_1`, `txt_city`, `txt_postal_code`) | For `at_customer` (`bookings.customer_address`) | `outlined-input` | mobile | `profile.address_1`, `profile.city`, `profile.postal_code` |
| Price display | `m3.Text` (`text_price`) | The total price, from the service | `body-medium` | — | `services.price` |
| Special requests field | `m3.TextArea` (`txt_special_requests`) | The customer's notes | `outlined-input` | — | `booking.flow.notes` |
| Terms/notes note | `m3.Text` (`text_editor_note`) | Reminder that confirming or rescheduling emails the customer with a calendar invite (UF-11) | `body-small` | — | `bookings.editor.email_note` |
| Save button | `m3.Button` (`btn_save_booking`, filled) | Saves the booking | `primary-button` | — | `services.session.save` |
| Cancel button | `m3.Button` (`btn_cancel_booking`, text) | Returns without saving | `tertiary-button` | — | `editmode.bar.cancel` |
| Help icon | `components.HelpTooltipComponent` (help entry `bookings.editor`) | The feature's tooltip and help video | — | — | `help.bookings.editor.tooltip` |

## 3. Data

| Element | Table | Column |
|---|---|---|
| Fields | `bookings` | `contact_id`, `service_id`, `provider_id`, `resource_id`, `meeting_type`, `customer_address`, `start_datetime`, `end_datetime`, `total_price`, `special_requests`, `status` |

## 4. Actions

| Action | Event handler | Server function | Result |
|---|---|---|---|
| Load the booking | `form_show` → `load_booking` | `booking_service` read (`server_bookings/booking_service`) | The fields fill; an empty-slot open arrives with provider, date and time filled (spec-calendar section 6) |
| Save | `btn_save_booking_click` | `booking_service` save | The booking is saved or rescheduled; confirming or rescheduling sends the customer an updated confirmation with a calendar invite (UF-11) |
| Cancel | `btn_cancel_booking_click` | None | The list opens without saving |

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
| Admin | all |
| Staff | all |
| Customer | none |
| SignedOut | none |

Staff own only; writable fields per `writable_fields.bookings` (Staff: special_requests).

## 7. Text

| Phrase key | English text |
|---|---|
| `bookings.new` | New booking |
| `bookings.editor.end` | End |
| `bookings.editor.email_note` | The customer gets an updated confirmation with a calendar invite. |
| `bookings.editor.slot_taken` | That slot was just taken. Pick another time. |

## 8. Help Videos

| Process | Help video |
|---|---|
| Creating or editing a booking | `master_template` help entry `bookings.editor` — played in the app language |

## 9. User Flows

| Flow | Part this form plays |
|---|---|
| UF-11 Admin daily operations | The empty-slot click starts a new booking with provider, date and time filled in; processing a booking |
