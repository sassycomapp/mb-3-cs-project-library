---
title: "DealBoardForm-requirement"
status: draft
doc-id: "mb-1242df0c"
updated: "2026-11-25"
---
# DealBoardForm — Screen Requirement

## 1. Form

| Item | Value |
|---|---|
| Module path | `sales.DealBoardForm` |
| Purpose | The sales pipeline: one column per stage, each with its count and total value; deal cards; a list view with the same filters behind a view toggle (ticket 38, F-35; DESIGN.md Sales Design) |
| Screen type and width | List — 1200 px, full width: header, filters, data grid |
| Layout | `layouts.AdminLayout` |
| Route | None — opened from the admin navigation (Sales & Operations → Deals) |

## 2. Elements

| Element | Theme component | Why this component | Role | `wrap_on` | Tooltip phrase key |
|---|---|---|---|---|---|
| Page title | `m3.Heading` (`hdg_deals`) | "Deals", `headline-large` | `page-title` | — | `deals.title` |
| New Deal button | `m3.Button` (`btn_new_deal`, filled) | Opens DealEditorForm in create mode | `primary-button` | — | `deals.new` |
| View toggle | `m3.ButtonGroup` (`bgrp_deals_view`, connected, single: Board/List) | A list view with the same filters sits behind a view toggle (DESIGN.md Sales Design) | — | mobile | `deals.view.board`, `deals.view.list` |
| Stage column | Anvil `ColumnPanel` (`col_stage_*`) | One column per stage: Lead, Qualified, Proposal, Negotiation, Won, Lost, each with its count and total value (DESIGN.md Sales Design) | — | mobile | `deals.stage.lead` … `deals.stage.lost` |
| Deal card | `m3.InteractiveCard` (`icard_deal_*`, outlined) | Title, contact or company, value, owner and expected close date (DESIGN.md Sales Design) | `surface-card` | mobile | — |
| Stage move | the card's drag or stage menu | Cards move by drag or by a stage menu (DESIGN.md Sales Design) | — | — | `deals.move` |
| Stage menu | `m3.DropdownMenu` (`dd_stage_move_*`) | The stage menu's move action | `outlined-select` | — | `deals.move` |
| Lost reason field | `m3.TextBox` (`txt_lost_reason_*`, outlined) | For `lost` (schema) | `outlined-input` | mobile | `deals.lost_reason` |
| List view grid | Anvil `DataGrid` (`dg_deals_list`) | The list view's table of the same deals and filters | — | mobile | — |
| Owner filter | `m3.DropdownMenu` (`dd_deal_owner`, `allow_none=True`) | The same filters as the board | `outlined-select` | — | `tasks.filter.owner` |
| Help icon | `components.HelpTooltipComponent` (help entry `sales.board`) | The feature's tooltip and help video | — | — | `help.sales.board.tooltip` |

Staff see and move only their own deals (ticket 38).

## 3. Data

| Element | Table | Column |
|---|---|---|
| Board | `deals` | `title`, `contact_id`, `company_id`, `owner_id`, `stage`, `value`, `expected_close_date`, `lost_reason` |

## 4. Actions

| Action | Event handler | Server function | Result |
|---|---|---|---|
| Load the board | `form_show` → `load_deals` | `list_deals` (`server_sales/deal_service`; Staff own) | The columns and cards arrive |
| Move a deal | the drag or `dd_stage_move_*_change` | `deal_service` stage move | The deal moves; `lost` records the reason; the timeline records the move |
| Open a deal | `icard_deal_*_click` | None | `sales.DealEditorForm` opens |
| New deal | `btn_new_deal_click` | None | `sales.DealEditorForm` opens |

## 5. Empty State

| Part | Content |
|---|---|
| Icon | Pipeline icon (`mi:view_kanban`) |
| Heading | `deals.empty.title` (DESIGN.md: "No deals yet") |
| Body text | `deals.empty.body` (DESIGN.md: "Deals follow each sale from lead to won.") |
| Primary action | `deals.new` (+ New Deal) |

## 6. Access

| Role | Scope |
|---|---|
| Owner | all |
| Manager | all |
| Admin | all |
| Staff | all |
| Customer | none |
| SignedOut | none |

Staff own only (access matrix).

## 7. Text

| Phrase key | English text |
|---|---|
| `deals.title` | Deals |
| `deals.new` | + New Deal |
| `deals.view.board` | Board |
| `deals.view.list` | List |
| `deals.stage.lead` | Lead |
| `deals.stage.qualified` | Qualified |
| `deals.stage.proposal` | Proposal |
| `deals.stage.negotiation` | Negotiation |
| `deals.stage.won` | Won |
| `deals.stage.lost` | Lost |
| `deals.move` | Move to |
| `deals.lost_reason` | Why it was lost |
| `deals.empty.title` | No deals yet |
| `deals.empty.body` | Deals follow each sale from lead to won. |

## 8. Help Videos

| Process | Help video |
|---|---|
| Working your pipeline | `master_template` help entry `sales.board` — played in the app language |

## 9. User Flows

| Flow | Part this form plays |
|---|---|
| UF-40 Deals, companies and quotes | The board and its list view |
| UF-41 Customer accepts a quote | A linked deal moves to won on acceptance |
