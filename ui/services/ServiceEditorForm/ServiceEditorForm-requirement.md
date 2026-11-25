---
title: "ServiceEditorForm-requirement"
status: draft
doc-id: "mb-86199005"
updated: "2026-11-25"
---
# ServiceEditorForm — Screen Requirement

## 1. Form

| Item | Value |
|---|---|
| Module path | `services.ServiceEditorForm` |
| Purpose | Create or edit a service: pricing model, price, duration, hourly rate, display currency, providers, meeting types, travel buffer, group flag, required resources, payment option, deposit, payment terms, intake schema, images, video, active flag (ticket 21, F-10) |
| Screen type and width | Editor, viewer, form — 900 px, sectioned panels |
| Layout | `layouts.AdminLayout` |
| Route | None — opened from `services.ServicesForm` |

## 2. Elements

| Element | Theme component | Why this component | Role | `wrap_on` | Tooltip phrase key |
|---|---|---|---|---|---|
| Page title | `m3.Heading` (`hdg_service_editor`) | "New service" / the service's name, `headline-large` | `page-title` | — | `services.new` |
| Basics panel | `m3.Card` (`card_basics`, outlined) | Name, slug, description, category | `surface-card` | mobile | — |
| Name field | `m3.TextBox` (`txt_name`, outlined) | The service's name | `outlined-input` | — | `services.name` |
| Slug field | `m3.TextBox` (`txt_slug`, outlined) | Unique; the public address `/services/{slug}` | `outlined-input` | — | `services.slug` |
| Description field | `m3.TextArea` (`txt_description`, outlined) | The public description | `outlined-input` | — | `services.description` |
| Category selector | `m3.DropdownMenu` (`dd_category`) | The service's category | `outlined-select` | — | `services.category` |
| Pricing panel | `m3.Card` (`card_pricing`, outlined) | Pricing model and money | `surface-card` | mobile | — |
| Pricing model selector | `m3.ButtonGroup` (`bgrp_pricing`, connected, single) | `duration` (bookable time) / `unit` (delivered work) — the choice drives the rest of the form | — | mobile | `services.pricing_model` |
| Price field | `m3.TextBox` (`txt_price`, type `number`, outlined) | The price in whole cents of the system currency (schema Rules) | `outlined-input` | — | `services.price` |
| Duration field | `m3.TextBox` (`txt_duration`, type `number`, outlined) | Minutes; shown for `duration` services | `outlined-input` | — | `services.duration` |
| Hourly rate field | `m3.TextBox` (`txt_hourly_rate`, type `number`, outlined) | The default rate for time entries (F-14) | `outlined-input` | — | `services.hourly_rate` |
| Display currency selector | `m3.DropdownMenu` (`dd_display_currency`, `allow_none=True`) | Optional per-service display currency, display-only (F-16) | `outlined-select` | — | `currency.display.select` |
| Delivery panel | `m3.Card` (`card_delivery`, outlined) | Providers, meeting types, travel, group, resources | `surface-card` | mobile | — |
| Providers selector | `m3.DropdownMenu` (`dd_providers`) | The providers who deliver the service (`services.providers`, links → users) | `outlined-select` | — | `services.providers` |
| Meeting types checkboxes | `m3.Checkbox` set (`cb_mt_at_business`, `cb_mt_at_customer`, `cb_mt_video`, `cb_mt_phone`) | The meeting types the service supports (`services.meeting_types`) | — | mobile | `services.meeting.at_business`, `services.meeting.at_customer`, `services.meeting.video`, `services.meeting.phone` |
| Travel buffer field | `m3.TextBox` (`txt_travel_buffer`, type `number`, outlined) | Minutes before and after a booking at the customer's location | `outlined-input` | — | `services.travel_buffer` |
| Group switch | `m3.Switch` (`sw_group`) | Booked by places in a group session (`services.is_group`) | — | — | `services.group` |
| Required resources selector | `m3.DropdownMenu` (`dd_resources`) | The rooms or equipment a booking needs one of (`services.required_resources`) | `outlined-select` | — | `services.required_resources` |
| Payment panel | `m3.Card` (`card_payment`, outlined) | Payment option and terms | `surface-card` | mobile | — |
| Payment option selector | `m3.DropdownMenu` (`dd_payment_option`) | `full` / `deposit` / `on_the_day` / `invoice` (`services.payment_option`) | `outlined-select` | — | `services.payment_option` |
| Deposit field | `m3.TextBox` (`txt_deposit`, type `number`, outlined) | For `deposit` | `outlined-input` | — | `services.deposit` |
| Payment terms field | `m3.TextBox` (`txt_payment_terms`, type `number`, outlined) | For `invoice`; empty = the business default | `outlined-input` | — | `services.payment_terms` |
| Intake panel | `m3.Card` (`card_intake`, outlined) | The intake form | `surface-card` | mobile | — |
| Intake schema selector | `m3.DropdownMenu` (`dd_intake_schema`, `allow_none=True`) | The intake form (`services.intake_schema_id`); empty = none | `outlined-select` | — | `services.intake` |
| Media panel | `m3.Card` (`card_media`, outlined) | Images and video | `surface-card` | mobile | — |
| Images | `m3.FileLoader` (`fu_images`, `multiple=True`) | The service's images (`services.images`) | — | — | `services.images` |
| Video address field | `m3.TextBox` (`txt_video_url`) | Empty hides the video; type detected on save | `outlined-input` | — | `services.video` |
| Active switch | `m3.Switch` (`sw_active`) | Only active services are public (`services.is_active`) | — | — | `services.active` |
| AI helper trigger | `components.AIHelperComponent` (trigger) | The service description generator assist drafts the description (F-26) | — | — | `ai.open` |
| Save button | `m3.Button` (`btn_save_service`, filled) | Saves the service | `primary-button` | — | `services.save` |
| Cancel button | `m3.Button` (`btn_cancel_service`, text) | Returns without saving | `tertiary-button` | — | `editmode.bar.cancel` |
| Help icon | `components.HelpTooltipComponent` (help entry `services.editor`) | The feature's tooltip and help video | — | — | `help.services.editor.tooltip` |

