---
title: "EmailSettingsForm-requirement"
status: draft
doc-id: "mb-0da9c6e9"
updated: "2026-11-25"
---
# EmailSettingsForm — Screen Requirement

## 1. Form

| Item | Value |
|---|---|
| Module path | `settings.EmailSettingsForm` |
| Purpose | Settings → Email: the client's email addresses, desks, signatures, auto-replies and the default sender (ticket 29, F-18; UF-39; DESIGN.md EmailSettingsForm) |
| Screen type and width | Editor, viewer, form — 900 px, sectioned panels |
| Layout | `layouts.AdminLayout` (opened from the Email tab) |
| Route | None — opened from `settings.SettingsForm` |

## 2. Elements

| Element | Theme component | Why this component | Role | `wrap_on` | Tooltip phrase key |
|---|---|---|---|---|---|
| Address list | Anvil `ColumnPanel` (`col_addresses_list`) | Address, kind badge, display name, members, status (DESIGN.md EmailSettingsForm) | — | mobile | — |
| Address row | `m3.ListItem` (`li_email_address_*`, clickable) | One address per row with its kind badge and status | — | — | — |
| Kind badge | `m3.Chip` (`chip_kind_*`, type `input`, removable=False) | Personal or desk, an outlined pill with the word (DESIGN.md EmailSettingsForm) | — | — | `email.kind.personal`, `email.kind.desk` |
| Add Address button | `m3.Button` (`btn_add_address`, filled) | Opens the address form | `primary-button` | — | `email.add` |
| Address form panel | `m3.Card` (`card_address_form`, outlined) | The part before the `@` with the domain shown after it, kind, display name, members, signature, auto-reply (DESIGN.md EmailSettingsForm) | `surface-card` | mobile | — |
| Address field | `m3.TextBox` (`txt_new_address`, outlined) | The part before the `@`; the domain is shown after it | `outlined-input` | — | `email.address` |
| Domain display | `m3.Text` (`text_domain_suffix`) | `@{domain}` from `email_config.domain` | `body-medium` | — | — |
| Kind selector | `m3.ButtonGroup` (`bgrp_kind`, connected, single) | Personal or desk | — | mobile | `email.kind.personal`, `email.kind.desk` |
| Display name field | `m3.TextBox` (`txt_display_name`, outlined) | The sender name shown to recipients | `outlined-input` | — | `email.display_name` |
| Members selector | `m3.DropdownMenu` (`dd_members`) | The team members who read and send from the address (multi-select of team members) | `outlined-select` | — | `email.members` |
| Signature field | `m3.TextArea` (`txt_signature`) | The signature (rich text) added to every email from the address | `outlined-input` | — | `email.signature` |
| Auto-reply switch | `m3.Switch` (`sw_auto_reply`) | Auto-reply text with start and end dates (DESIGN.md EmailSettingsForm) | — | — | `email.auto_reply` |
| Auto-reply text field | `m3.TextArea` (`txt_auto_reply_text`) | The auto-reply's text | `outlined-input` | mobile | `email.auto_reply.text` |
| Auto-reply dates | `m3.DatePicker` (`dp_auto_reply_from`), `m3.DatePicker` (`dp_auto_reply_until`) | Start and end | `outlined-date` | mobile | `email.auto_reply.from`, `email.auto_reply.until` |
| Save address button | `m3.Button` (`btn_save_address`, filled) | Saves the address | `primary-button` | — | `email.save` |
| Default sender selector | `m3.DropdownMenu` (`dd_default_sender`) | For confirmations, reminders, invoices and receipts (`email_config.default_from_address_id`) | `outlined-select` | — | `email.default_sender` |
| Help icon | `components.HelpTooltipComponent` (help entry `settings.email`) | The feature's tooltip and help video | — | — | `help.settings.email.tooltip` |

The client enters no email credential: Amazon SES is set up by App 5 (DESIGN.md EmailSettingsForm Architecture).

## 3. Data

| Element | Table | Column |
|---|---|---|
| Addresses | `email_addresses` | `address`, `kind`, `display_name`, `member_ids`, `signature`, `auto_reply_text`, `auto_reply_from`, `auto_reply_until`, `is_active` |
| Default sender | `email_config` | `default_from_address_id` |
| Domain | `email_config` | `domain` |

## 4. Actions

| Action | Event handler | Server function | Result |
|---|---|---|---|
| Load the tab | `form_show` → `load_email` | `list_addresses` (`server_inbox/address_service`) + `email_config` read | The addresses and the default sender arrive |
| Save an address | `btn_save_address_click` | `address_service` save (`server_inbox/address_service`) | The address is saved; the personal addresses of invited team members are created at invitation (UF-39 step 2) |
| Set the default sender | `dd_default_sender_change` | `address_service` save | `email_config.default_from_address_id` is stored |
| Deactivate an address | the row's deactivate action | `address_service` save | `is_active = False`; the address stops receiving into the Team Inbox |

## 5. Empty State

| Part | Content |
|---|---|
| Icon | Mail icon (`mi:alternate_email`) |
| Heading | `email.empty.title` |
| Body text | `email.empty.body` |
| Primary action | `email.add` (+ Add Address) |

## 6. Access

| Role | Scope |
|---|---|
| Owner | all |
| Manager | all |
| Admin | none |
| Staff | none |
| Customer | none |
| SignedOut | none |

Per `access-matrix.yaml` `settings_tabs.Email`.

## 7. Text

| Phrase key | English text |
|---|---|
| `email.add` | + Add Address |
| `email.address` | Address |
| `email.kind.personal` | Personal |
| `email.kind.desk` | Desk |
| `email.display_name` | Display name |
| `email.members` | Members |
| `email.signature` | Signature |
| `email.auto_reply` | Auto-reply |
| `email.auto_reply.text` | Auto-reply message |
| `email.auto_reply.from` | From |
| `email.auto_reply.until` | Until |
| `email.save` | Save address |
| `email.default_sender` | Default sender for confirmations, reminders, invoices and receipts |
| `email.empty.title` | No addresses yet |
| `email.empty.body` | Create your first address on your own domain. |

## 8. Help Videos

| Process | Help video |
|---|---|
| Adding addresses and desks | `master_template` help entry `settings.email` — played in the app language |

## 9. User Flows

| Flow | Part this form plays |
|---|---|
| UF-39 Email addresses and desks | The whole flow: add, kind, members, signature, auto-reply, deactivate |
| UF-03 Staff invitation | The personal address created for each team member |
| UF-02 Owner onboarding | The wizard's email step creates the Owner's personal address and the default sender here |
