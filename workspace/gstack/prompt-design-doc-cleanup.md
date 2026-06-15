# Prompt: Design Documentation Cleanup and Rationalization

## Prompt ID
`design-doc-cleanup-v1`

## Purpose
Rationalize and clean up existing design documentation for `project-library`. Produce a clear, minimal, and current design documentation set. Redundant, superseded, or obsolete files must be moved to `/mnt/c/_mb2-cs-app/project-library/Obsolete`.

## Agent
`plan` (primary)

## Workflow
`docs-plan`

## Workspace
`/mnt/c/_mb2-cs-app/project-library`

---

## Prompt Text

### Objective

Rationalize and clean up existing design documentation for `project-library`. Produce a clear, minimal, and current design documentation set. Any redundant, superseded, or obsolete files must be moved to:

`/mnt/c/_mb2-cs-app/project-library/Obsolete`

Wireframes and screen documents are currently non-compliant and must not be treated as acceptable examples. Do not create new wireframes or screens in this task; only classify and document their status.

### Scope

- Repository root:
  `/mnt/c/_mb2-cs-app/project-library`

- In-scope folders:
  - `/mnt/c/_mb2-cs-app/project-library/gstack-outputs`
  - `/mnt/c/_mb2-cs-app/project-library/docs`

- Primary design direction:
  - Material UI (MUI) and Material Design 3 decisions already captured in the documentation are the authoritative direction for the system.

### High-priority files

The following files must be explicitly inspected and classified. None of them may be skipped.

From `gstack-outputs`:

- `/mnt/c/_mb2-cs-app/project-library/gstack-outputs/plan-tune-verification-report.md`
- `/mnt/c/_mb2-cs-app/project-library/gstack-outputs/wireframe-reconciliation-audit.md`
- `/mnt/c/_mb2-cs-app/project-library/gstack-outputs/wireframe-screen-coverage-report.md`
- `/mnt/c/_mb2-cs-app/project-library/gstack-outputs/DESIGN.md`
- `/mnt/c/_mb2-cs-app/project-library/gstack-outputs/design-consultation-plan-descriprive.md`
- `/mnt/c/_mb2-cs-app/project-library/gstack-outputs/design-consultation-plan-workflows.md`
- `/mnt/c/_mb2-cs-app/project-library/gstack-outputs/design-consultation-screen-standard-report.md`
- `/mnt/c/_mb2-cs-app/project-library/gstack-outputs/design-html-report.md`
- `/mnt/c/_mb2-cs-app/project-library/gstack-outputs/design-review-report.md`

From `docs`:

- `/mnt/c/_mb2-cs-app/project-library/docs/ui-standards.md`
- `/mnt/c/_mb2-cs-app/project-library/docs/custom-component-requirements-matrix.md`
- `/mnt/c/_mb2-cs-app/project-library/docs/DESIGN_md_additions.md`
- `/mnt/c/_mb2-cs-app/project-library/docs/DESIGN_palette_section.md`
- `/mnt/c/_mb2-cs-app/project-library/docs/design-direction.md`
- `/mnt/c/_mb2-cs-app/project-library/docs/m3_component_mapping.md`
- `/mnt/c/_mb2-cs-app/project-library/docs/palette_selector_tokens.md`
- `/mnt/c/_mb2-cs-app/project-library/docs/platform-overview.md`
- `/mnt/c/_mb2-cs-app/project-library/docs/screen-production-standard.md`
- `/mnt/c/_mb2-cs-app/project-library/docs/spec-component-properties.md`
- `/mnt/c/_mb2-cs-app/project-library/docs/spec-material-3-theme.md`
- `/mnt/c/_mb2-cs-app/project-library/docs/theme_mui_patch.css`
- `/mnt/c/_mb2-cs-app/project-library/docs/theme_patched.css`

### Constraints

- Treat current MUI / Material Design 3 work as the design baseline.
- Existing wireframes and screen documents are non-compliant.
  - Do not use them as design authority or positive examples.
  - Only reference them for understanding current state and for classification.
- No new visual artifacts (wireframes, hi-fi screens) are to be created in this task.

### Required outputs

1. **Design documentation information architecture**

   - Define the target structure for design documentation under `docs` and, where necessary, `gstack-outputs`.
   - Identify a minimal set of canonical documents (e.g. design overview, UI standards, theme specification, component specification, screen production standard).
   - Specify the intended role and scope of each canonical document.
   - Specify where MUI/M3 decisions live and how other documents should reference them.