## 3. Data

| Element | Table | Column |
|---|---|---|
| All fields | `services` | `service_id`, `name`, `slug`, `description`, `category`, `pricing_model`, `price`, `duration_minutes`, `hourly_rate`, `display_currency`, `providers`, `meeting_types`, `travel_buffer_minutes`, `is_group`, `required_resources`, `payment_option`, `deposit_amount`, `payment_terms_days`, `intake_schema_id`, `images`, `video_url`, `video_type`, `is_active` |

## 4. Actions

| Action | Event handler | Server function | Result |
|---|---|---|---|
| Load the service | `form_show` → `load_service` | `catalogue_service` read (`server_services/catalogue_service`) | The fields fill; the pricing model shows or hides the `duration`-only fields |
| Save | `btn_save_service_click` | `catalogue_service` save | The service is saved; the slug is checked unique; the video type is detected on save; money fields are whole cents of the system currency |
| AI description | the AI trigger | `request(assist="service_description", ...)` (`server_ai/ai_request_layer`) | The description draft appears for acceptance |
| Cancel | `btn_cancel_service_click` | None | The list opens without saving |

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
| Admin | none |
| Staff | none |
| Customer | none |
| SignedOut | none |

Per the access matrix; Owner/Manager edit (ticket 21).

## 7. Text

| Phrase key | English text |
|---|---|
| `services.new` | New service |
| `services.name` | Name |
| `services.slug` | Web address (slug) |
| `services.description` | Description |
| `services.category` | Category |
| `services.pricing_model` | Pricing |
| `services.price` | Price |
| `services.duration` | Duration (minutes) |
| `services.hourly_rate` | Hourly rate |
| `services.providers` | Providers |
| `services.meeting.at_business` | At your business |
| `services.meeting.at_customer` | At the customer's location |
| `services.meeting.video` | Video call |
| `services.meeting.phone` | Phone call |
| `services.travel_buffer` | Travel buffer (minutes each side) |
| `services.group` | Booked as a group session |
| `services.required_resources` | Needs a room or equipment |
| `services.payment_option` | Payment |
| `services.deposit` | Deposit amount |
| `services.payment_terms` | Payment terms (days) |
| `services.intake` | Intake form |
| `services.images` | Images |
| `services.video` | Video address |
| `services.active` | Active |
| `services.save` | Save |

## 8. Help Videos

| Process | Help video |
|---|---|
| Adding a service | `master_template` help entry `services.editor` — played in the app language |
| Choosing the pricing model | `master_template` help entry `services.pricing` |

## 9. User Flows

| Flow | Part this form plays |
|---|---|
| UF-05–UF-08 Bookings | The service's bookable settings the flow uses |
| UF-09 Ordering delivered work | The `unit` pricing model |
| UF-10 Packages | The service a package bundles |
| UF-13 Time tracking | The service's hourly rate |
| UF-20 Lead capture | The service pages' public address (`/services/{slug}`) |
