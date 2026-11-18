---
title: "mb-3-cs-c20-app1-receipt"
doc-id: "mb-39375a03"
updated: "2026-11-18"
---
# Step Receipt — mb-3-cs — Step 20 — app1

Phase: C   |   Type: skill   |   Status: complete
Started: 2026-11-17 15:09:12.909885+00   |   Completed: 2026-11-17 15:58:08.401863+00

## What was invoked

Skill family: gstack
Skill: design-consultation
Inputs used: {"inputs": ["Step 19: the approved ticket list, in build order"]}

## Output

Artifact: [[[mb-3-cs-project-library:docs-local/design]]](C:/dev/dev-mb-3-cs/mb-3-cs-project-library/docs-local/DESIGN.md)
Step-folder copy: [[[mb-3-cs-project-library:pdlf-outputs/c20/mb-3-cs-c20-app1-design]]](C:/dev/dev-mb-3-cs/mb-3-cs-project-library/pdlf-outputs/c20/mb-3-cs-c20-app1-design.md)

## What was applied

- `/design-consultation` was started and held to the fixed answers of `c20-config.md`: `DESIGN.md` stays at `docs-local/DESIGN.md` in the project library; colours, fonts, sizes and spacing are never defined by this document; no preview page was written. Existing `DESIGN.md` took the skill's Update path; every decision of David's that met the rules was carried.
- David's rulings applied as the document's law: nothing departs from `m3-custom-theme`; every value and every recorded departure removed; choices named only with the theme's tokens; Roboto stays as Material 3's standard typeface; Anvil canonical built-in components are not departures and are not recorded as such.
- Choices recorded: starting palette `professional_blue` (Mybizz's corporate colour), starting mode `system`, user choice yes (palette per client, dark mode always follows the device), menu style `baseline` (the theme's default — flagged for David's approval), logo and imagery: `assets-local/` holds no brand files yet, tone of text from brand-profile §10.
- Removed: the type scale, spacing scale, radius, elevation and density value tables, the card-shadow `color-mix()` value, the divider opacity, the state-layer percentages, all pixel lengths (sidebar, max-widths, badge heights), and the entire "Project Deviations from M3 Defaults" section (nine recorded departures).
- Kept: every decision, status lifecycle, permitted-action table, empty state and flow decision — choices, not appearance facts.

## Canonical files read and applied

| File | Applied |
|---|---|
| `project-library-global/specifications-global/spec-ui-standards.md` | Fixed statements (theme-only, full-inventory choice, banned components); screen widths left to its §7; states, buttons, empty states, accessibility |
| `project-library-global/adr-global/adr-dark-mode.md` | Starting mode `system`; light is the design baseline |
| `mb-3-cs-project-library/docs-local/design/app1/design-doc.md` | The reviewed plan; the run's constraints |
| `mb-3-cs-project-library/docs-local/DESIGN.md` (previous) | Every decision carried; values and departures removed |
| `mb-3-cs-project-library/docs-local/GLOSSARY.md` | Term discipline (client, customer, user, roles) |
| `mb-3-cs-project-library/docs-local/brand-profile.md` | Corporate colour, voice, personality, accessibility promises |
| `mb-3-cs-project-library/pdlf-outputs/c19/mb-3-cs-c19-app1-ticket-list.md` | Ticket context (52 tickets; 19-f31-design-system consumes this document) |
| `mb-3-cs-project-library/assets-local/README.md` | No brand asset files exist yet — recorded as such |
| `m3-custom-theme-project-library/docs/usage-guide.md` | Palettes, mode, menu style, motion scheme, window classes, component set, `--m3x-*` tokens |
| `m3-custom-theme-project-library/docs/base-inventory.md` | Component facts (theme + layouts) |
| `m3-custom-theme-project-library/m3-custom-theme.md` | WCAG AA palette build guarantee (line 41) |
| `m3-custom-theme-project-library/tools/build_palettes.py` | The contrast check itself (4.5:1 text, 3:1 outlines, every palette, light and dark) |
| `pdlf:spec-execution-discipline`, `spec-bft`, receipt template | Step conduct |

GBrain search fired at substep 1 (recorded in the ledger row): `design system m3-custom-theme palette Anvil mb-3-cs`. Results matched the canonical files above; where a result differed from a file, the file was followed.

## Tests

Table `ts_report.py pdlf-outputs/c20 --prefix mb-3-cs-c20-app1-` prints, unchanged:

| File | Passed | Failed | Review | Skipped | Errors |
|---|---|---|---|---|---|
| mb-3-cs-c20-app1-checks.xml | 2 | 0 | 0 | 0 | 0 |
| **Total** | **2** | **0** | **0** | **0** | **0** |

- **PR-01** — passed. Entry record complete; Step 19 complete for run `app1` (not synthesized, not skipped — no input notices arise).
- **DC-12** — passed. Contrast: `professional_blue` reaches WCAG 2.2 AA in light and dark by the theme's own palette build (`tools/build_palettes.py` stops on any failure; 4.5:1 text, 3:1 outlines). Excluded components named in the document's law: `Label`, `HtmlTemplate`, `HtmlPanel`, Anvil Extras, anything outside the theme and `approved_components`. Value scan of the document on 2026-11-17: zero matches for hex/rgb/rgba/hsl colour values, zero `px` lengths, zero font sizes, zero type weights, zero named spacing values.

## BFT checkpoint

Level applied: full
Bones:   Three — the law (theme-only, choices-never-values, built-ins-not-departures), the choices table, and the component/role map that restates every look as a role name. Key bone: the law — remove it and the structure collapses into a values document again.
Muscle:  Channel Muscle dominant. Applied by name: spec-ui-standards (§§2, 3, 7, 8, 10, 13) and adr-dark-mode from `project-library-global`; the design-doc, GLOSSARY, brand-profile, ticket list, existing DESIGN.md, and the theme's own docs (`usage-guide`, `base-inventory`, `m3-custom-theme.md` line 41, `tools/build_palettes.py`). Logic: DC-12 (no values in the document) and spec-ui-standards (no values in templates) force the same conclusion — every appearance fact named as a `--m3x-*` token, colour role or type role.
Nerves:  Backward chain from the output to actions complete: verify Step 19 → read inputs → run the skill held to the config answers → apply the ruling → scan for values → evidence contrast → copy → BFT → ledger → receipt. Decisions decomposed into values (cut) and choices (kept); never mixed.
Fat:     Cut the value tables, the `color-mix()` shadow, the hairline opacity, state-layer percentages, all pixel lengths and the nine recorded departures. Kept every decision, lifecycle, permitted-action table, empty state and flow decision. Nothing kept that still met the law, and nothing cut that was a choice.
Skin:    Law-first: the ruling is conclusion-first in the opening section; the law governs everything after it. Every look is named as a role; the theme repository is named as the single source of values.
Health:  Good in itself — `baseline` menu style is flagged for approval rather than invented; `assets-local` recorded as empty rather than imagined. Good for the project — unblocks Step 21 and every Phase E ticket. Downstream consequence watched: ticket `19-f31-design-system` starts theming with `set_menu_style` and `set_scheme` calls fixed here as `baseline` and `standard`; if either choice changes at approval, that startup call is the only code that changes.

## Gate status

Step 20 has no gate of its own; the run's next gate is `concept-locked` at Step 21.

## Deviation

None recorded.

## Ledger row written

steps:       (id 13) step_number=20 | sort_order=20 | phase=C | run=app1 | app_number=1 | step_type=skill | skill_family=gstack | skill_name=design-consultation | inputs_used={"inputs": ["Step 19: the approved ticket list, in build order"]} | artifact_path=[[[mb-3-cs-project-library:docs-local/design]]](C:/dev/dev-mb-3-cs/mb-3-cs-project-library/docs-local/DESIGN.md) | synthesized=f | status=complete | next_action=Step 21 - concept lock | started_at=2026-11-17 15:09:12+00 | completed_at=2026-11-17 15:58:08+00 | gbrain_query=design system m3-custom-theme palette Anvil mb-3-cs
gates:       none
bft_records: (id 9) project_slug=mb-3-cs | step_id=13 | level=full
deviations:  none

## Close-out

Please run `/mb-close-out` when you are ready.

## Blockers

None

## Notes for next session

Menu style `baseline` and motion scheme `standard` are recorded in the choices table and flagged for David's approval with this document; they are carried into ticket `19-f31-design-system`'s startup calls.

## Next action

Step 21 — concept lock, in a new session started with `/pdlf-init`.

---
Type "continue" to proceed to the next step, or raise anything you want changed first.
