---
title: "AdminLayout-requirement"
status: draft
doc-id: "mb-5ce20563"
updated: "2026-11-25"
---
# AdminLayout — Screen Requirement

## 1. Form

| Item | Value |
|---|---|
| Module path | `layouts.AdminLayout` |
| Purpose | The admin shell every signed-in team page sits on: the expressive navigation rail with the admin menu per architecture reference section 9, the three always-available controls (tooltips, help videos, dashboard), notifications, the user menu, sign out, and the sample-data banner (ticket 01, F-31, F-25) |
| Screen type and width | Layout (shell) built on the theme's `m3.Layouts.NavigationRailLayout`; it hosts every admin screen, and each page form's own screen type sets its width (spec-ui-standards section 7) |
| Layout | `m3.Layouts.NavigationRailLayout`, `rail_style="expressive"`, `rail_adaptive=True` (DESIGN.md AdminLayout: the expressive rail, expanded on wide windows); `show_sidesheet=True` for guided flows |
| Route | None — opened with `open_form()` from the layouts package; every admin page form names AdminLayout as its layout |

## 2. Elements

| Element | Theme component | Why this component | Role | `wrap_on` | Tooltip phrase key |
|---|---|---|---|---|---|
| The shell | `m3.Layouts.NavigationRailLayout` | The theme's own admin layout; `NavigationDrawerLayout` cannot host the admin menu per architecture reference section 9 (AdminLayout is the NavigationRailLayout) | — | — | — |
| Sample-data banner container | Anvil `FlowPanel` (`flow_banner`) | Anvil canonical container (DESIGN.md rule 3); holds the banner in one row that wraps on a phone | `banner` role: container `secondary-container`, content `on-secondary-container` (DESIGN.md Sample Data Banner) | mobile | — |
| Banner icon | `m3.IconButton` (`ico_sample_data`, decorative) | The theme's icon button; decorative (no click), so `ico_` per nomenclature 1.6 | — | — | `admin.banner.sample.icon` |
| Banner text | `m3.Text` (`text_banner`) | The theme's Text replaces the banned Label | `body-medium` | — | `admin.banner.sample.text` |
| Clear sample data button | `m3.Button` (`btn_clear_sample`, outlined) | Secondary action is outlined (spec-ui-standards section 8.1); visible to the Owner only, hidden for every other role | `secondary-button` | — | `admin.banner.sample.clear` |
| Title | `m3.Text` (`text_app_title`, bold) | The app title in the top slot; Text, not Heading, because it is app chrome, not a page title | `title-medium` | — | — |
| Help videos menu | `m3.IconButtonMenu` (`ibm_help_videos`, icon `mi:help_outline`) | An icon-button menu is the theme's control for a menu of items (spec-ui-standards section 12.1: the help-videos control opens a menu); one control for every process on the current page | `icon-button` | — | `layout.help.videos` |
| Notifications menu | `m3.IconButtonMenu` (`ibm_notifications`, icon `mi:notifications`) | The theme's icon-button menu opens the notification feed; the inbox-style unread count uses the theme's Badge | `icon-button` | — | `layout.notifications` |
| Unread badge | `m3.Badge` (`bdg_unread`, count = unread) | The theme's Badge marks unread items (Badge.md); placed with the notifications icon | — | — | — |
| User menu | `m3.AvatarMenu` (`avm_user`) | The theme's avatar menu holds the signed-in user's menu; `user_name` from the session user | — | — | `layout.user.menu` |
| Sidebar navigation | the layout's `navigation` slot | Navigation is the layout's own `NavigationLink` components (spec-ui-standards section 9.1; adr-navigation-lambda-link-open-form) | — | — | — |
| Dashboard link | `m3.NavigationLink` (`nav_dashboard`, icon `mi:dashboard`) | The layout's own navigation link; every admin menu item is one | `nav-item` (selected: `nav-item-selected`) | — | `admin.nav.dashboard` |
| Inbox link | `m3.NavigationLink` (`nav_inbox`, icon `mi:mail`) | As above; its badge count is the user's unread count | `nav-item` | — | `admin.nav.inbox` |
| Group label (each of Sales & Operations, Customers & Marketing, Content & Website, Finance & Reports) | `m3.Text` (`text_group_*`, label style, bold) | Section labels in the rail; Text, not Heading, at `label-small` scale | — | — | `admin.nav.group.sales`, `admin.nav.group.customers`, `admin.nav.group.content`, `admin.nav.group.finance` |
| Bookings / Services / Orders / Packages / Deals / Quotes links | `m3.NavigationLink` (`nav_bookings`, `nav_services`, `nav_orders`, `nav_packages`, `nav_deals`, `nav_quotes`) | The Sales & Operations group per architecture reference section 9 | `nav-item` | — | `admin.nav.bookings`, `admin.nav.services`, `admin.nav.orders`, `admin.nav.packages`, `admin.nav.deals`, `admin.nav.quotes` |
| Contacts / Companies / Campaigns / Broadcasts / Segments / Tasks / Automations / Reviews links | `m3.NavigationLink` (`nav_contacts`, `nav_companies`, `nav_campaigns`, `nav_broadcasts`, `nav_segments`, `nav_tasks`, `nav_automations`, `nav_reviews`) | The Customers & Marketing group; Campaigns, Broadcasts, Segments and Automations are visible only while `config.marketing_enabled` is True | `nav-item` | — | `admin.nav.contacts`, `admin.nav.companies`, `admin.nav.campaigns`, `admin.nav.broadcasts`, `admin.nav.segments`, `admin.nav.tasks`, `admin.nav.automations`, `admin.nav.reviews` |
| Blog / Pages / Landing Pages / Media links | `m3.NavigationLink` (`nav_blog`, `nav_pages`, `nav_landing_pages`, `nav_media`) | The Content & Website group; Blog is visible only while `config.blog_enabled` is True | `nav-item` | — | `admin.nav.blog`, `admin.nav.pages`, `admin.nav.landing_pages`, `admin.nav.media` |
| Invoices / Payments / Expenses / Accounts / Time Entries / Reports / Analytics links | `m3.NavigationLink` (`nav_invoices`, `nav_payments`, `nav_expenses`, `nav_accounts`, `nav_time_entries`, `nav_reports`, `nav_analytics`) | The Finance & Reports group | `nav-item` | — | `admin.nav.invoices`, `admin.nav.payments`, `admin.nav.expenses`, `admin.nav.accounts`, `admin.nav.time_entries`, `admin.nav.reports`, `admin.nav.analytics` |
| Settings link | `m3.NavigationLink` (`nav_settings`, icon `mi:settings`) | Visible to Owner and Manager only (architecture reference section 9) | `nav-item` | — | `admin.nav.settings` |
| Vault link | `m3.NavigationLink` (`nav_vault`, icon `mi:lock`) | Owner only | `nav-item` | — | `admin.nav.vault` |
| Help link | `m3.NavigationLink` (`nav_help`, icon `mi:support_agent`) | Every team member | `nav-item` | — | `admin.nav.help` |
| Tooltips switch | `m3.Switch` (`sw_tooltips`) | The theme's Switch is the on/off control (spec-ui-standards section 12.1); saves `users.preferences.tooltips` | — | — | `layout.tooltips.switch` |
| Sign out | `m3.Link` (`lnk_sign_out`) | Always visible at the sidebar bottom for all roles (DESIGN.md AdminLayout) | `tertiary-button` | — | `layout.sign_out` |
| Content slot | the layout's `content` slot | The page form opened with `open_form()` fills it; the layout stays (scaffold-spec Navigation Architecture) | — | — | — |
| Page loading indicator | `m3.LoadingIndicator` (`load_page`, hidden) | Shown while `get_layout_context` and a page's first server call run (brand profile section 25: a server call shows a progress indicator) | — | — | — |

