---
title: "BookingFlowForm-requirement"
status: draft
doc-id: "mb-827fa3ed"
updated: "2026-11-25"
---
# BookingFlowForm — Screen Requirement

## 1. Form

| Item | Value |
|---|---|
| Module path | `bookings.BookingFlowForm` |
| Purpose | The customer booking flow, steps 1–7, three server round trips maximum ending in one `confirm_booking` (ticket 24, F-11; DESIGN.md 7-Step Booking Flow; adr-form-architecture-and-state) |
| Screen type and width | Editor, viewer, form — 900 px, sectioned panels (the flow's step panels) |
| Layout | `layouts.PublicLayout` (entered from `/services`; the flow is reachable by every role, SignedOut included) |
| Route | Opened from the public service pages; the source (blog, landing page, social, direct) is carried through (UF-20 funnel) |

## 2. Elements

| Element | Theme component | Why this component | Role | `wrap_on` | Tooltip phrase key |
|---|---|---|---|---|---|
| Step container | Anvil `ColumnPanel` (`col_flow`) | The flow's one step at a time, in `self.item` per the guided-flow rule (spec-ui-standards section 8.7) | — | mobile | — |
| Progress indicator | `m3.LinearProgressIndicator` (`lpi_steps`, determinate) | The theme's progress indicator for the guided flow | — | — | — |
| Step 1: service cards | `m3.InteractiveCard` (`icard_service_*`, outlined) per service | Service cards (image, name, price, duration) with search/filter (DESIGN.md step 1); the selected card highlights with the primary-container treatment | `surface-card` | mobile (320 px card in a grid) | — |
| Service search | `m3.SearchBar` (`srch_services`) | The step's search/filter | — | — | `booking.flow.search` |
| Step 2: provider cards | `m3.InteractiveCard` (`icard_provider_*`, outlined) per provider | Provider cards (avatar, name, specialty) from the RT1 response; client-side from cache | `surface-card` | mobile | — |
| Provider avatar | `m3.Avatar` (`av_provider_*`) | The provider's photo (`users.photo`) or initials | — | — | — |
| Step 3: date and time | `m3.DatePicker` (`dp_booking_date`) + the slot grid (`m3.InteractiveCard` slots) | The theme's DatePicker (month calendar) + time slot grid; available slots from RT2; selected slot highlights; unavailable slots shown disabled (DESIGN.md step 3) | `outlined-date` | mobile | `booking.flow.date`, `booking.flow.time` |
| Step 4: meeting type cards | `m3.InteractiveCard` (`icard_meeting_*`) | At the business, at my location (with address entry), video call, phone call — only the types the service offers; from RT1 cache, no server call | `surface-card` | mobile | `services.meeting.at_business`, `services.meeting.at_customer`, `services.meeting.video`, `services.meeting.phone` |
| Address fields | `m3.TextBox` (`txt_address_line_1`, `txt_city`, `txt_postal_code`, `txt_country`) | The address for `at_customer` (`bookings.customer_address`) | `outlined-input` | mobile | `profile.address_1`, `profile.city`, `profile.postal_code`, `profile.country` |
| Step 5: intake form | dynamic fields from `booking_metadata_schemas` (`m3.TextBox`, `m3.DropdownMenu`, `m3.TextArea`) | Text inputs, dropdowns, textareas as the schema defines; from RT1 cache | `outlined-input` | mobile | set by the schema's labels |
| Notes field | `m3.TextArea` (`txt_special_requests`) | Appointment notes or special requests (UF-05 step 8) | `outlined-input` | — | `booking.flow.notes` |
| Customer details fields | `m3.TextBox` (`txt_customer_name`, type `email` `txt_customer_email`, type `tel` `txt_customer_phone`) | Name, email, phone; signed-in customers are pre-filled (UF-05 step 6) | `outlined-input` | mobile | `booking.flow.name`, `booking.flow.email`, `booking.flow.phone` |
| Step 6: order summary card | `m3.Card` (`card_summary`, elevated) | Service, provider, date, time, meeting type, address, amount | `elevated-card` | mobile | — |
| Payment option display | `m3.Text` (`text_payment_option`) | The service's payment option; button text follows it ("Confirm & Pay" / "Confirm Booking") | `body-medium` | — | `booking.flow.pay_full`, `booking.flow.confirm` |
| Stripe fields | the gateway's Elements (given the app language as `locale`) | The card payment surface (SC-10: card data never touches the app) | — | mobile | — |
| PayPal surface | `components.PayPalButtonComponent` | The PayPal option's buttons (ticket 15) | — | mobile | — |
| Loading indicator | `m3.LoadingIndicator` (`load_flow`, hidden, contained) | The theme's loading indicator during RT3 and the step transitions (DESIGN.md booking flow rules) | — | — | — |
| Step 7: confirmation card | `m3.Card` (`card_confirmation`, elevated) | Success card with a check icon, booking ID, details summary | `elevated-card` | mobile | — |
| View Booking button | `m3.Button` (`btn_view_booking`, filled) | Opens the booking for the customer | `primary-button` | — | `booking.flow.view_booking` |
| Book Another button | `m3.Button` (`btn_book_another`, outlined) | Starts the flow again | `secondary-button` | — | `booking.flow.book_another` |
| Back button | `m3.IconButton` (`ibtn_flow_back`, icon `mi:arrow_back`) | Back navigation preserves state — no re-fetch when cached in `booking_flow_state.cached_context` (DESIGN.md) | `icon-button` | — | `booking.flow.back` |
| Terms acceptance box | `m3.Checkbox` (`cb_terms`, never ticked in advance) + two `m3.Link` (`lnk_terms`, `lnk_privacy`) | "I accept the Terms and Conditions and have read the Privacy Policy", with both documents linked beside it; the confirm button stays disabled until the box is ticked (DESIGN.md Terms Acceptance Box) | — | — | `consent.terms.checkbox`, `consent.terms.link_terms`, `consent.terms.link_privacy` |
| Retry message | `m3.Text` (`text_flow_retry`, hidden) | "Connection timed out. Please try again." with retry (spec-test-specification 2.7); `booking_flow_state` is preserved | `body-small` | — | `booking.flow.timeout` |

## 3. Data

| Element | Table | Column |
|---|---|---|
| RT1 context | `services`, `users`, `booking_metadata_schemas` | `services` (public subset), `users.photo`/`specialisations`, the intake schema's `field_definitions` |
| RT2 slots | `availability_rules`, `availability_exceptions`, `bookings` | the availability calculation (a pure module beside `availability_service`) |
| The booking | `bookings` | `contact_id`, `service_id`, `provider_id`, `resource_id`, `group_session_id`, `places`, `booking_type`, `meeting_type`, `customer_address`, `call_link`, `status`, `start_datetime`, `end_datetime`, `total_price`, `payment_option`, `deposit_amount`, `special_requests`, `metadata`, `source` |
| Flow state | `shared/globals.py` | `booking_flow_state` (`booking_flow_state.cached_context`) |

## 4. Actions

| Action | Event handler | Server function | Result |
|---|---|---|---|
| Load the flow | `form_show` → `load_context` | `get_booking_context(service_id)` (RT1) | Services, providers, meeting types and the intake schema arrive in one call |
| Choose provider and date | `dp_booking_date_change` | `get_available_slots(provider_id, date_range)` (RT2) | Only available slots are offered (provider and required resource both free) |
| Confirm | `btn_confirm_click` | `confirm_booking(booking_data)` (RT3) | The server re-checks the slot at the moment of confirmation (UF-05 clash prevention); the atomic re-check of slot, resource, places and price inside `@tables.in_transaction` (SR-16); the contact is created or updated; payment is taken or the booking is held |
| Payment failure | the gateway's failure path | the webhook handlers | The customer sees the failure with retry; the booking is held as payment pending for 15 minutes with an emailed link (UF-38); the slot is released when the hold expires |
| Accept terms | `cb_terms_change` | None | The confirm button enables; acceptance writes the `consents` rows with the contact (DESIGN.md Terms Acceptance Box) |

## 5. Empty State

| Part | Content |
|---|---|
| Icon | Calendar icon (`mi:event`) |
| Heading | `booking.flow.no_slots.title` |
| Body text | `booking.flow.no_slots.body` — when no slots are offered for the chosen provider and date; the customer can select a different date or provider (spec-test-specification: Provider Unavailable) |
| Primary action | `booking.flow.choose_other_date` |

## 6. Access

| Role | Scope |
|---|---|
| Owner | all |
| Manager | all |
| Admin | all |
| Staff | all |
| Customer | all |
| SignedOut | all |

The flow is public (UF-05); `confirm_booking`'s access matrix row: Staff none, everyone else all.

## 7. Text

| Phrase key | English text |
|---|---|
| `booking.flow.search` | Find a service |
| `booking.flow.date` | Choose a date |
| `booking.flow.time` | Choose a time |
| `booking.flow.notes` | Anything we should know? |
| `booking.flow.name` | Your name |
| `booking.flow.email` | Email |
| `booking.flow.phone` | Phone |
| `booking.flow.pay_full` | Confirm & Pay |
| `booking.flow.confirm` | Confirm Booking |
| `booking.flow.view_booking` | View Booking |
| `booking.flow.book_another` | Book Another |
| `booking.flow.back` | Back |
| `booking.flow.timeout` | Connection timed out. Please try again. |
| `booking.flow.no_slots.title` | No available time slots |
| `booking.flow.no_slots.body` | Try another day, or another provider. |
| `booking.flow.choose_other_date` | Choose another date |
| `consent.terms.checkbox` | I accept the Terms and Conditions and have read the Privacy Policy |
| `consent.terms.link_terms` | Terms and Conditions |
| `consent.terms.link_privacy` | Privacy Policy |

## 8. Help Videos

| Process | Help video |
|---|---|
| Booking an appointment | `master_template` help entry `booking.flow` — played in the app language |

## 9. User Flows

| Flow | Part this form plays |
|---|---|
| UF-05 Services appointment booking | Steps 1–7 with the three round trips |
| UF-06 Booking at the customer's location | The at-my-location meeting type and the address |
| UF-07 Group session booking | The group service's session and places |
| UF-08 Booking that needs a room or equipment | Only slots where provider and resource are both free |
| UF-10 Buying and using a session package | The use-a-package-session option at step 9 |
| UF-20 Lead capture | The source is carried into the booking |
| UF-38 Error recovery | The 15-minute hold, the retry with the same card, the timeout message |
