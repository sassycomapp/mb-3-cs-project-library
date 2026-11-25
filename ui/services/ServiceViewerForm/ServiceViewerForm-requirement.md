---
title: "ServiceViewerForm-requirement"
status: draft
doc-id: "mb-b10c6bb5"
updated: "2026-11-25"
---
# ServiceViewerForm — Screen Requirement

## 1. Form

| Item | Value |
|---|---|
| Module path | `services.ServiceViewerForm` |
| Purpose | The service's record: everything the editor holds, read-only, with the edit and video actions at hand (ticket 21, F-10) |
| Screen type and width | Editor, viewer, form — 900 px, sectioned panels |
| Layout | `layouts.AdminLayout` |
| Route | None — opened from `services.ServicesForm` |

## 2. Elements

| Element | Theme component | Why this component | Role | `wrap_on` | Tooltip phrase key |
|---|---|---|---|---|---|
| Page title | `m3.Heading` (`hdg_service_viewer`) | The service's name, `headline-large` | `page-title` | — | — |
| Header action row | Anvil `FlowPanel` (`flow_actions`) | Edit and view-public actions | — | mobile | — |
| Edit button | `m3.Button` (`btn_edit_service`, outlined) | Opens `services.ServiceEditorForm` | `secondary-button` | — | `users.row.edit` |
| View public page link | `m3.Link` (`lnk_public_page`) | Opens the service's public page `/services/{slug}` | `tertiary-button` | — | `services.view_public` |
| Basics panel | `m3.Card` (`card_basics`, outlined) | Name, category, description, pricing model, price, duration, hourly rate, display currency | `surface-card` | mobile | — |
| Delivery panel | `m3.Card` (`card_delivery`, outlined) | Providers, meeting types, travel buffer, group, required resources | `surface-card` | mobile | — |
| Payment panel | `m3.Card` (`card_payment`, outlined) | Payment option, deposit, payment terms | `surface-card` | mobile | — |
| Intake panel | `m3.Card` (`card_intake`, outlined) | The intake form's name | `surface-card` | mobile | — |
| Media panel | `m3.Card` (`card_media`, outlined) | The images and the video (through `components.VideoDisplayComponent`) | `surface-card` | mobile | — |
| Video display | `components.VideoDisplayComponent` | The service's video, hidden when no address is set (custom-component-requirements-matrix host contexts) | — | mobile | — |
| Active badge | `m3.Chip` (`chip_active`, type `input`, removable=False) | Active or inactive, as an outlined pill with the word | — | — | `services.active` |
| Help icon | `components.HelpTooltipComponent` (help entry `services.viewer`) | The feature's tooltip and help video | — | — | `help.services.viewer.tooltip` |

## 3. Data

| Element | Table | Column |
|---|---|---|
| All panels | `services` | the service's columns (as the editor's) |
| Video | `services` | `video_url`, `video_type` |

## 4. Actions

| Action | Event handler | Server function | Result |
|---|---|---|---|
| Load the service | `form_show` → `load_service` | `catalogue_service` read (`server_services/catalogue_service`) | The panels fill |
| Edit | `btn_edit_service_click` | None | `services.ServiceEditorForm` opens |
| View the public page | `lnk_public_page_click` | None | The public ServiceDetailPage opens |

## 5. Empty State

| Part | Content |
|---|---|
| Icon | Service icon (`mi:spa`) |
| Heading | `services.empty.title` |
| Body text | `services.empty.body` — shown when the service record is not found (defensive) |
| Primary action | `services.add` |

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
| `services.view_public` | View the public page |
| `services.active` | Active |
| `services.inactive` | Inactive |
| `services.empty.title` | Service not found |
| `services.empty.body` | This service may have been removed. |

## 8. Help Videos

| Process | Help video |
|---|---|
| Reading a service's record | `master_template` help entry `services.viewer` — played in the app language |

## 9. User Flows

| Flow | Part this form plays |
|---|---|
| UF-05–UF-08 Bookings | The service's settings at a glance before editing |
| UF-09 Ordering delivered work | The `unit` service's record |
