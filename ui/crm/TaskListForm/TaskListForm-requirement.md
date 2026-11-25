---
title: "TaskListForm-requirement"
status: draft
doc-id: "mb-cc11bf5b"
updated: "2026-11-25"
---
# TaskListForm — Screen Requirement

## 1. Form

| Item | Value |
|---|---|
| Module path | `crm.TaskListForm` |
| Purpose | Tasks by due date, filtered by owner and status; complete with a timestamp (ticket 32, F-20; UF-26) |
| Screen type and width | List — 1200 px, full width: header, filters, data grid |
| Layout | `layouts.AdminLayout` |
| Route | None — opened from the admin navigation (Customers & Marketing → Tasks) |

## 2. Elements

| Element | Theme component | Why this component | Role | `wrap_on` | Tooltip phrase key |
|---|---|---|---|---|---|
| Page title | `m3.Heading` (`hdg_tasks`) | "Tasks", `headline-large` | `page-title` | — | `tasks.title` |
| Add Task button | `m3.Button` (`btn_add_task`, filled) | Adds a task linked to a contact (UF-26 step 2; DESIGN.md empty states: "+ Add Task") | `primary-button` | — | `tasks.add` |
| Owner filter | `m3.DropdownMenu` (`dd_task_owner`) | Filtered by owner (UF-26 step 1) | `outlined-select` | — | `tasks.filter.owner` |
| Status filter | `m3.ButtonGroup` (`bgrp_task_status`, connected, single: Open/Done/All) | Filtered by status | — | mobile | `tasks.filter.open`, `tasks.filter.done`, `tasks.filter.all` |
| Tasks grid | Anvil `DataGrid` (`dg_tasks`) | Title, contact, due date, assigned to, source, completed | — | mobile | — |
| Task row | Anvil `DataRowPanel` (`drp_task_*`) | One task per row, with its complete checkbox | — | — | — |
| Complete checkbox | `m3.Checkbox` (`cb_task_done_*`) | Completes the task with a timestamp (UF-26 step 3) | — | — | — |
| Auto-generated marker | `m3.Badge` (no count) | Marks a task created automatically (`tasks.auto_generated`) | — | — | — | — | — |
| Help icon | `components.HelpTooltipComponent` (help entry `crm.tasks`) | The feature's tooltip and help video | — | — | `help.crm.tasks.tooltip` |

## 3. Data

| Element | Table | Column |
|---|---|---|
| Grid | `tasks` | `task_title`, `contact_id`, `assigned_to`, `task_type`, `source`, `due_date`, `completed`, `completed_date`, `notes`, `auto_generated` |

## 4. Actions

| Action | Event handler | Server function | Result |
|---|---|---|---|
| Load the list | `form_show` → `load_tasks` | `task_service` list read (`server_automation/task_service`) | The grid arrives (Staff see and complete their own tasks) |
| Filter | `dd_task_owner_change`, `bgrp_task_status_change` | the same read | The grid reloads |
| Complete a task | `cb_task_done_*_change` | `task_service` save | The completion is timestamped |
| Add a task | `btn_add_task_click` | `task_service` save | The task is created with title, due date, notes and contact |

## 5. Empty State

| Part | Content |
|---|---|
| Icon | Task icon (`mi:checklist`) |
| Heading | `tasks.empty.title` (DESIGN.md: "No tasks yet") |
| Body text | `tasks.empty.body` (DESIGN.md: "Tasks are created automatically from bookings and campaigns, or you can add them manually.") |
| Primary action | `tasks.add` (+ Add Task) |

## 6. Access

| Role | Scope |
|---|---|
| Owner | all |
| Manager | all |
| Admin | all |
| Staff | all |
| Customer | none |
| SignedOut | none |

Staff own only; writable fields per `writable_fields.tasks` (Staff: completed, notes).

## 7. Text

| Phrase key | English text |
|---|---|
| `tasks.title` | Tasks |
| `tasks.add` | + Add Task |
| `tasks.filter.owner` | Owner |
| `tasks.filter.open` | Open |
| `tasks.filter.done` | Done |
| `tasks.filter.all` | All |
| `tasks.source.manual` | Manual |
| `tasks.source.automation` | Automatic |
| `tasks.source.voice` | Voice |
| `tasks.source.system` | System |
| `tasks.empty.title` | No tasks yet |
| `tasks.empty.body` | Tasks are created automatically from bookings and campaigns, or you can add them manually. |

## 8. Help Videos

| Process | Help video |
|---|---|
| Working through your tasks | `master_template` help entry `crm.tasks` — played in the app language |

## 9. User Flows

| Flow | Part this form plays |
|---|---|
| UF-26 Tasks and automation rules | The tasks half of the flow |
| UF-19 Contact form submission | The follow-up task created by the contact form appears here |
| UF-27 Reviews | The automatic review task appears here |
| UF-31 WhatsUp voice | Voice-created tasks arrive here |
