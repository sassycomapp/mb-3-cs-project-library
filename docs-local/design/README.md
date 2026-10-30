---
title: "README"
doc-id: "mb-a9168a12"
updated: "2026-10-30"
---
# design

One folder per run: `app1`, `app5`, or a feature's short name. A run's folder is created by Step 12 and holds:

| File | What it is | Written by |
|---|---|---|
| `design-doc-first-concept.md` | The concept as first agreed. Never changed afterwards. | Step 12 |
| `design-doc.md` | The working design doc, amended by the reviews, GStack's report last. | Steps 12, 14, 15, 16 |
| `test-plan.md` | Every critical flow with its tests. | Step 15 |
| `access-matrix.yaml` | Every server function, form and settings tab against every role. In `app{n}\` only. | Step 15 |
| `threat-model.md` | Every trust boundary with its threats and controls. In `app{n}\` only. | Step 15 |

No file here is stubbed: Step 12 never overwrites an existing first concept.
