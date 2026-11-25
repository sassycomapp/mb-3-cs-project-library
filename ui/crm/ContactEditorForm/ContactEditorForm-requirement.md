---
title: "ContactEditorForm-requirement"
status: draft
doc-id: "mb-9962ac12"
updated: "2026-11-25"
---
# ContactEditorForm — Screen Requirement

## 1. Form

| Item | Value |
|---|---|
| Module path | `crm.ContactEditorForm` |
| Purpose | Create or edit a contact: identity, status, tags, notes and preferences (ticket 20, F-17) |
| Screen type and width | Editor, viewer, form — 900 px, sectioned panels |
| Layout | `layouts.AdminLayout` |
| Route | None — opened from `crm.ContactListForm` |

## 2. Elements

| Element | Theme component | Why this component | Role | `wrap_on` | Tooltip phrase key |
|---|---|---|---|---|---|
| Page title | `m3.Heading` (`hdg_contact_editor`) | "New contact" / the contact's name, `headline-large` | `page-title` | — | `crm.contact.new` |
| Identity panel | `m3.Card` (`card_identity`, outlined) | The contact's identity fields | `surface-card` | mobile | — |
| First name field | `m3.TextBox` (`txt_first_name`, outlined) | The contact's first name | `outlined-input` | — | `crm.contact.first_name` |
| Last name field | `m3.TextBox` (`txt_last_name`, outlined) | The last name | `outlined-input` | — | `crm.contact.last_name` |
| Email field | `m3.TextBox` (`txt_email`, type `email`, outlined) | The matching key for automatic creation; may be empty for walk-ins (schema) | `outlined-input` | — | `crm.contact.email` |
| Phone field | `m3.TextBox` (`txt_phone`, type `tel`, outlined) | The phone | `outlined-input` | — | `crm.contact.phone` |
| Company selector | `m3.DropdownMenu` (`dd_company`, `allow_none=True`) | The optional company (F-35) | `outlined-select` | — | `crm.contact.company` |
| Address panel | `m3.Card` (`card_address`, outlined) | The address shown on invoices (`contacts.address`) | `surface-card` | mobile | — |
| Address fields | `m3.TextBox` (`txt_address_line_1`, `txt_address_line_2`, `txt_city`, `txt_region`, `txt_postal_code`, `txt_country`) | The address keys the schema's `address` simpleObject holds | `outlined-input` | — | `profile.address_1`, `profile.address_2`, `profile.city`, `crm.contact.region`, `profile.postal_code`, `profile.country` |
| CRM panel | `m3.Card` (`card_crm`, outlined) | Status, tags and notes | `surface-card` | mobile | — |
| Status selector | `m3.DropdownMenu` (`dd_status`) | `lead` / `customer` / `inactive` | `outlined-select` | — | `crm.contact.status` |
| Tags field | `m3.Chip` set (`chip_tag_*`, type `input`, removable=True) in an Anvil `FlowPanel` | The contact's tags as removable input chips; a tag field adds one | — | mobile | `crm.contact.tags` |
| Internal notes field | `m3.TextArea` (`txt_internal_notes`, outlined) | The internal notes; readable only by the roles the access matrix allows (the restricted projection, IN-04) | `outlined-input` | — | `crm.contact.notes` |
| Save button | `m3.Button` (`btn_save_contact`, filled) | Saves the contact | `primary-button` | — | `crm.contact.save` |
| Cancel button | `m3.Button` (`btn_cancel_contact`, text) | Returns without saving | `tertiary-button` | — | `editmode.bar.cancel` |
| Help icon | `components.HelpTooltipComponent` (help entry `crm.contact_editor`) | The feature's tooltip and help video | — | — | `help.crm.contact_editor.tooltip` |

## 3. Data

| Element | Table | Column |
|---|---|---|
| Identity and CRM fields | `contacts` | `first_name`, `last_name`, `email`, `phone`, `company_id`, `status`, `tags`, `internal_notes`, `preferences` |
| Address | `contacts` | `address` (simpleObject: `address_line_1`, `address_line_2`, `city`, `region`, `postal_code`, `country`) |

## 4. Actions

| Action | Event handler | Server function | Result |
|---|---|---|---|
| Load the contact | `form_show` → `load_contact` | `get_contact` (`server_customers/contact_service`) | The contact arrives with the restricted projection (internal_notes only for allowed roles) |
| Save | `btn_save_contact_click` | `contact_service` save (`server_customers/contact_service`) | The contact is saved; a new one gets its `C-{number}` id from `contact_counter` inside `@tables.in_transaction`; the timeline records the change |
| Cancel | `btn_cancel_contact_click` | None | The list opens without saving |

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

Admin may write only the listed writable fields (`writable_fields.contacts`: first_name, last_name, email, phone, status, tags).

## 7. Text

| Phrase key | English text |
|---|---|
| `crm.contact.new` | New contact |
| `crm.contact.first_name` | First name |
| `crm.contact.last_name` | Last name |
| `crm.contact.email` | Email |
| `crm.contact.phone` | Phone |
| `crm.contact.company` | Company |
| `crm.contact.region` | Region |
| `crm.contact.status` | Status |
| `crm.contact.tags` | Tags |
| `crm.contact.notes` | Internal notes |
| `crm.contact.save` | Save |
| `crm.contact.region.label` | Region |

## 8. Help Videos

| Process | Help video |
|---|---|
| Adding or editing a contact | `master_template` help entry `crm.contact_editor` — played in the app language |

## 9. User Flows

| Flow | Part this form plays |
|---|---|
| UF-21 Contact management | Add a contact; edit one |
| UF-19 Contact form submission | Not this form — the submission creates the contact on the server |
| UF-35 Team Inbox | Not this form — Add as lead runs on the server |
