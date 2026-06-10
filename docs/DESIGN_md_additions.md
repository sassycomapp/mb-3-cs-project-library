## MUI Aesthetic Implementation

This section records the MUI aesthetic decisions applied to the Mybizz CS design system.
These decisions are authoritative. The four supporting artifact files listed below are the
implementation deliverables. OpenCode and GStack `/design-consultation` must apply them
exactly as specified — no deviation, no regeneration unless source inputs change.

### Decision Summary

The MUI dashboard (https://mui.com/material-ui/getting-started/templates/dashboard/)
was selected as the aesthetic reference for Mybizz CS.

The following MUI aesthetic variables have been adopted and applied:

| Variable | MUI Value Adopted |
|---|---|
| Surface distinction | Two-level: background vs surface-container-low for panels/nav |
| Card shadow | Soft dual-layer: `hsla(220,30%,5%,0.07) 0px 4px 16px 0px, hsla(220,25%,10%,0.07) 0px 8px 16px -5px` |
| Card border radius | 8px (tightened from M3 default 12px) |
| Typography heading weight | 600 (updated from 400 on headline role) |
| Dividers | outline-variant at 40% opacity — translucent hairlines |
| State hover opacity | 0.08 |
| State pressed/focused opacity | 0.12 |
| App bar | surface token, hairline bottom border, no gradient at rest |
| Left nav | surface-container-low background, hairline right border |

All MUI values are applied through CSS custom properties and Anvil role selectors.
No hardcoded hex values were introduced.
All color values resolve through the palette selector token system.

### Constraints Preserved

The following DESIGN.md rules were not altered by the MUI aesthetic patch:

- Button radius: 100px (pill) — unchanged
- Dialog radius: 28px — unchanged
- Input radius (outlined): 4px — unchanged
- Navigation item radius: 100px (pill) — unchanged
- Data grid radius: 24px — unchanged
- Font family: Roboto — unchanged
- Spacing rhythm: 4px base grid — unchanged
- M3 color role structure — unchanged

---

## MUI Aesthetic Artifact Files

These four files are located in `project-library\docs\` and are the authoritative
implementation artifacts for the MUI aesthetic and palette system.
OpenCode must load and apply these files when executing `/design-consultation` design tasks.

| File | Location | Purpose | Status |
|---|---|---|---|
| `DESIGN_palette_section.md` | `project-library\docs\` | Full M3 CSS token sets for all 4 palettes (32 tokens each, light mode). Replaces the Palette System section of this file. | Authoritative |
| `palette_selector_tokens.md` | `project-library\docs\` | Python dict blocks for the palette selector component. Contains all 32 tokens per palette plus `apply_palette` usage pattern. | Authoritative |
| `theme_mui_patch.css` | `project-library\docs\` | Standalone MUI aesthetic patch block. Documents exactly what was changed in theme.css and why. Reference only — do not apply separately. | Reference |
| `theme_patched.css` | `project-library\docs\` | Complete theme.css with MUI patch appended. This is the deployment-ready theme file. Deploy as `theme.css` in Anvil. | Deploy |

### Source Colors (Locked)

| Palette | Source Hex | Algorithm |
|---|---|---|
| Professional Blue | `#2b2ee0` | M3 HCT TonalSpot |
| Warm Coral | `#fa7268` | M3 HCT TonalSpot |
| Calm Teal | `#008080` | M3 HCT TonalSpot |
| Sophisticated Purple | `#a30dd1` | M3 HCT TonalSpot |

If a source color changes, `DESIGN_palette_section.md` and `palette_selector_tokens.md`
must be regenerated together and the source color table above updated.
If the MUI aesthetic patch changes, `theme_mui_patch.css` and `theme_patched.css`
must be regenerated together.

---

## Source Reference Mapping

This section records how the source files map into this consolidated design authority.

| Source file | Primary contribution to this document |
|---|---|
| `design-direction.md` | Core visual direction, tokens, spacing, typography, elevations, role map |
| `design-consultation-plan-workflows.md` | Workflow details, custom components, schema additions, implementation notes |
| `design-consultation-plan-descriprive.md` | Workstreams, palette model, blog template strategy, layout architecture |
| `DESIGN_palette_section.md` | Full M3 token sets for all 4 palettes — palette selector implementation |
| `palette_selector_tokens.md` | Python implementation of palette selector token swap |
| `theme_mui_patch.css` | MUI aesthetic patch — reference for what changed in theme.css |
| `theme_patched.css` | Deployment-ready theme.css with MUI aesthetic applied |

---

## Final Instruction for GStack and OpenCode

Treat `DESIGN.md` as the authoritative design system document for Mybizz CS.
If a downstream command requires the design consultation output, use this file first.
Use the supporting design consultation documents only for deeper detail where needed.

### `/design-consultation` Workflow Instructions

When executing `/design-consultation` or any downstream design task:

1. Load `DESIGN.md` as the primary design authority.
2. Load `project-library\docs\DESIGN_palette_section.md` for the full palette token sets.
3. Load `project-library\docs\palette_selector_tokens.md` for the palette selector implementation.
4. Apply `project-library\docs\theme_patched.css` as the active `theme.css` — do not regenerate or modify the MUI aesthetic patch section without explicit instruction.
5. Reference `project-library\docs\theme_mui_patch.css` to understand what the patch changed and why.
6. Do not deviate from the MUI aesthetic variables recorded in the MUI Aesthetic Implementation section above.
7. Do not introduce hardcoded hex values. All colors must resolve through palette selector tokens or existing Anvil `%color:Token%` placeholders.
8. Do not create a parallel CSS architecture. All styling flows through `theme.css` and Anvil component roles.