Every navigation link's click is a lambda handler that captures its loop variable (`lambda e, f=form_class: self._nav_click(f)`, adr-navigation-lambda-link-open-form) and opens the form with `open_form()`; `navigate_to` is never used. Restricted links are hidden, never disabled (ticket 01, B1).

## 3. Data

| Element | Table | Column |
|---|---|---|
| Sample-data banner visibility | `sample_rows` | row presence (via `get_layout_context`) |
| Banner text | — | phrase key `admin.banner.sample.text` |
| Clear button visibility | `users` | `role` (Owner only) |
| App title | — | phrase key `layout.title` |
| Notifications badge | `notifications` | `is_read` (unread count via `get_unread_count`) |
| User menu | `users` | `full_name` (session user projection) |
| Menu item visibility (Campaigns, Broadcasts, Segments, Automations, Blog) | `config` | `value` of `marketing_enabled` / `blog_enabled` (via `get_layout_context`) |
| Settings / Vault link visibility | `users` | `role` |
| Tooltips switch | `users` | `preferences.tooltips` (via the session user) |

## 4. Actions

| Action | Event handler | Server function | Result |
|---|---|---|---|
| Open a menu page | `nav_<page>_click` (lambda per adr-navigation-lambda-link-open-form) | — | The page form opens in the content slot with `open_form(<FormClass>)`; the layout stays |
| Open the Inbox | `nav_inbox_click` | — | `inbox.TeamInboxForm` opens |
| Open the Dashboard | `nav_dashboard_click` | — | `dashboard.DashboardForm` opens |
| Open Settings | `nav_settings_click` | — | `settings.SettingsForm` opens |
| Open the Vault | `nav_vault_click` | — | `settings.VaultForm` opens (Owner only; TOTP on every open) |
| Open Help | `nav_help_click` | — | `support.HelpForm` opens |
| Sign out | `lnk_sign_out_click` | `anvil.users.logout` | The session ends; `auth.LoginForm` shows |
| Load the layout | `form_show` → `load_context` | `get_layout_context` (`server_settings/settings_service`) | Palette, feature switches, onboarding status and the sample-banner state arrive; only what the calling role may see (ticket 01) |
| Refresh the unread count | `tmr_unread_tick` (Timer) | `get_unread_count` (`server_shared/notification_service`) | The badge shows the user's unread count; polling per adr-real-time-and-background-tasks |
| Switch tooltips | `sw_tooltips_change` | `update_preferences` (`server_shared/notification_service`) | `users.preferences.tooltips` saved for the signed-in user; applies on every page |
| Open the help-videos menu | `ibm_help_videos_click` | — | The menu lists a help video link for every process on the current page, with the all/this-page switch (spec-ui-standards section 12.1) |
| Open the notifications feed | `ibm_notifications_click` | `get_notifications` (`server_shared/notification_service`) | The user's notifications, newest first; an item opens its `data.url` target |
| Mark a notification read | `mnu_notification_click` | `mark_read` (`server_shared/notification_service`) | The row is marked read; the badge updates |
| Clear sample data | `btn_clear_sample_click` | `clear_sample_data` (`server_settings/sample_data_service`) | Owner only; the confirmation dialog states the step cannot be undone (UF-43); the banner disappears when the task completes |

