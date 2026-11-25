---
title: "CustomerLayout-requirement"
status: draft
doc-id: "mb-a0dad4d8"
updated: "2026-11-25"
---
# CustomerLayout — Screen Requirement

## 1. Form

| Item | Value |
|---|---|
| Module path | `layouts.CustomerLayout` |
| Purpose | The customer portal shell: the navigation drawer with the portal menu per architecture reference section 9, the three always-available controls, notifications, the user menu and sign out (ticket 01) |
| Screen type and width | Layout (shell) built on the theme's `m3.Layouts.NavigationDrawerLayout`; it hosts every portal page, and each page form's own screen type sets its width |
| Layout | `m3.Layouts.NavigationDrawerLayout`; the drawer auto-collapses to a bottom app bar or modal drawer on small screens (adr-responsive-behaviour-mechanism) |
| Route | None — opened with `open_form()`; every portal page form names CustomerLayout as its layout |

## 2. Elements

| Element | Theme component | Why this component | Role | `wrap_on` | Tooltip phrase key |
|---|---|---|---|---|---|
| The shell | `m3.Layouts.NavigationDrawerLayout` | The theme's drawer layout for the portal (architecture reference section 9: CustomerLayout is the NavigationDrawerLayout) | — | — | — |
| Help videos menu | `m3.IconButtonMenu` (`ibm_help_videos`, icon `mi:help_outline`) | The always-available help control (spec-ui-standards section 12.1) | `icon-button` | — | `layout.help.videos` |
| Notifications menu | `m3.IconButtonMenu` (`ibm_notifications`, icon `mi:notifications`) | The customer's notification feed | `icon-button` | — | `layout.notifications` |
| Unread badge | `m3.Badge` (`bdg_unread`) | The theme's Badge marks unread items | — | — | — |
| User menu | `m3.AvatarMenu` (`avm_user`) | The signed-in customer's menu; opens Account | — | — | `layout.user.menu` |
| My Dashboard link | `m3.NavigationLink` (`nav_my_dashboard`, icon `mi:dashboard`) | The layout's own navigation link | `nav-item` | — | `portal.nav.dashboard` |
| My Bookings link | `m3.NavigationLink` (`nav_my_bookings`, icon `mi:event`) | Portal menu per architecture reference section 9 | `nav-item` | — | `portal.nav.bookings` |
| My Packages link | `m3.NavigationLink` (`nav_my_packages`, icon `mi:card_giftcard`) | Portal menu | `nav-item` | — | `portal.nav.packages` |
| My Quotes link | `m3.NavigationLink` (`nav_my_quotes`, icon `mi:description`) | Portal menu | `nav-item` | — | `portal.nav.quotes` |
| My Invoices link | `m3.NavigationLink` (`nav_my_invoices`, icon `mi:receipt_long`) | Portal menu | `nav-item` | — | `portal.nav.invoices` |
| My Reviews link | `m3.NavigationLink` (`nav_my_reviews`, icon `mi:star`) | Portal menu | `nav-item` | — | `portal.nav.reviews` |
| Account link | `m3.NavigationLink` (`nav_account`, icon `mi:person`) | Portal menu | `nav-item` | — | `portal.nav.account` |
| Sign out | `m3.Link` (`lnk_sign_out`) | Always visible at the sidebar bottom | `tertiary-button` | — | `layout.sign_out` |
| Content slot | the layout's `content` slot | The portal page form fills it; the layout stays | — | — | — |
| Page loading indicator | `m3.LoadingIndicator` (`load_page`, hidden) | Shown while a portal page's first server call runs | — | — | — |

Every link's click is a lambda handler capturing its loop variable; navigation opens the form with `open_form(<FormClass>)`; `navigate_to` is never used.

## 3. Data

| Element | Table | Column |
|---|---|---|
| App title | — | phrase key `layout.title` |
| Notifications badge | `notifications` | `is_read` (unread count via `get_unread_count`) |
| User menu | `users` | `full_name` (session user projection) |
| Link visibility | `users` | `role` (Customer only; every link is always shown to the Customer role) |

## 4. Actions

| Action | Event handler | Server function | Result |
|---|---|---|---|
| Open a portal page | `nav_<page>_click` | — | The portal form opens in the content slot; the layout stays |
| Sign out | `lnk_sign_out_click` | `anvil.users.logout` | The session ends; `auth.LoginForm` shows |
| Refresh the unread count | `tmr_unread_tick` (Timer) | `get_unread_count` (`server_shared/notification_service`) | The badge shows the customer's unread count |
| Open the notifications feed | `ibm_notifications_click` | `get_notifications` (`server_shared/notification_service`) | The customer's notifications, newest first |
| Mark a notification read | `mnu_notification_click` | `mark_read` (`server_shared/notification_service`) | The row is marked read; the badge updates |
| Open the help-videos menu | `ibm_help_videos_click` | — | A help video link for every process on the current page, with the all/this-page switch |
| Switch tooltips | `sw_tooltips_change` | `update_preferences` (`server_shared/notification_service`) | `users.preferences.tooltips` saved for the signed-in customer |

## 5. Empty State

| Part | Content |
|---|---|
| Icon | None — the layout is a shell; it always has the menu and controls |
| Heading | None — not applicable: the layout shows no data of its own |
| Body text | None — not applicable |
| Primary action | None — not applicable |

## 6. Access

| Role | Scope |
|---|---|
| Owner | all |
| Manager | all |
| Admin | all |
| Staff | none |
| Customer | all |
| SignedOut | none |

The Owner, Manager and Admin scopes let the client's team open the portal for support, exactly as `access-matrix.yaml` scopes the portal forms.

## 7. Text

| Phrase key | English text |
|---|---|
| `layout.title` | Mybizz CS |
| `portal.nav.dashboard` | My Dashboard |
| `portal.nav.bookings` | My Bookings |
| `portal.nav.packages` | My Packages |
| `portal.nav.quotes` | My Quotes |
| `portal.nav.invoices` | My Invoices |
| `portal.nav.reviews` | My Reviews |
| `portal.nav.account` | Account |
| `layout.sign_out` | Sign out |
| `layout.tooltips.switch` | Tooltips |
| `layout.help.videos` | Help videos |
| `layout.notifications` | Notifications |
| `layout.user.menu` | Account menu |

## 8. Help Videos

| Process | Help video |
|---|---|
| Finding your way around the customer portal | `master_template` help entry `portal.navigation` — one video per language, played in the app language |
| The notifications feed | `master_template` help entry `layout.notifications` |

## 9. User Flows

| Flow | Part this form plays |
|---|---|
| UF-05–UF-08, UF-10 Bookings and packages | The shell the customer reaches the portal from after booking |
| UF-29 Customer portal | The shell of the whole portal: My Dashboard, My Bookings, My Packages, My Quotes, My Invoices, My Reviews, Account |
| UF-41 Customer accepts a quote | The shell hosting My Quotes |
| UF-27 Reviews | The shell hosting My Reviews |
