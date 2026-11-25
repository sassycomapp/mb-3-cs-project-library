---
title: "LandingPageListForm-requirement"
status: draft
doc-id: "mb-13eea6bb"
updated: "2026-11-25"
---
# LandingPageListForm — Screen Requirement

## 1. Form

| Item | Value |
|---|---|
| Module path | `landing_pages.LandingPageListForm` |
| Purpose | The landing pages list with their views and conversions (ticket 30, F-08; UF-18) |
| Screen type and width | List — 1200 px, full width: header, filters, data grid |
| Layout | `layouts.AdminLayout` |
| Route | None — opened from the admin navigation (Content & Website → Landing Pages) |

## 2. Elements

| Element | Theme component | Why this component | Role | `wrap_on` | Tooltip phrase key |
|---|---|---|---|---|---|
| Page title | `m3.Heading` (`hdg_landing_pages`) | "Landing Pages", `headline-large` | `page-title` | — | `landing.title` |
| Create button | `m3.Button` (`btn_create_landing`, filled) | Opens LandingPageEditorForm in create mode (DESIGN.md empty states: "+ Create Landing Page") | `primary-button` | — | `landing.create` |
| Landing pages grid | Anvil `DataGrid` (`dg_landing_pages`) | Title, address, template, status, views, conversions, conversion rate | — | mobile | — |
| Landing page row | Anvil `DataRowPanel` (`drp_landing_*`) | One landing page per row | — | — | — |
| Status badge | `m3.Chip` (`chip_landing_status_*`, type `input`, removable=False) | Draft or Published | — | — | — | — | — |
| Help icon | `components.HelpTooltipComponent` (help entry `landing.list`) | The feature's tooltip and help video | — | — | `help.landing.list.tooltip` |

## 3. Data

| Element | Table | Column |
|---|---|---|
| Grid | `landing_pages` | `title`, `slug`, `template_type`, `status`, `views_count`, `conversions_count` |

## 4. Actions

| Action | Event handler | Server function | Result |
|---|---|---|---|
| Load the list | `form_show` → `load_landing_pages` | `landing_page_service` list read (`server_marketing/landing_page_service`) | The grid arrives |
| Create a landing page | `btn_create_landing_click` | None | `landing_pages.LandingPageEditorForm` opens in create mode |
| Open a landing page | the row click | None | `landing_pages.LandingPageEditorForm` opens |

## 5. Empty State

| Part | Content |
|---|---|
| Icon | Page icon (`mi:web`) |
| Heading | `landing.empty.title` (DESIGN.md: "No landing pages yet") |
| Body text | `landing.empty.body` (DESIGN.md: "Landing pages capture leads from marketing campaigns. Create one to start collecting sign-ups.") |
| Primary action | `landing.create` (+ Create Landing Page) |

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
| `landing.title` | Landing Pages |
| `landing.create` | + Create Landing Page |
| `landing.status.draft` | Draft |
| `landing.status.published` | Published |
| `landing.empty.title` | No landing pages yet |
| `landing.empty.body` | Landing pages capture leads from marketing campaigns. Create one to start collecting sign-ups. |

## 8. Help Videos

| Process | Help video |
|---|---|
| Managing landing pages | `master_template` help entry `landing.list` — played in the app language |

## 9. User Flows

| Flow | Part this form plays |
|---|---|
| UF-18 Landing page creation | The list and its analytics columns |
| UF-20 Lead capture | The capture pages' conversions roll up here |
