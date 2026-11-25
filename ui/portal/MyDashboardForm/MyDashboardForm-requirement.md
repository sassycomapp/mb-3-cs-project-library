---
title: "MyDashboardForm-requirement"
status: draft
doc-id: "mb-cf6e800c"
updated: "2026-11-25"
---
# MyDashboardForm — Screen Requirement

## 1. Form

| Item | Value |
|---|---|
| Module path | `portal.MyDashboardForm` |
| Purpose | The customer's portal home: upcoming bookings, package balances, open quotes, invoices due (ticket 39, F-24; UF-29) |
| Screen type and width | Dashboard — 1200 px, full width: metric cards |
| Layout | `layouts.CustomerLayout` |
| Route | None — opened by the portal routing |

## 2. Elements

| Element | Theme component | Why this component | Role | `wrap_on` | Tooltip phrase key |
|---|---|---|---|---|---|
| Page title | `m3.Heading` (`hdg_my_dashboard`) | "My Dashboard", `headline-large` | `page-title` | — | `portal.dashboard.title` |
| Upcoming bookings card | `m3.Card` (`card_upcoming_bookings`, outlined) | The customer's upcoming bookings (UF-29 step 2) | `surface-card` | mobile | — |
| Booking row | `m3.ListItem` (`li_my_booking_*`, clickable) | One booking per row: service, date, time, status; the click opens My Bookings | — | — | — |
| Package balances card | `m3.Card` (`card_package_balances`, outlined) | The customer's packages and their balances | `surface-card` | mobile | — |
| Package row | `m3.ListItem` (`li_my_package_*`) | Package name, sessions remaining, expiry | — | — | — |
| Open quotes card | `m3.Card` (`card_open_quotes`, outlined) | The customer's open quotes | `surface-card` | mobile | — |
| Quote row | `m3.ListItem` (`li_my_quote_*`, clickable) | Quote number, total, valid until; the click opens My Quotes | — | — | — |
| Invoices due card | `m3.Card` (`card_invoices_due`, outlined) | The customer's invoices due | `surface-card` | mobile | — |
| Invoice row | `m3.ListItem` (`li_my_invoice_*`, clickable) | Invoice number, total, due date; the click opens My Invoices | — | — | — |
| Book again button | `m3.Button` (`btn_portal_book_again`, filled) | The portal's primary action: books again (UF-29 step 3) | `primary-button` | — | `portal.book_again` |
| Help icon | `components.HelpTooltipComponent` (help entry `portal.dashboard`) | The feature's tooltip and help video | — | — | `help.portal.dashboard.tooltip` |

## 3. Data

| Element | Table | Column |
|---|---|---|
| Bookings | `bookings` | the signed-in customer's rows via `get_my_bookings` |
| Packages | `customer_packages` | `sessions_remaining`, `expires_at`, `status` |
| Quotes | `quotes` | `quote_number`, `total_amount`, `valid_until`, `status` |
| Invoices | `invoice` | `invoice_number`, `total_amount`, `amount_paid`, `due_date`, `status` |

Every view shows only the signed-in customer's own records, derived from `anvil.users.get_user()` (ticket 39).

## 4. Actions

| Action | Event handler | Server function | Result |
|---|---|---|---|
| Load the dashboard | `form_show` → `load_my_dashboard` | the portal's own customer functions (ticket 39) | The four cards arrive, own records only |
| Book again | `btn_portal_book_again_click` | None | The booking flow opens |
| Open a card's item | the row clicks | None | The item's portal page opens |

## 5. Empty State

| Part | Content |
|---|---|
| Icon | Dashboard icon (`mi:dashboard`) |
| Heading | `portal.dashboard.empty.title` |
| Body text | `portal.dashboard.empty.body` |
| Primary action | `portal.book_again` (Book again) |

## 6. Access

| Role | Scope |
|---|---|
| Owner | all |
| Manager | all |
| Admin | all |
| Staff | none |
| Customer | all |
| SignedOut | none |

Per the access matrix (the team roles support the customer).

## 7. Text

| Phrase key | English text |
|---|---|
| `portal.dashboard.title` | My Dashboard |
| `portal.book_again` | Book again |
| `portal.dashboard.upcoming` | Upcoming bookings |
| `portal.dashboard.packages` | Package balances |
| `portal.dashboard.quotes` | Open quotes |
| `portal.dashboard.invoices` | Invoices due |
| `portal.dashboard.empty.title` | Nothing here yet |
| `portal.dashboard.empty.body` | Your bookings, packages, quotes and invoices appear here. |

## 8. Help Videos

| Process | Help video |
|---|---|
| Your dashboard | `master_template` help entry `portal.dashboard` — played in the app language |

## 9. User Flows

| Flow | Part this form plays |
|---|---|
| UF-29 Customer portal | The portal's home |
| UF-41 Customer accepts a quote | The open quotes card |