## 5. Empty State

| Part | Content |
|---|---|
| Icon | None — the layout is a shell, not a data listing; it always has the menu and controls |
| Heading | None — not applicable: the layout shows no data of its own |
| Body text | None — not applicable |
| Primary action | None — not applicable |

## 6. Access

| Role | Scope |
|---|---|
| Owner | all |
| Manager | all |
| Admin | all |
| Staff | all |
| Customer | none |
| SignedOut | none |

Menu visibility inside the layout is narrower than the shell's: Settings Owner/Manager, Vault Owner, Campaigns/Broadcasts/Segments/Automations while `marketing_enabled`, Blog while `blog_enabled` (architecture reference section 9). Hiding is the interface only; the server enforces (ticket 01, B1).

## 7. Text

| Phrase key | English text |
|---|---|
| `layout.title` | Mybizz CS |
| `admin.banner.sample.text` | You are viewing sample data. |
| `admin.banner.sample.clear` | Clear sample data |
| `admin.banner.sample.icon` | Sample data |
| `admin.nav.dashboard` | Dashboard |
| `admin.nav.inbox` | Inbox |
| `admin.nav.group.sales` | Sales & Operations |
| `admin.nav.group.customers` | Customers & Marketing |
| `admin.nav.group.content` | Content & Website |
| `admin.nav.group.finance` | Finance & Reports |
| `admin.nav.bookings` | Bookings |
| `admin.nav.services` | Services |
| `admin.nav.orders` | Orders |
| `admin.nav.packages` | Packages |
| `admin.nav.deals` | Deals |
| `admin.nav.quotes` | Quotes |
| `admin.nav.contacts` | Contacts |
| `admin.nav.companies` | Companies |
| `admin.nav.campaigns` | Campaigns |
| `admin.nav.broadcasts` | Broadcasts |
| `admin.nav.segments` | Segments |
| `admin.nav.tasks` | Tasks |
| `admin.nav.automations` | Automations |
| `admin.nav.reviews` | Reviews |
| `admin.nav.blog` | Blog |
| `admin.nav.pages` | Pages |
| `admin.nav.landing_pages` | Landing Pages |
| `admin.nav.media` | Media |
| `admin.nav.invoices` | Invoices |
| `admin.nav.payments` | Payments |
| `admin.nav.expenses` | Expenses |
| `admin.nav.accounts` | Accounts |
| `admin.nav.time_entries` | Time Entries |
| `admin.nav.reports` | Reports |
| `admin.nav.analytics` | Analytics |
| `admin.nav.settings` | Settings |
| `admin.nav.vault` | Vault |
| `admin.nav.help` | Help |
| `layout.sign_out` | Sign out |
| `layout.tooltips.switch` | Tooltips |
| `layout.tooltips.on` | Tooltips on |
| `layout.tooltips.off` | Tooltips off |
| `layout.help.videos` | Help videos |
| `layout.help.videos.all` | All help videos |
| `layout.help.videos.this_page` | This page only |
| `layout.notifications` | Notifications |
| `layout.user.menu` | Account menu |
| `layout.user.profile` | Profile |
| `layout.user.settings` | Settings |

