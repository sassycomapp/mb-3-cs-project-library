---
title: "ServicesForm-requirement"
status: draft
doc-id: "mb-d89db74e"
updated: "2026-11-25"
---
# ServicesForm — Screen Requirement

## 1. Form

| Item | Value |
|---|---|
| Module path | `services.ServicesForm` |
| Purpose | The admin service list: the catalogue of bookable and deliverable services, with categories and resources at hand (ticket 21, F-10) |
| Screen type and width | List — 1200 px, full width: header, filters, data grid |
| Layout | `layouts.AdminLayout` |
| Route | None — opened from the admin navigation |

## 2. Elements

| Element | Theme component | Why this component | Role | `wrap_on` | Tooltip phrase key |
|---|---|---|---|---|---|
| Page title | `m3.Heading` (`hdg_services`) | "Services", `headline-large` | `page-title` | — | `services.title` |
| Search field | `m3.SearchBar` (`srch_services`) | Search services by name or category | — | — | `services.search` |
| Category filter | `m3.DropdownMenu` (`dd_category`, `allow_none=True`) | Filter by category | `outlined-select` | — | `services.filter.category` |
| Add Service button | `m3.Button` (`btn_add_service`, filled) | The primary action; opens ServiceEditorForm in create mode | `primary-button` | — | `services.add` |
| Resources button | `m3.Button` (`btn_resources`, outlined) | Opens `services.ResourceListForm` (Services → Resources) | `secondary-button` | — | `services.resources` |
| Categories button | `m3.Button` (`btn_categories`, text) | Opens `services.ServiceCategoriesForm` | `tertiary-button` | — | `services.categories` |
| Service grid | Anvil `DataGrid` (`dg_services`) | Name, category, pricing model, price, duration, providers, active | — | mobile | — |
| Service row | Anvil `DataRowPanel` (`drp_service_*`) | One service per row | — | — | — | — | — |
| Help icon | `components.HelpTooltipComponent` (help entry `services.catalogue`) | The feature's tooltip and help video | — | — | `help.services.catalogue.tooltip` |

## 3. Data

| Element | Table | Column |
|---|---|---|
| Grid | `services` | `name`, `category`, `pricing_model`, `price`, `duration_minutes`, `providers`, `is_active` |

## 4. Actions

| Action | Event handler | Server function | Result |
|---|---|---|---|
| Load the list | `form_show` → `load_services` | `catalogue_service` list read (`server_services/catalogue_service`) | The grid arrives |
| Search / filter | `srch_services_search`, `dd_category_change` | the same read | The grid reloads |
| Open a service | the row click | None | `services.ServiceViewerForm` opens |
| Add a service | `btn_add_service_click` | None | `services.ServiceEditorForm` opens in create mode |

## 5. Empty State

| Part | Content |
|---|---|
| Icon | Service icon (`mi:spa`) |
| Heading | `services.empty.title` (DESIGN.md: "No services yet") |
| Body text | `services.empty.body` (DESIGN.md: "Add your first service so clients can book appointments with you.") |
| Primary action | `services.add` (+ Add Service) |

## 6. Access

| Role | Scope |
|---|---|
| Owner | all |
| Manager | all |
| Admin | none |
| Staff | none |
| Customer | none |
| SignedOut | none |

Per the access matrix; every team member views (ticket 21: ServicesForm scope all for team roles — the row notes the ticket's intent; the matrix row is authoritative and gives Owner/Manager only, which this requirement follows).

## 7. Text

| Phrase key | English text |
|---|---|
| `services.title` | Services |
| `services.search` | Search services |
| `services.filter.category` | Category |
| `services.add` | + Add Service |
| `services.resources` | Resources |
| `services.categories` | Categories |
| `services.pricing.duration` | Bookable time |
| `services.pricing.unit` | Delivered work |
| `services.empty.title` | No services yet |
| `services.empty.body` | Add your first service so clients can book appointments with you. |

## 8. Help Videos

| Process | Help video |
|---|---|
| Building your service catalogue | `master_template` help entry `services.catalogue` — played in the app language |

## 9. User Flows

| Flow | Part this form plays |
|---|---|
| UF-05–UF-08 Bookings | The catalogue the booking flow draws its services from |
| UF-09 Ordering delivered work | The `unit`-priced services the order flow uses |
| UF-07 Group sessions | The group services' set-up entry |
| UF-08 Rooms and equipment | The Resources entry |
