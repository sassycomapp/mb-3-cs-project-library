---
title: "ActivityFeed-requirement"
status: draft
doc-id: "mb-9314067c"
updated: "2026-11-25"
---
# ActivityFeed — Screen Requirement

## 1. Form

| Item | Value |
|---|---|
| Module path | `dashboard.DashboardForm.ActivityFeed` |
| Purpose | The dashboard's activity feed: the latest significant actions (ticket 25, F-04) |
| Screen type and width | Card in a grid — 320 px, embedded |
| Layout | A local component of `dashboard.DashboardForm`; moved to this module path when DashboardForm is built (David's Step 22 rule) |
| Route | None — a local component |

## 2. Elements

| Element | Theme component | Why this component | Role | `wrap_on` | Tooltip phrase key |
|---|---|---|---|---|---|
| Feed card | `m3.Card` (`card_activity`, outlined) | Standard content in an outlined card | `surface-card` | mobile | — |
| Feed title | `m3.Heading` (`hdg_activity`) | "Recent activity", `headline-small` | `headline-small` | — | `dashboard.activity.title` |
| Feed list | Anvil `ColumnPanel` (`col_activity_rows`) | The latest events in time order | — | mobile | — |
| Feed row | `m3.ListItem` (`li_activity_*`) | One event per row: event icon, who did what, when | — | — | — |

## 3. Data

| Element | Table | Column |
|---|---|---|
| Feed rows | `activity_log` | `action_type`, `description`, `created_at`, `user_id` |

## 4. Actions

| Action | Event handler | Server function | Result |
|---|---|---|---|
| None | — | — | The feed is presentation; the host passes the rows |

## 5. Empty State

| Part | Content |
|---|---|
| Icon | Activity icon (`mi:timeline`) |
| Heading | `dashboard.activity.empty.title` (DESIGN.md: "No recent activity") |
| Body text | `dashboard.activity.empty.body` (DESIGN.md: "Activity from bookings, emails, and contacts will appear here as your business operates." — no action) |
| Primary action | None — the empty state explains that activity appears automatically (DESIGN.md) |

## 6. Access

| Role | Scope |
|---|---|
| Owner | all |
| Manager | all |
| Admin | all |
| Staff | all |
| Customer | none |
| SignedOut | none |

Same as the dashboard.

## 7. Text

| Phrase key | English text |
|---|---|
| `dashboard.activity.title` | Recent activity |
| `dashboard.activity.empty.title` | No recent activity |
| `dashboard.activity.empty.body` | Activity from bookings, emails, and contacts will appear here as your business operates. |

## 8. Help Videos

| Process | Help video |
|---|---|
| None | The dashboard's video covers the feed |

## 9. User Flows

| Flow | Part this form plays |
|---|---|
| UF-11 Admin daily operations | The feed the morning routine scans |
| UF-43 Clearing the sample data | The clearing is recorded here |
