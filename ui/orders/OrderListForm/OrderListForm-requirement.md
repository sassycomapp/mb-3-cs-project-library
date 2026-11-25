---
title: "OrderListForm-requirement"
status: draft
doc-id: "mb-06a82d7a"
updated: "2026-11-25"
---
# OrderListForm — Screen Requirement

## 1. Form

| Item | Value |
|---|---|
| Module path | `orders.OrderListForm` |
| Purpose | The admin order list with its statuses (ticket 26, F-12; UF-12) |
| Screen type and width | List — 1200 px, full width: header, filters, data grid |
| Layout | `layouts.AdminLayout` |
| Route | None — opened from the admin navigation (Sales & Operations → Orders) |

## 2. Elements

| Element | Theme component | Why this component | Role | `wrap_on` | Tooltip phrase key |
|---|---|---|---|---|---|
| Page title | `m3.Heading` (`hdg_orders`) | "Orders", `headline-large` | `page-title` | — | `orders.title` |
| Status filter | `m3.DropdownMenu` (`dd_order_status`) | All, Ordered, In progress, Delivered, Cancelled | `outlined-select` | — | `orders.filter.status` |
| Search field | `m3.SearchBar` (`srch_orders`) | Search by order number or customer | — | — | `orders.search` |
| Orders grid | Anvil `DataGrid` (`dg_orders`) | Order number, customer, service, status, total, ordered at | — | mobile | — |
| Order row | Anvil `DataRowPanel` (`drp_order_*`) | One order per row | — | — | — |
| Status badge | `m3.Chip` (`chip_order_status_*`, type `input`, removable=False) | The order status as an outlined pill with the word | — | — | — | — | — |
| Help icon | `components.HelpTooltipComponent` (help entry `orders.list`) | The feature's tooltip and help video | — | — | `help.orders.list.tooltip` |

## 3. Data

| Element | Table | Column |
|---|---|---|
| Grid | `orders` | `order_number`, `contact_id`, `service_id`, `status`, `total_price`, `ordered_at` |

## 4. Actions

| Action | Event handler | Server function | Result |
|---|---|---|---|
| Load the list | `form_show` → `load_orders` | `order_service` list read (`server_orders/order_service`) | The grid arrives (Staff own only) |
| Filter / search | `dd_order_status_change`, `srch_orders_search` | the same read | The grid reloads |
| Open an order | the row click | None | `orders.OrderViewerForm` opens |

## 5. Empty State

| Part | Content |
|---|---|
| Icon | Package icon (`mi:inventory_2`) |
| Heading | `orders.empty.title` (DESIGN.md: "No orders yet") |
| Body text | `orders.empty.body` (DESIGN.md: "Orders for delivered work appear here when customers order a fixed-price service." — no action, per DESIGN.md) |
| Primary action | None — the empty state explains how orders arrive (DESIGN.md) |

## 6. Access

| Role | Scope |
|---|---|
| Owner | all |
| Manager | all |
| Admin | all |
| Staff | all |
| Customer | none |
| SignedOut | none |

Staff own only (access matrix note).

## 7. Text

| Phrase key | English text |
|---|---|
| `orders.title` | Orders |
| `orders.search` | Search orders |
| `orders.filter.status` | Status |
| `orders.status.ordered` | Ordered |
| `orders.status.in_progress` | In progress |
| `orders.status.delivered` | Delivered |
| `orders.status.cancelled` | Cancelled |
| `orders.empty.title` | No orders yet |
| `orders.empty.body` | Orders for delivered work appear here when customers order a fixed-price service. |

## 8. Help Videos

| Process | Help video |
|---|---|
| Managing orders | `master_template` help entry `orders.list` — played in the app language |

## 9. User Flows

| Flow | Part this form plays |
|---|---|
| UF-12 Managing an order | The list the orders are worked from |
| UF-09 Ordering delivered work | The orders created by the flow appear here |