2. **File classification for high-priority files**

   For each file listed in "High-priority files", provide:

   - Current role and main content.
   - Classification, chosen from:
     - `CANONICAL` -- keep as-is or with focused updates; part of the target architecture.
     - `MERGE_SOURCE` -- content to be merged into another document; source can then be moved to `Obsolete`.
     - `OBSOLETE` -- redundant, superseded, or no longer relevant; move to `/mnt/c/_mb2-cs-app/project-library/Obsolete`.
   - If `MERGE_SOURCE`, specify:
     - Target canonical document file path.
     - Concrete merge instructions (which sections, what to extract, what to drop).
   - If `CANONICAL`, specify:
     - Required edits (if any) to align with MUI/M3 baseline and with the rest of the documentation set.

3. **Obsolete and cleanup actions**

   - List all files (beyond the high-priority list, if applicable) that should be moved to `/mnt/c/_mb2-cs-app/project-library/Obsolete`, with a short reason per file.
   - Provide a stepwise cleanup plan that can be executed in the repo, including:
     - Moves/renames.
     - Merge operations.
     - Deletion of clearly invalid fragments where appropriate.

4. **Wireframe and screen status**

   - Identify any documents that contain wireframes or screen-level specifications.
   - Mark their status explicitly as `NON_COMPLIANT_REFERENCE_ONLY`.
   - Provide criteria for future compliant wireframes/screens aligned with the MUI/M3 baseline and the canonical documentation set.

5. **Alignment with MUI / Material Design 3**

   - Summarize the key MUI and Material Design 3 decisions already present in the docs (themes, tokens, component mapping, etc.).
   - Confirm how these decisions are reflected in:
     - `ui-standards.md`
     - `spec-material-3-theme.md`
     - `m3_component_mapping.md`
     - CSS theme patch files
   - Identify gaps or contradictions and specify which canonical document(s) should be updated to resolve them.

### Format of the response

- Provide the response as structured markdown with clear headings:
  - `Information Architecture`
  - `File Classification`
  - `Obsolete and Cleanup Actions`
  - `Wireframes and Screens`
  - `MUI / Material 3 Alignment`
- Use tables where helpful for file classification and cleanup actions.
- Reference files by their full Linux path as shown above.

---

## Execution notes for agents

### Pre-work
1. Read the `project-library/AGENTS.md` for workspace rules.
2. Read `project-library/gstack-artifact-governance.md` for file placement rules.
3. Read `project-library/docs/design-direction.md` for the current MUI/M3 baseline.
4. Read `project-library/docs/ui-standards.md` for the current UI standards.
5. Read `project-library/docs/spec-material-3-theme.md` for the current theme spec.

### File existence check
Before classifying a high-priority file, confirm it exists at the stated path. If a file does not exist:
- Note it as `MISSING` in the classification table.
- Do not create it.
- Check if it was previously renamed or moved (look for similar filenames in the same directory or in `Obsolete/`).

### Classification rules
- `CANONICAL`: The file is part of the target documentation set. It may need edits but its core content is current and relevant.
- `MERGE_SOURCE`: The file contains content that should be absorbed into a canonical document. After extraction, the source file moves to `Obsolete/`.
- `OBSOLETE`: The file is redundant, superseded, or no longer relevant. Move to `Obsolete/`.

### Merge execution
When a file is classified as `MERGE_SOURCE`:
1. Identify the target canonical document.
2. Extract the relevant sections from the source.
3. Integrate them into the target at the appropriate location.
4. Remove the source file from its current location (move to `Obsolete/`).
5. Note the merge in the cleanup actions output.

### Obsolete execution
When a file is classified as `OBSOLETE`:
1. Move it to `/mnt/c/_mb2-cs-app/project-library/Obsolete/`.
2. Preserve the original filename.
3. Note the reason for obsolescence in the cleanup actions output.

### Output location
Save the analysis report to:
`/mnt/c/_mb2-cs-app/project-library/gstack-outputs/design-doc-cleanup-report.md`

### Handoff
After completing the analysis:
1. Produce a structured handoff (per `gstack-handoff-contract.md`) if Build agent execution is needed.
2. The handoff should list each file operation (move, merge, edit) with exact source and target paths.
3. Include a validation checklist for Build to confirm after execution.
