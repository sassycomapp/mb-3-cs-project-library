---
title: "ContactViewerForm-requirement"
status: draft
doc-id: "mb-c25e0f67"
updated: "2026-11-25"
---
# ContactViewerForm — Screen Requirement

## 1. Form

| Item | Value |
|---|---|
| Module path | `crm.ContactViewerForm` |
| Purpose | The contact's record: statistics, company, the activity timeline, deals, quotes, active campaigns, packages, open invoices and quick actions (ticket 20, F-17; UF-21) |
| Screen type and width | Editor, viewer, form — 900 px, sectioned panels |
| Layout | `layouts.AdminLayout` |
| Route | None — opened from the contact grid and from the Team Inbox's contact link |

## 2. Elements

| Element | Theme component | Why this component | Role | `wrap_on` | Tooltip phrase key |
|---|---|---|---|---|---|
| Page title | `m3.Heading` (`hdg_contact_viewer`) | The contact's name, `headline-large` | `page-title` | — | — |
| Avatar | `m3.Avatar` (`av_contact`) | The theme's avatar with the contact's initials | — | — | — |
| Header action row | Anvil `FlowPanel` (`flow_actions`) | Edit, quick actions | — | mobile | — |
| Edit button | `m3.Button` (`btn_edit_contact`, outlined) | Opens `crm.ContactEditorForm` | `secondary-button` | — | `users.row.edit` |
| Quick actions menu | `m3.SplitButton` (`split_quick`, text "Actions") | The quick actions: new booking, new quote, new deal, new task, send email (UF-21 quick actions) | — | — | `crm.contact.quick_actions` |
| Statistics panel | `m3.Card` (`card_stats`, outlined) | Total spent, total transactions, average order value, last contact date (UF-21 statistics) | `surface-card` | mobile | — |
| Company panel | `m3.Card` (`card_company`, outlined) | The linked company and its link | `surface-card` | mobile | — |
| Timeline panel | `m3.Card` (`card_timeline`, outlined) | The contact's timeline: contact events in order (F-17) | `surface-card` | mobile | — |
| Timeline row | `m3.ListItem` (`li_event_*`, leading_icon per event type) | One event per row: type icon, headline, supporting text, trailing date; every email to and from the contact is on the timeline (F-17, F-18) | — | — | — |
| Deals panel | `m3.Card` (`card_deals`, outlined) | The contact's deals | `surface-card` | mobile | — |
| Quotes panel | `m3.Card` (`card_quotes`, outlined) | The contact's quotes | `surface-card` | mobile | — |
| Campaigns panel | `m3.Card` (`card_campaigns`, outlined) | Active campaigns the contact is enrolled in | `surface-card` | mobile | — |
| Packages panel | `m3.Card` (`card_packages`, outlined) | The contact's customer packages and their balances | `surface-card` | mobile | — |
| Open invoices panel | `m3.Card` (`card_invoices`, outlined) | Open invoices | `surface-card` | mobile | — |
| AI helper trigger | `components.AIHelperComponent` (trigger) | The client sentiment summary assist (assist 9) reads this contact's history (F-26) | — | — | `ai.open` |
| Help icon | `components.HelpTooltipComponent` (help entry `crm.contact_viewer`) | The feature's tooltip and help video | — | — | `help.crm.contact_viewer.tooltip` |

## 3. Data

| Element | Table | Column |
|---|---|---|
| Header and stats | `contacts` | `first_name`, `last_name`, `email`, `phone`, `total_spent`, `total_transactions`, `average_order_value`, `last_contact_date`, `company_id` |
| Timeline | `contact_events` | `event_type`, `event_date`, `event_data`, `related_id`, `user_visible` |
| Deals | `deals` | rows linked by `contact_id` |
| Quotes | `quotes` | rows linked by `contact_id` |
| Campaigns | `contact_campaigns` | `campaign_id`, `status`, `sequence_day` |
| Packages | `customer_packages` | `sessions_total`, `sessions_remaining`, `expires_at`, `status` |
| Open invoices | `invoice` | `invoice_number`, `total_amount`, `amount_paid`, `status`, `due_date` |

## 4. Actions

| Action | Event handler | Server function | Result |
|---|---|---|---|
| Load the contact | `form_show` → `load_contact` | `get_contact` (`server_customers/contact_service`) | The contact, its timeline and its records arrive; internal_notes restricted per the access matrix |
| Edit | `btn_edit_contact_click` | None | `crm.ContactEditorForm` opens |
| Quick action | `split_quick_item_click` | None | The chosen flow's editor opens with this contact preselected |
| Open a related record | the panel row clicks | None | The deal, quote, campaign, package or invoice opens in its own viewer |

## 5. Empty State

| Part | Content |
|---|---|
| Icon | `mi:person` |
| Heading | `crm.contact.empty.title` |
| Body text | `crm.contact.empty.body` — for the timeline and record panels when the contact has none of that record yet |
| Primary action | None — the panels explain what arrives automatically |

## 6. Access

| Role | Scope |
|---|---|
| Owner | all |
| Manager | all |
| Admin | all |
| Staff | none |
| Customer | none |
| SignedOut | none |

Per the access matrix; internal_notes restricted to the roles the access matrix allows (IN-04).

## 7. Text

| Phrase key | English text |
|---|---|
| `crm.contact.quick_actions` | Actions |
| `crm.contact.quick.new_booking` | New booking |
| `crm.contact.quick.new_quote` | New quote |
| `crm.contact.quick.new_deal` | New deal |
| `crm.contact.quick.new_task` | New task |
| `crm.contact.quick.send_email` | Send email |
| `crm.contact.stats.spent` | Total spent |
| `crm.contact.stats.transactions` | Transactions |
| `crm.contact.stats.average` | Average value |
| `crm.contact.stats.last` | Last contact |
| `crm.contact.timeline.title` | Timeline |
| `crm.contact.company.title` | Company |
| `crm.contact.deals.title` | Deals |
| `crm.contact.quotes.title` | Quotes |
| `crm.contact.campaigns.title` | Active campaigns |
| `crm.contact.packages.title` | Packages |
| `crm.contact.invoices.title` | Open invoices |
| `crm.contact.empty.title` | Nothing here yet |
| `crm.contact.empty.body` | As you work with this contact, its history appears here. |

## 8. Help Videos

| Process | Help video |
|---|---|
| Reading a contact's record | `master_template` help entry `crm.contact_viewer` — played in the app language |
| The timeline | `master_template` help entry `crm.timeline` |

## 9. User Flows

| Flow | Part this form plays |
|---|---|
| UF-21 Contact management | The contact's record with everything about it |
| UF-35 Team Inbox | The contact link in the message view opens here |
| UF-40 Deals and quotes | The deals and quotes panels |
| UF-41 Customer accepts a quote | The quote's state appears here after acceptance |
