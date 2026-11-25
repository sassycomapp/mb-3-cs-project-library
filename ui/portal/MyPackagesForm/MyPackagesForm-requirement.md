---
title: "MyPackagesForm-requirement"
status: draft
doc-id: "mb-1fb0527a"
updated: "2026-11-25"
---
# MyPackagesForm — Screen Requirement

## 1. Form

| Item | Value |
|---|---|
| Module path | `portal.MyPackagesForm` |
| Purpose | The customer's packages: balance and expiry (ticket 39, F-24; UF-29 step 4) |
| Screen type and width | List — 1200 px, full width: header, filters, data grid |
| Layout | `layouts.CustomerLayout` |
| Route | None — opened by the portal routing |

## 2. Elements

| Element | Theme component | Why this component | Role | `wrap_on` | Tooltip phrase key |
|---|---|---|---|---|---|
| Page title | `m3.Heading` (`hdg_my_packages`) | "My Packages", `headline-large` | `page-title` | — | `portal.nav.packages` |
| Packages grid | Anvil `DataGrid` (`dg_my_packages`) | Package, service, sessions remaining, expires, status | — | mobile | — |
| Package row | Anvil `DataRowPanel` (`drp_my_package_*`) | One purchased package per row | — | — | — |
| Balance badge | `m3.Chip` (`chip_my_package_balance_*`, type `input`, removable=False) | The sessions remaining as an outlined pill | — | — | — |
| Book with package button | `m3.Button` (`btn_book_with_package`, filled) | Starts the booking flow for the package's service (the package session is used at step 9) | `primary-button` | — | `portal.packages.book` |
| Help icon | `components.HelpTooltipComponent` (help entry `portal.packages`) | The feature's tooltip and help video | — | — | `help.portal.packages.tooltip` |

## 3. Data

| Element | Table | Column |
|---|---|---|
| Packages | `customer_packages` | the signed-in customer's rows: `package_id`, `sessions_total`, `sessions_remaining`, `expires_at`, `status` |

## 4. Actions

| Action | Event handler | Server function | Result |
|---|---|---|---|
| Load the packages | `form_show` → `load_my_packages` | the portal's package read (own only) | The grid arrives |
| Book with a package | `btn_book_with_package_click` | None | The booking flow opens with the package's service |

## 5. Empty State

| Part | Content |
|---|---|
| Icon | Bundle icon (`mi:card_giftcard`) |
| Heading | `portal.packages.empty.title` |
| Body text | `portal.packages.empty.body` |
| Primary action | `public.nav.services` |

## 6. Access

| Role | Scope |
|---|---|
| Owner | all |
| Manager | all |
| Admin | all |
| Staff | none |
| Customer | all |
| SignedOut | none |

## 7. Text

| Phrase key | English text |
|---|---|
| `portal.packages.book` | Book with this package |
| `portal.packages.empty.title` | No packages yet |
| `portal.packages.empty.body` | Packages you buy appear here with the sessions you have left. |

## 8. Help Videos

| Process | Help video |
|---|---|
| Your packages | `master_template` help entry `portal.packages` — played in the app language |

## 9. User Flows

| Flow | Part this form plays |
|---|---|
| UF-10 Buying and using a session package | The balance and expiry (UF-10 step 2) |
| UF-29 Customer portal | My Packages |
