---
title: "32 — F-20 Tasks and automation"
status: open
labels: [ready-for-agent]
created: 2026-11-17
doc-id: "mb-cc5f28d9"
updated: "2026-11-17"
---

## Description

**Feature:** F-20 Tasks and automation (UF-26; SR-36; FR-20).
**Depends on:** 20 (contacts), 29 (the send-email action), 23 (the invoice-overdue trigger).

### Forms

- `crm.TaskListForm` — tasks by due date, filtered by owner and status; complete with timestamp
- `crm.AutomationListForm`, `crm.AutomationEditorForm` — trigger event, conditions (service, segment, tag), actions (create task, send email, enrol in a sequence, add a tag), switch on

### Server functions

- `server_automation/task_service` (moved from `server_marketing/` at this step, per the divergence inventory)
- `server_automation/automation_service` — the trigger evaluation as a pure module beside it; one run per rule and trigger reference
- `server_automation/automation_tasks` — the background task that evaluates automations and records each run (`automation_runs`: success, skipped, failed)

### Tables

- `tasks`, `automation_rules`, `automation_runs`, `contacts`

### Acceptance tests

- `SR-36`; UF-26; `IN-02` (Staff see and complete their own tasks; `writable_fields.tasks`)
- The automation run's one-per-rule-and-trigger invariant

### Security criteria

- B1, AC-S2 — row scoping; the automation engine runs server-side only
- AC-S12 — automation runs recorded; no personal data beyond ids in the run records

## Comments
