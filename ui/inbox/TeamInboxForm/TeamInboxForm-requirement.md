---
title: "TeamInboxForm-requirement"
status: draft
doc-id: "mb-25fbf588"
updated: "2026-11-25"
---
# TeamInboxForm — Screen Requirement

## 1. Form

| Item | Value |
|---|---|
| Module path | `inbox.TeamInboxForm` |
| Purpose | The Team Inbox: addresses and folders, message list, conversation — three panes on desktop, one pane at a time with a back button on a phone (ticket 29, F-18; DESIGN.md Team Inbox Design; spec-team-inbox section 6) |
| Screen type and width | List — 1200 px, three panes (the Team Inbox's own pattern per DESIGN.md) |
| Layout | `layouts.AdminLayout` |
| Route | None — opened from the admin navigation |

## 2. Elements

| Element | Theme component | Why this component | Role | `wrap_on` | Tooltip phrase key |
|---|---|---|---|---|---|
| Left pane container | `m3.PaneLayout` (`pane_inbox`, layout `list_detail`) | The theme's list-detail layout gives the three panes on wide windows and one pane at a time on narrow ones (spec-team-inbox: the Inbox works in the phone browser; PaneLayout.md) | — | — | — |
| Addresses and folders list | Anvil `ColumnPanel` (`col_addresses`) | The user's addresses, each with its unread count, and the folders Inbox, Drafts, Sent, Archive, Spam and Trash; Unassigned for Owners and Managers (spec-team-inbox section 6) | — | — | — |
| Address row | `m3.ListItem` (`li_address_*`, clickable) | One address or folder per row with its unread badge | — | — | — |
| Unread badge | `m3.Badge` (`bdg_address_unread_*`) | The theme's badge shows the count (Badge.md) | — | — | — |
| Message list | Anvil `ColumnPanel` (`col_messages`) | The messages of the selected address and folder | — | — | — |
| List header | Anvil `FlowPanel` (`flow_list_header`) | Sort selector, filter chips row, search box (DESIGN.md Team Inbox Design) | — | mobile | — |
| Sort selector | `m3.DropdownMenu` (`dd_message_sort`) | Date (newest or oldest), sender, recipient, subject (UF-35 step 2) | `outlined-select` | — | `inbox.sort` |
| Filter chips | `m3.Chip` set (`chip_filter_unread`, `chip_filter_starred`, `chip_filter_attachments`, `chip_filter_date`, `chip_filter_sender`, type `filter`) | The filter chips row built from the theme's Chips (DESIGN.md Team Inbox Design) | — | mobile | `inbox.filter.unread`, `inbox.filter.starred`, `inbox.filter.attachments`, `inbox.filter.date`, `inbox.filter.sender` |
| Search box | `m3.SearchBar` (`srch_messages`) | Search across sender, recipient, subject, body and attachment names, in one address or all the user's addresses | — | — | `inbox.search` |
| Message row | `m3.ListItem` (`li_message_*`, clickable) | Sender or recipient, subject, first line, date; unread rows in title weight; star and attachment icons; a checkbox per row for bulk actions (DESIGN.md Team Inbox Design) | — | — | — |
| Bulk action bar | `m3.Toolbar` (`tbar_inbox_bulk`, docked) | Replaces the list header when rows are selected: mark read/unread, star, archive, move to address, move to Spam, delete (UF-35 step 7) | — | — | — |
| Conversation pane | Anvil `ColumnPanel` (`col_conversation`) | The open message and its whole conversation | — | — | — |
| Message body | `m3.Text` (`text_message_body`) | Plain text or sanitised HTML; remote images blocked until the user selects "Show images" (DESIGN.md Team Inbox Design) | `body-medium` | mobile | — |
| Show images button | `m3.Button` (`btn_show_images`, text) | Loads the blocked remote images | `tertiary-button` | — | `inbox.show_images` |
| Message action row | Anvil `FlowPanel` (`flow_message_actions`) | Reply, Reply All, Forward, Star, Mark as unread, Archive, Delete, Move to address, Download attachment, Add as lead (spec-team-inbox section 6) | — | mobile | one key per action, section 7 |
| Reply button | `m3.Button` (`btn_reply`, filled) | The conversation's primary action; opens ComposeForm | `primary-button` | — | `inbox.reply` |
| Attachment row | `m3.ListItem` (`li_attachment_*`) | One attachment per row with its download | — | — | — |
| Contact link | `m3.Link` (`lnk_open_contact`) | A message from a contact shows a link to the contact's record (spec-team-inbox section 6) | — | — | `inbox.open_contact` |
| Compose trigger | `m3.FloatingActionButton` (`fab_compose`, icon `mi:edit`, extended with a label) | New Email opens the compose side sheet (UF-35 step 4) | `fab-button` | — | `inbox.compose` |
| Compose host | the layout's sidesheet holding `inbox.ComposeForm` | Compose opens as a side sheet on desktop and full screen on a phone (DESIGN.md Team Inbox Design) | — | — | — |
| Back button (phone) | `m3.IconButton` (`ibtn_inbox_back`, icon `mi:arrow_back`) | One pane at a time with a back button on a phone (DESIGN.md Team Inbox Design) | `icon-button` | — | `booking.flow.back` |
| Help icon | `components.HelpTooltipComponent` (help entry `inbox.team`) | The feature's tooltip and help video | — | — | `help.inbox.team.tooltip` |

## 3. Data

| Element | Table | Column |
|---|---|---|
| Addresses and folders | `email_addresses`, `email_messages` | `address`, `kind`, `display_name`; `folder`, `is_read` counts |
| Message list | `email_messages` | `direction`, `from_address`, `from_name`, `to_addresses`, `subject`, `body_text` (first line), `is_read`, `is_starred`, `attachments`, `received_at`, `sent_at`, `thread_id`, `contact_id` |
| Conversation | `email_messages` | the thread's rows by `thread_id` |

## 4. Actions

| Action | Event handler | Server function | Result |
|---|---|---|---|
| Load the inbox | `form_show` → `load_inbox` | `list_addresses` (`server_inbox/address_service`; Owner sees every address) + `inbox_service` read | The addresses, folders and the selected list arrive |
| Choose an address or folder | `li_address_*_click` | `inbox_service` read | The list reloads |
| Open a message | `li_message_*_click` | `inbox_service` read | The conversation shows; the message is marked read (shared per address) |
| Search | `srch_messages_search` | `inbox_service` search | The list reloads with the matches |
| Bulk actions | `tbar_inbox_bulk` actions | `inbox_service` write | Read/unread, star, archive, move, Spam, delete (Trash keeps 30 days, with Restore) |
| Reply / Reply All / Forward / New Email | `btn_reply_click` etc. | None | `inbox.ComposeForm` opens in the side sheet with the context |
| Add as lead | the message action | `inbox_service` add as lead (`server_inbox/inbox_service`) | The sender becomes a lead: the lead and contact created together, linked to the message (UF-35; ADR lead-capture-simultaneous-creation) |
| Show images | `btn_show_images_click` | None | The remote images load for this message |

## 5. Empty State

| Part | Content |
|---|---|
| Icon | Mail icon (`mi:mail`) |
| Heading | `inbox.empty.title` (DESIGN.md: "No emails yet") |
| Body text | `inbox.empty.body` (DESIGN.md: "Emails to your addresses arrive here.") |
| Primary action | `inbox.compose` (New Email — DESIGN.md) |

## 6. Access

| Role | Scope |
|---|---|
| Owner | all |
| Manager | all |
| Admin | all |
| Staff | all |
| Customer | none |
| SignedOut | none |

Staff and Admin see their own addresses and desks only; Owners see every address (access matrix note; writable fields per `writable_fields.email_messages` for own addresses and desks only).

## 7. Text

| Phrase key | English text |
|---|---|
| `inbox.folder.inbox` | Inbox |
| `inbox.folder.drafts` | Drafts |
| `inbox.folder.sent` | Sent |
| `inbox.folder.archive` | Archive |
| `inbox.folder.spam` | Spam |
| `inbox.folder.trash` | Trash |
| `inbox.folder.unassigned` | Unassigned |
| `inbox.sort` | Sort |
| `inbox.search` | Search email |
| `inbox.filter.unread` | Unread |
| `inbox.filter.starred` | Starred |
| `inbox.filter.attachments` | Attachments |
| `inbox.filter.date` | Date |
| `inbox.filter.sender` | Sender |
| `inbox.reply` | Reply |
| `inbox.reply_all` | Reply all |
| `inbox.forward` | Forward |
| `inbox.star` | Star |
| `inbox.mark_unread` | Mark as unread |
| `inbox.archive` | Archive |
| `inbox.delete` | Delete |
| `inbox.move_to` | Move to |
| `inbox.download` | Download |
| `inbox.add_as_lead` | Add as lead |
| `inbox.show_images` | Show images |
| `inbox.compose` | New Email |
| `inbox.open_contact` | Open contact |
| `inbox.restore` | Restore |
| `inbox.empty.title` | No emails yet |
| `inbox.empty.body` | Emails to your addresses arrive here. |

## 8. Help Videos

| Process | Help video |
|---|---|
| Reading and replying in the Team Inbox | `master_template` help entry `inbox.team` — played in the app language |
| The folders and the addresses | `master_template` help entry `inbox.folders` |

## 9. User Flows

| Flow | Part this form plays |
|---|---|
| UF-35 Team Inbox | The whole flow: reading, replying, organising, Unassigned |
| UF-21 Contact management | Add as lead; the contact link |
| UF-23 / UF-24 Campaigns | Not this form — the campaign editor is its own form |
