# GStack Handoff Contract

## Purpose
This document defines the minimum required handoff structure between Plan and Build workflows in the Mybizz CS workspace.

The goal is to make transitions explicit, reviewable, reusable, and aligned with devlog/todo/ADR discipline.

## Rule
A workflow that changes ownership from Plan to Build, from Build back to Plan, or from one repo context to another must produce a structured handoff.

Handoffs must respect:

- OpenCode permissions and protected-file rules,
- canonical devlog/todo ownership,
- ADR authority over architecture,
- GBrain’s advisory-only role.

---

## Plan to Build handoff

A Plan to Build handoff must contain:

1. **Task summary**
   - one short statement of the approved goal

2. **Scope**
   - what is in scope
   - what is out of scope

3. **Source context**
   - files read
   - key constraints
   - relevant ADRs and document references

4. **Target change set**
   - files expected to change
   - files that must not change (especially protected files)

5. **Risk notes**
   - protected files
   - ambiguous areas
   - validation concerns

6. **Build instructions**
   - ordered implementation steps
   - coding constraints
   - style or pattern constraints

7. **Validation expectation**
   - what Build must verify before closing
   - which tests/checks are expected

8. **Tracking expectation**
   - whether canonical devlog update is likely required
   - whether canonical todo update is likely required
   - whether new or updated ADRs are expected

### Required Plan output template

```text
Task summary:
Scope in:
Scope out:
Files read:
Relevant ADRs:
Files expected to change:
Files that must not change:
Key constraints:
Implementation steps:
Validation required:
Tracking handoff required (devlog / todo / ADR):
```

---

## Build to Plan handoff

A Build to Plan handoff must contain:

1. **Completion status**
   - complete / partial / blocked

2. **Files changed**
   - exact files changed

3. **Backup confirmation**
   - confirm backups were created where required

4. **Protection confirmation**
   - confirm protected-file rules were respected

5. **Validation result**
   - passed / partial / failed / not run
   - short explanation

6. **Key implementation summary**
   - short summary of what changed and why

7. **Proposed devlog handoff**
   - type (feature, fix, refactor, investigation, etc.)
   - suggested slug/title
   - one-line brief
   - files changed
   - validation outcome
   - pointer to relevant ADRs / decisions

8. **Proposed todo handoff**
   - only if new valid out-of-scope work was discovered
   - each item should be suitable to drop directly into `todo.md`

9. **Proposed ADR updates**
   - only if architectural decisions changed
   - whether a new ADR is needed or an existing one should be updated

### Required Build output template

```text
Completion status:
Files changed:
Backup confirmation:
Protected-file confirmation:
Validation result:
Implementation summary:
Proposed devlog handoff:
Proposed todo handoff:
Proposed ADR updates:
```

---

## Status handoff

For session close or cross-session continuation, include:

- current objective,
- completed work (and whether devlog has been updated),
- unresolved items (and whether they are in `todo.md`),
- ADRs touched or needing updates,
- next recommended step,
- whether canonical tracking still needs updating.

---

## Devlog, todo, and ADR discipline

These rules always apply:

- **Devlog**
  - Every completed task should be reflected in the devlog via Plan or you.
  - Handoffs must make it easy to create a concise, well-structured devlog entry.

- **Todo**
  - Any out-of-scope but useful work discovered must be recorded in `todo.md`.
  - Completed todo items are marked `[DONE]` and moved to `todo-archive.md`.

- **ADR**
  - At the start of any substantial task, relevant ADRs must be consulted.
  - New architectural decisions must result in new ADR files or updates to existing ADRs.

Handoffs are the mechanism that ensures this discipline is practical.

---

## Minimum quality bar
A handoff is acceptable only if:

- it is short and specific,
- file paths are explicit,
- scope boundaries are clear,
- relevant ADRs and tracking expectations are visible,
- unresolved risks and out-of-scope items are clearly listed,
- it is sufficient for a new session to continue without reconstructing the entire context from scratch.