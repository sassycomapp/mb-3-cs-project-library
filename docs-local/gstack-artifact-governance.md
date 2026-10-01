---
title: "gstack-artifact-governance"
doc-id: "mb-2b5d047d"
updated: "2026-10-01"
---
# GStack Artifact Governance

## Purpose

This document defines where GStack and OpenCode outputs belong in the `project-library` repository. Its goal is to keep planning outputs, architectural decisions, application documentation, and wireframes separated so they are easy to find and easy for agents to use correctly.

## Folder Roles

### `project-library/gstack-outputs/`

Use this folder for GStack-generated working outputs:

- office-hours reports.
- CEO review reports.
- ENG review reports.
- planning outputs.
- task lists.
- handoff notes.
- other temporary or session-based artifacts produced by GStack.

These files are useful records of agent work, but they are not the canonical app documentation.

### `project-library/docs/`

Use this folder for application-specific documentation:

- design documents.
- specifications.
- UI standards.
- architecture summaries.
- component mappings.
- policy documents.
- feature documentation.

This is the home for durable documentation that describes the actual application and its intended behavior.

### `project-library/adr/`

Use this folder for Architecture Decision Records:

- one decision per file.
- clear context.
- the decision taken.
- rationale.
- consequences.
- scope and constraints.

ADRs explain why the system is built the way it is and are part of the repository’s essential governance layer.

### `project-library/wireframes/`

Use this folder for visual display artifacts:

- wireframes.
- mockups.
- reference screens.
- static UI layout examples.

These files are design references, not policy documents and not session outputs.

## File Placement Rules

- If the file is a GStack session output, place it in `gstack-outputs/`.
- If the file is a durable app document, place it in `docs/`.
- If the file is an architectural decision, place it in `adr/`.
- If the file is a screen reference or mockup, place it in `wireframes/`.

## Special Notes

- [[[mb-3-cs-project-library:docs-local/design|DESIGN]]](C:/dev/dev-mb-3-cs/mb-3-cs-project-library/docs-local/DESIGN.md) belongs in `docs/` because it is an application UI design document, not a general repository artifact.
- Do not place long-lived app specifications in `gstack-outputs/`.
- Do not place transient planning reports in `docs/` unless they have been promoted into durable documentation.
- Do not place ADRs inside `docs/` or `gstack-outputs/`.

## Lifecycle Guidance

- GStack outputs start in `gstack-outputs/`.
- Important decisions or stable guidance may be promoted into `docs/` or `adr/`.
- Wireframes remain in `wireframes/` unless replaced by updated visual references.

## Repository Separation

This governance applies to `project-library` only.  
The `mb-3-cs` repository remains the application implementation repo and should not be used as the storage location for documentation governance artifacts from `project-library`.

## Maintenance Rule

When a new GStack workflow or document type is introduced, decide first which of the four categories it belongs to:

- GStack-generated output.
- Application documentation.
- Architecture decision.
- Visual reference.

Place the file accordingly and keep the category boundaries strict.
