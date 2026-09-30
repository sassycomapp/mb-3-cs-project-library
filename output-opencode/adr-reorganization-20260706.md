---
title: "adr-reorganization-20260706"
doc-id: "mb-3d2fd248"
---
# ADR Reorganization — 2026-07-06

## Task
Split ADR documents into global vs local, drop numeric prefixes, delete superseded.

## Actions

1. **Classified all 44 ADR files** through document inspection and developer decisions.
2. **Deleted 4 superseded/cancelled ADRs:**
   - `9-multi-vertical-to-single-vertical-conversion` — historical, no longer relevant
   - `12-consolidated-build-sequence` — superseded
   - `13-system-currency-setting` — superseded by ADR-16
   - `25-cancelled-onboarding-finality` — superseded by ADR-26 (fully reversed)
3. **Renamed 40 surviving ADRs** — dropped all numeric prefixes, preserved descriptive slugs exactly.
4. **Moved to `adr/adr-global/`** — 39 ADRs (all company-wide architecture, technology, standards, and process decisions).
5. **Moved to `adr/adr-local/`** — 1 ADR (`schema-scope-reduction`).

## Result

| Folder | Count |
|---|---|
| `adr/adr-global/` | 39 |
| `adr/adr-local/` | 1 |
| `adr/adr-index.md` | 1 (unchanged) |

## Known follow-up (phase 2 — document content editing)

- `adr-index.md` references ADRs by old numeric prefix — needs rewrite to name-based references.
- All documents outside `adr/` that reference ADRs by number will need updating.
- `docs/authoritative-schema.md` references `25-onboarding-finality.md` — needs updating to reference `adr-global/onboarding-finality.md`.

## Next steps
- Continue points 5/6/7 of the PDLF roadmap (docs organization, project structure, template) — Build
- ADR content updates reserved for phase 2 (document-content editing phase)
