---
title: "UserManagementForm-requirement"
status: draft
doc-id: "mb-6237ae5a"
updated: "2026-11-25"
---
# UserManagementForm — Screen Requirement

## 1. Form

| Item | Value |
|---|---|
| Module path | `settings.UserManagementForm` |
| Purpose | Users and Permissions: invite team members, assign roles, deactivate and reactivate (ticket 18, F-05, F-01; UF-03) |
| Screen type and width | List — 1200 px, full width: header, filters, data grid (spec-ui-standards section 7) |
| Layout | `layouts.AdminLayout` (opened from the Users and Permissions tab) |
| Route | None — opened from `settings.SettingsForm` |

## 2. Elements

| Element | Theme component | Why this component | Role | `wrap_on` | Tooltip phrase key |
|---|---|---|---|---|---|
| Page title | `m3.Heading` (`hdg_users`) | "Users and Permissions", `headline-large` | `page-title` | — | `users.title` |
| Role filter | `m3.DropdownMenu` (`dd_role_filter`) | All, Owner, Manager, Admin, Staff, Customer (DESIGN.md UserManagementForm) | `outlined-select` | — | `users.filter.role` |
| Search field | `m3.SearchBar` (`srch_users`) | Search by name or email | — | — | `users.search` |
| Invite User button | `m3.Button` (`btn_invite`, filled) | The primary action (DESIGN.md UserManagementForm: Invite User button) | `primary-button` | — | `users.invite` |
| User table | Anvil `DataGrid` (`dg_users`) | Name, Email, Role, Status, Actions columns (DESIGN.md UserManagementForm) | — | mobile | — |
| User row | Anvil `DataRowPanel` (`drp_user_*`) | One user per row; the row template of the grid | — | — | — |
| Role badge | `m3.Chip` (`chip_role_*`, type `input`, removable=False) | Role badges as outlined pills with the role word; no colour coding by role (DESIGN.md UserManagementForm) | — | — | — |
| Status badge | `m3.Chip` (`chip_status_*`, type `input`, removable=False) | Active, Inactive, Pending as outlined pills with the word | — | — | — |
| Row actions | `m3.IconButton` (`ibtn_edit_*`, `ibtn_deactivate_*`, `ibtn_reactivate_*`) | Edit (all), Deactivate (active users), Reactivate (inactive users) (DESIGN.md UserManagementForm); icon buttons with names | `icon-button` | — | `users.row.edit`, `users.row.deactivate`, `users.row.reactivate` |
| Invite dialog | `m3.Dialog` (`dlg_invite`) | Name, email, role; for Staff, the services the provider delivers and the schedule; the personal email address on the client's domain; the desks (UF-03 step 2) | — | — | `users.invite.title` |
| Invite name field | `m3.TextBox` (`txt_invite_name`, outlined) | The invitee's name | `outlined-input` | — | `users.invite.name` |
| Invite email field | `m3.TextBox` (`txt_invite_email`, type `email`, outlined) | The invitee's email | `outlined-input` | — | `users.invite.email` |
| Invite role selector | `m3.DropdownMenu` (`dd_invite_role`) | Manager, Admin or Staff (UF-03) | `outlined-select` | — | `users.invite.role` |
| Provider services selector | `m3.DropdownMenu` (`dd_provider_services`, `multiple` behaviour via the service links) | The services the provider delivers (UF-03) | `outlined-select` | — | `users.invite.provider_services` |
| Address selector | `m3.DropdownMenu` (`dd_invite_address`) | The invitee's personal email address on the client's domain (UF-39) | `outlined-select` | — | `users.invite.address` |
| Desks selector | `m3.DropdownMenu` (`dd_invite_desks`) | The desks the invitee belongs to (UF-39) | `outlined-select` | — | `users.invite.desks` |
| Send invite button | `m3.Button` (`btn_send_invite`, filled) | Sends the invitation | `primary-button` | — | `users.invite.send` |
| Help icon | `components.HelpTooltipComponent` (help entry `settings.users`) | The feature's tooltip and help video | — | — | `help.settings.users.tooltip` |

## 3. Data

| Element | Table | Column |
|---|---|---|
| User table | `users` | `full_name`, `email`, `role`, `account_status` |
| Role filter | `users` | `role` |
| Provider services | `services` | `providers` (links → users) |
| Desks | `email_addresses` | `member_ids` |

## 4. Actions

| Action | Event handler | Server function | Result |
|---|---|---|---|
| Load the list | `form_show` → `load_users` | `settings_service` / `auth_service` read (`server_settings/settings_service`, `server_auth/auth_service`) | The users arrive filtered by role |
| Filter by role | `dd_role_filter_change` | the same read | The grid reloads |
| Search | `srch_users_search` | the same read | The grid reloads with the matches |
| Open the invite dialog | `btn_invite_click` | None | The dialog opens |
| Send the invite | `btn_send_invite_click` | `auth_service` invitation functions (`server_auth/auth_service`) | The invitation is emailed with a link to set a password (UF-03 step 3); the personal address is created for a team member (UF-39) |
| Edit a user | `ibtn_edit_*_click` | `auth_service` save | Role, name and provider details are changed; role changes are audited (SC-12) |
| Deactivate a user | `ibtn_deactivate_*_click` | `auth_service` save + `end_user_sessions` | `account_status = deactivated`; the user cannot sign in and their sessions end (UF-03 step 5) |
| Reactivate a user | `ibtn_reactivate_*_click` | `auth_service` save | `account_status = active` |

## 5. Empty State

| Part | Content |
|---|---|
| Icon | `mi:group` |
| Heading | `users.empty.title` |
| Body text | `users.empty.body` |
| Primary action | `users.invite` (Invite User) |

## 6. Access

| Role | Scope |
|---|---|
| Owner | all |
| Manager | none |
| Admin | none |
| Staff | none |
| Customer | none |
| SignedOut | none |

Owner only (access matrix `settings.UserManagementForm`).

## 7. Text

| Phrase key | English text |
|---|---|
| `users.title` | Users and Permissions |
| `users.filter.role` | Role |
| `users.search` | Search people |
| `users.invite` | Invite User |
| `users.invite.title` | Invite a team member |
| `users.invite.name` | Name |
| `users.invite.email` | Email |
| `users.invite.role` | Role |
| `users.invite.provider_services` | Services they deliver |
| `users.invite.address` | Their email address |
| `users.invite.desks` | Desks they belong to |
| `users.invite.send` | Send invitation |
| `users.row.edit` | Edit |
| `users.row.deactivate` | Deactivate |
| `users.row.reactivate` | Reactivate |
| `users.role.owner` | Owner |
| `users.role.manager` | Manager |
| `users.role.admin` | Admin |
| `users.role.staff` | Staff |
| `users.role.customer` | Customer |
| `users.status.active` | Active |
| `users.status.inactive` | Inactive |
| `users.status.pending` | Pending |
| `users.empty.title` | No users match |
| `users.empty.body` | Try a different search, or invite a team member. |

## 8. Help Videos

| Process | Help video |
|---|---|
| Inviting a team member | `master_template` help entry `settings.users` — played in the app language |
| Roles and what they see | `master_template` help entry `settings.roles` |

## 9. User Flows

| Flow | Part this form plays |
|---|---|
| UF-03 Staff invitation and sign-in | The whole flow: invite, role, provider services and schedule, address and desks; role changes and deactivation |
| UF-44 Trying the demo apps | In demo mode this form is read-only (architecture reference F-36) |
