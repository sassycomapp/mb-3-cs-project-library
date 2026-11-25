---
title: "LeadCaptureForm-requirement"
status: draft
doc-id: "mb-d5ee0800"
updated: "2026-11-25"
---
# LeadCaptureForm — Screen Requirement

## 1. Form

| Item | Value |
|---|---|
| Module path | `crm.LeadCaptureForm` |
| Purpose | The admin lead-capture form editor: the three kinds, trigger settings, form fields, offer text (ticket 31, F-19; UF-20) |
| Screen type and width | Editor, viewer, form — 900 px, sectioned panels |
| Layout | `layouts.AdminLayout` |
| Route | None — opened from the admin navigation (Customers & Marketing → Lead Capture) |

## 2. Elements

| Element | Theme component | Why this component | Role | `wrap_on` | Tooltip phrase key |
|---|---|---|---|---|---|
| Page title | `m3.Heading` (`hdg_lead_capture`) | "Lead capture", `headline-large` | `page-title` | — | `leads.title` |
| Name field | `m3.TextBox` (`txt_capture_name`, outlined) | The capture's name (`lead_captures.capture_name`) | `outlined-input` | — | `leads.name` |
| Kind selector | `m3.ButtonGroup` (`bgrp_capture_type`, connected, single) | Lead form, Event registration, or Video sales letter (`lead_captures.capture_type`) | — | mobile | `leads.kind.form`, `leads.kind.event`, `leads.kind.vsl` |
| Landing page selector | `m3.DropdownMenu` (`dd_capture_page`) | The landing page the capture belongs to (`leads.landing_page_id`) | `outlined-select` | — | `landing.title` |
| Offer text field | `m3.TextArea` (`txt_offer_text`) | What the visitor gets (`lead_captures.offer_text`) | `outlined-input` | — | `leads.offer` |
| Form fields editor | `m3.Card` (`card_form_fields`, outlined) with field rows | The form's fields: field, label, type, required (`lead_captures.form_fields`) | `surface-card` | mobile | — |
| Field row | Anvil `FlowPanel` (`flow_field_*`) | One field per row: label, type, required | — | mobile | — |
| Field label field | `m3.TextBox` (`txt_field_label_*`, outlined) | The field's label | `outlined-input` | — | `leads.field.label` |
| Field type selector | `m3.DropdownMenu` (`dd_field_type_*`) | Text, email, phone, dropdown, textarea | `outlined-select` | — | `leads.field.type` |
| Required switch | `m3.Switch` (`sw_field_required_*`) | Whether the field is required | — | — | `leads.field.required` |
| Add field button | `m3.Button` (`btn_add_field`, outlined) | Adds a field row | `secondary-button` | — | `leads.field.add` |
| Trigger settings panel | `m3.Card` (`card_trigger_settings`, outlined) | The capture's trigger settings (`lead_captures.trigger_settings`) | `surface-card` | mobile | — |
| Welcome sequence switch | `m3.Switch` (`sw_welcome_sequence`) | The contact is enrolled in the welcome sequence (UF-20 step 4; ticket 31's server side) | — | — | `leads.welcome` |
| Status selector | `m3.DropdownMenu` (`dd_capture_status`) | Draft, Active, Inactive (`lead_captures.status`) | `outlined-select` | — | `leads.status` |
| Save button | `m3.Button` (`btn_save_capture`, filled) | Saves the capture | `primary-button` | — | `services.session.save` |
| Cancel button | `m3.Button` (`btn_cancel_capture`, text) | Returns without saving | `tertiary-button` | — | `editmode.bar.cancel` |
| Analytics display | `m3.Text` (`text_capture_stats`) | Captures and conversions (`lead_captures.captures_count`, `conversions_count`) | `body-small` | — | `leads.stats` |
| Help icon | `components.HelpTooltipComponent` (help entry `crm.lead_capture`) | The feature's tooltip and help video | — | — | `help.crm.lead_capture.tooltip` |

## 3. Data

| Element | Table | Column |
|---|---|---|
| Capture | `lead_captures` | `capture_name`, `capture_type`, `trigger_settings`, `form_fields`, `offer_text`, `status`, `captures_count`, `conversions_count` |

## 4. Actions

| Action | Event handler | Server function | Result |
|---|---|---|---|
| Load the capture | `form_show` → `load_capture` | `lead_capture_service` read (`server_marketing/lead_capture_service`) | The fields fill |
| Save | `btn_save_capture_click` | `lead_capture_service` save | The capture is saved; an active capture's landing page shows its form |
| Add a field | `btn_add_field_click` | None | A field row is added |

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
| Staff | none |
| Customer | none |
| SignedOut | none |

Per the access matrix (`crm.LeadCaptureForm`: Owner, Manager, Admin).

## 7. Text

| Phrase key | English text |
|---|---|
| `leads.title` | Lead capture |
| `leads.name` | Name |
| `leads.kind.form` | Lead form |
| `leads.kind.event` | Event registration |
| `leads.kind.vsl` | Video sales letter |
| `leads.offer` | What they get |
| `leads.field.label` | Label |
| `leads.field.type` | Type |
| `leads.field.required` | Required |
| `leads.field.add` | + Add field |
| `leads.welcome` | Enrol new leads in the welcome sequence |
| `leads.status` | Status |
| `leads.stats` | {captures} captured, {conversions} became customers. |

## 8. Help Videos

| Process | Help video |
|---|---|
| Setting up a capture form | `master_template` help entry `crm.lead_capture` — played in the app language |

## 9. User Flows

| Flow | Part this form plays |
|---|---|
| UF-20 Lead capture via landing page | The capture's set-up half |
| UF-18 Landing page creation | The capture is hosted by the landing page |
