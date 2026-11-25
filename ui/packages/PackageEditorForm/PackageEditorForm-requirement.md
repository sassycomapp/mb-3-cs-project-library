---
title: "PackageEditorForm-requirement"
status: draft
doc-id: "mb-baefaddf"
updated: "2026-11-25"
---
# PackageEditorForm — Screen Requirement

## 1. Form

| Item | Value |
|---|---|
| Module path | `packages.PackageEditorForm` |
| Purpose | Create or edit a package: the service it bundles, session count, price, expiry (ticket 27, F-13) |
| Screen type and width | Editor, viewer, form — 900 px, sectioned panels |
| Layout | `layouts.AdminLayout` |
| Route | None — opened from `packages.PackageListForm` |

## 2. Elements

| Element | Theme component | Why this component | Role | `wrap_on` | Tooltip phrase key |
|---|---|---|---|---|---|
| Page title | `m3.Heading` (`hdg_package_editor`) | "New package" / the package's name, `headline-large` | `page-title` | — | `packages.new` |
| Name field | `m3.TextBox` (`txt_package_name`, outlined) | The package's name | `outlined-input` | — | `packages.name` |
| Description field | `m3.TextArea` (`txt_package_description`, outlined) | What the package includes | `outlined-input` | — | `packages.description` |
| Service selector | `m3.DropdownMenu` (`dd_package_service`) | The service the sessions are of | `outlined-select` | — | `services.name` |
| Session count field | `m3.TextBox` (`txt_session_count`, type `number`, outlined) | How many sessions the package holds | `outlined-input` | — | `packages.sessions` |
| Price field | `m3.TextBox` (`txt_package_price`, type `number`, outlined) | Whole cents of the system currency | `outlined-input` | — | `services.price` |
| Expiry field | `m3.TextBox` (`txt_expiry_days`, type `number`, outlined) | Days from purchase | `outlined-input` | — | `packages.expiry` |
| Active switch | `m3.Switch` (`sw_package_active`) | Only active packages are offered | — | — | `services.active` |
| Save button | `m3.Button` (`btn_save_package`, filled) | Saves the package | `primary-button` | — | `services.session.save` |
| Cancel button | `m3.Button` (`btn_cancel_package`, text) | Returns without saving | `tertiary-button` | — | `editmode.bar.cancel` |
| Help icon | `components.HelpTooltipComponent` (help entry `packages.editor`) | The feature's tooltip and help video | — | — | `help.packages.editor.tooltip` |

## 3. Data

| Element | Table | Column |
|---|---|---|
| Fields | `packages` | `name`, `description`, `service_id`, `session_count`, `price`, `expiry_days`, `is_active` |

## 4. Actions

| Action | Event handler | Server function | Result |
|---|---|---|---|
| Load the package | `form_show` → `load_package` | `package_service` read (`server_packages/package_service`) | The fields fill |
| Save | `btn_save_package_click` | `package_service` save | The package is saved |
| Cancel | `btn_cancel_package_click` | None | The list opens without saving |

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

Per the access matrix.

## 7. Text

| Phrase key | English text |
|---|---|
| `packages.new` | New package |
| `packages.name` | Name |
| `packages.description` | Description |
| `packages.sessions` | Sessions |
| `packages.expiry` | Expires after (days) |
| `packages.editor.no_service` | Choose the service the sessions are of. |

## 8. Help Videos

| Process | Help video |
|---|---|
| Creating a package | `master_template` help entry `packages.editor` — played in the app language |

## 9. User Flows

| Flow | Part this form plays |
|---|---|
| UF-10 Buying and using a session package | The package the customer buys and draws from |
