---
title: "ErrorLayout-requirement"
status: draft
doc-id: "mb-f4af9727"
updated: "2026-11-25"
---
# ErrorLayout — Screen Requirement

## 1. Form

| Item | Value |
|---|---|
| Module path | `layouts.ErrorLayout` |
| Purpose | Application-level error and exception presentation (404, 403, 500) on BlankLayout: no chrome, a centred card with the error code, heading, message and a way home (DESIGN.md ErrorLayout; ticket 01) |
| Screen type and width | Layout (shell), full viewport centred; responsive at every width, no sidebar to collapse (DESIGN.md ErrorLayout) |
| Layout | `layouts.BlankLayout` (DESIGN.md ErrorLayout structure: ErrorLayout sits on BlankLayout) |
| Route | None — the routing error handlers open it |

## 2. Elements

| Element | Theme component | Why this component | Role | `wrap_on` | Tooltip phrase key |
|---|---|---|---|---|---|
| Centred container | Anvil `ColumnPanel` (`col_error`) | Anvil canonical container, centred (DESIGN.md rule 3); the layout's one content column | — | never | — |
| Error code | `m3.Heading` (`hdg_error_code`) | The code (404, 403, 500) in the display type role, colour the primary role (DESIGN.md ErrorLayout) | `display` style, `page-title` role colour `Primary` | — | — |
| Heading | `m3.Heading` (`hdg_error_title`) | "Page Not Found" / "Something Went Wrong" / "Access Denied", `headline-large`, on-surface colour | `headline-large` | — | — |
| Message | `m3.Text` (`text_error_message`) | `body-large`, colour `on-surface-variant` (DESIGN.md ErrorLayout) | `body-large` | — | — |
| Go Home button | `m3.Button` (`btn_go_home`, filled) | The one filled button is the primary action (spec-ui-standards section 8.1); navigates to the homepage signed out or to the Dashboard signed in | `primary-button` | — | `error.go_home` |
| Loading indicator | `m3.LoadingIndicator` (`load_route`, hidden) | Shown while the routing decision resolves the signed-in state | — | — | — |

All four texts are set programmatically based on the error type (DESIGN.md ErrorLayout: "All four elements are set programmatically based on error type").

## 3. Data

| Element | Table | Column |
|---|---|---|
| Error code, heading, message | — | Set in code from the error type (404 / 403 / 500); no table holds them |
| Go Home target | `users` | `role` — signed in opens the Dashboard, signed out opens the public homepage |

## 4. Actions

| Action | Event handler | Server function | Result |
|---|---|---|---|
| Go Home | `btn_go_home_click` | — | The homepage opens signed out; the Dashboard opens signed in, per DESIGN.md ErrorLayout |

## 5. Empty State

| Part | Content |
|---|---|
| Icon | None — the error card itself is the content |
| Heading | The error heading, set by code (Page Not Found / Something Went Wrong / Access Denied) |
| Body text | The error message, set by code |
| Primary action | Go Home (`error.go_home`) |

## 6. Access

| Role | Scope |
|---|---|
| Owner | all |
| Manager | all |
| Admin | all |
| Staff | all |
| Customer | all |
| SignedOut | all |

Every error screen is reachable by anyone who hits the error.

## 7. Text

| Phrase key | English text |
|---|---|
| `error.404.title` | Page Not Found |
| `error.404.message` | The page you're looking for doesn't exist or has been moved. |
| `error.500.title` | Something Went Wrong |
| `error.500.message` | An unexpected error occurred. Please try again or contact support. |
| `error.403.title` | Access Denied |
| `error.403.message` | You don't have permission to access this page. |
| `error.go_home` | Go Home |

## 8. Help Videos

| Process | Help video |
|---|---|
| None | An error screen explains itself; no help video is attached |

## 9. User Flows

| Flow | Part this form plays |
|---|---|
| UF-38 Error recovery | The 403, 404 and 500 screens; the recovery path is the Go Home button |
| UF-16 Content management | The 403 screen a non-Owner sees on a refused edit route |