English texts follow the brand voice: direct, honest, empathetic, confident but not arrogant, human, practical; the next action is always obvious (DESIGN.md, Tone of text).

## 8. Help Videos

| Process | Help video |
|---|---|
| Finding your way around the admin area | `master_template` help entry `admin.navigation` — one video per language, played in the app language |
| The three always-available controls | `master_template` help entry `layout.controls` |
| Sample data and clearing it | `master_template` help entry `admin.sample_data` |
| Notifications and the unread badge | `master_template` help entry `layout.notifications` |
| The help-videos menu itself | `master_template` help entry `layout.help_videos` |

Every process's video address is held per language in `master_template` (ticket 10; F-25), played through `components.HelpTooltipComponent`'s dialog.

## 9. User Flows

| Flow | Part this form plays |
|---|---|
| UF-02 Owner onboarding | The shell the Owner's Settings links return to; the sample-data banner shows after onboarding completes |
| UF-03 Staff invitation and sign-in | The shell every invitee lands on; role visibility of the menu applies at once |
| UF-04 Opening the Vault | The shell that hosts the Vault link and the VaultForm page |
| UF-11 Admin daily operations | The shell of the morning routine: Dashboard → calendar → bookings |
| UF-13 Time tracking and invoicing hours | The shell hosting Time Entries navigation |
| UF-14 Invoices, payment terms and refunds | The shell hosting Invoices and Payments navigation |
| UF-16 Content management and edit mode | The shell hosting Pages, Blog and Media navigation |
| UF-21 Contact management | The shell hosting Contacts and Companies navigation |
| UF-23 / UF-24 Campaigns and broadcasts | The shell hosting the Campaigns and Broadcasts navigation |
| UF-25 Marketing dashboard | The shell hosting the Analytics navigation |
| UF-26 Tasks and automation | The shell hosting Tasks and Automations navigation |
| UF-27 Reviews and testimonials | The shell hosting Reviews navigation |
| UF-29 Customer portal | Not this layout — the portal sits on CustomerLayout |
| UF-32 Expenses, accounts reports and export | The shell hosting Expenses and Accounts navigation |
| UF-33 Restoring or viewing data | The shell hosting the Settings → Data navigation |
| UF-34 Help | The shell hosting the Help navigation |
| UF-35 Team Inbox | The shell hosting the Inbox navigation and its unread badge |
| UF-38 Error recovery | The shell stays; a failed page load shows its message in the content slot |
| UF-43 Clearing the sample data | The banner lives here on every admin screen |
| UF-44 Trying the demo apps | The demo sign-in lands here with `is_demo_visitor` state |
| UF-45 / UF-46 Plan and billing | The shell hosting the Settings → Plan and billing navigation |
