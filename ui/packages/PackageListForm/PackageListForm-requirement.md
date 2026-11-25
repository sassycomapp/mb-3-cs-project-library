---
title: "PackageListForm-requirement"
status: draft
doc-id: "mb-b2c85ca8"
updated: "2026-11-25"
---
# PackageListForm — Screen Requirement

## 1. Form

| Item | Value |
|---|---|
| Module path | `packages.PackageListForm` |
| Purpose | The package definitions and the customers' purchased packages (ticket 27, F-13; UF-10) |
| Screen type and width | List — 1200 px, full width: header, filters, data grid |
| Layout | `layouts.AdminLayout` |
| Route | None — opened from the admin navigation (Sales & Operations → Packages) |

## 2. Elements

| Element | Theme component | Why this component | Role | `wrap_on` | Tooltip phrase key |
|---|---|---|---|---|---|
| Page title | `m3.Heading` (`hdg_packages`) | "Packages", `headline-large` | `page-title` | — | `packages.title` |
| Add Package button | `m3.Button` (`btn_add_package`, filled) | The primary action; opens PackageEditorForm in create mode | `primary-button` | — | `packages.add` |
| View selector | `m3.ButtonGroup` (`bgrp_packages_view`, connected, single: Definitions/Customers) | Definitions (the packages for sale) and Customers (the purchased packages with their balances) | — | mobile | `packages.view.definitions`, `packages.view.customers` |
| Packages grid | Anvil `DataGrid` (`dg_packages`) | Name, service, sessions, price, expiry, active | — | mobile | — |
| Package row | Anvil `DataRowPanel` (`drp_package_*`) | One package definition per row | — | — | — |
| Customer packages grid | Anvil `DataGrid` (`dg_customer_packages`) | Customer, package, sessions remaining, expires, status | — | mobile | — |
| Customer row | Anvil `DataRowPanel` (`drp_customer_package_*`) | One purchased package per row | — | — | — | — | — |
| Help icon | `components.HelpTooltipComponent` (help entry `packages.list`) | The feature's tooltip and help video | — | — | `help.packages.list.tooltip` |

## 3. Data

| Element | Table | Column |
|---|---|---|
| Definitions | `packages` | `name`, `service_id`, `session_count`, `price`, `expiry_days`, `is_active` |
| Customers' packages | `customer_packages` | `contact_id`, `package_id`, `sessions_total`, `sessions_remaining`, `expires_at`, `status` |

## 4. Actions

| Action | Event handler | Server function | Result |
|---|---|---|---|
| Load the lists | `form_show` → `load_packages` | `package_service` list reads (`server_packages/package_service`) | The grids arrive |
| Switch the view | `bgrp_packages_view_change` | None | The chosen grid shows |
| Add a package | `btn_add_package_click` | None | `packages.PackageEditorForm` opens in create mode |
| Open a purchased package | the customers' row click | None | `crm.ContactViewerForm` opens for the customer |

## 5. Empty State

| Part | Content |
|---|---|
| Icon | Bundle icon (`mi:card_giftcard`) |
| Heading | `packages.empty.title` (DESIGN.md: "No packages yet") |
| Body text | `packages.empty.body` (DESIGN.md: "Packages let customers buy several sessions up front.") |
| Primary action | `packages.add` (+ Create Package) |

## 6. Access

| Role | Scope |
|---|---|
| Owner | all |
| Manager | all |
| Admin | all |
| Staff | none |
| Customer | none |
| SignedOut | none |

Per the access matrix (`packages.PackageListForm`: Owner, Manager, Admin).

## 7. Text

| Phrase key | English text |
|---|---|
| `packages.title` | Packages |
| `packages.add` | + Create Package |
| `packages.view.definitions` | Packages |
| `packages.view.customers` | Customers' packages |
| `packages.sessions_remaining` | {count} of {total} left |
| `packages.status.active` | Active |
| `packages.status.used` | Used up |
| `packages.status.expired` | Expired |
| `packages.status.cancelled` | Cancelled |
| `packages.empty.title` | No packages yet |
| `packages.empty.body` | Packages let customers buy several sessions up front. |

## 8. Help Videos

| Process | Help video |
|---|---|
| Working with packages | `master_template` help entry `packages.list` — played in the app language |

## 9. User Flows

| Flow | Part this form plays |
|---|---|
| UF-10 Buying and using a session package | The definitions the customer buys from; the balances shown here |
| UF-05 Services appointment booking | The package session drawn when a booking uses one |
