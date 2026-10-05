---
title: "ui-build-audit-report-2026-06-28"
doc-id: "mb-3af4f4ec"
updated: "2026-10-05"
---
# UI Build Audit Report

**Date:** 2026-06-28
**Skill:** /investigate
**Scope:** Comprehensive consistency audit across 17+ files
**Source:** `ui-build-prep/` instructional documents, `adr/`, `docs/`, Anvil.works official documentation

---

## Section 1: Summary

**Total discrepancies found:** 14

**Discrepancies grouped by document:**

| Document | Discrepancies involving it |
|---|---|
| `anvil-ui-build-standard.md` | 10 |
| `anvil-ui-build-prompt-template.md` | 2 |
| `design-direction.md` | 2 |
| `spec-material-3-theme.md` | 2 |
| `adr/37-ui-customization-approach.md` | 1 |
| `nomenclature.md` | 1 |
| `DESIGN.md` | 1 |

---

## Section 2: Detailed Findings

### DISCREPANCY #1 — ADR register reference (deleted file)

**Document A:** `anvil-ui-build-standard.md` §Changelog line 18 says: "All three ADRs referenced in this document now live in `adr-register.md`, not separate files."
**Document B:** `adr-register.md` has been deleted. The ADRs now live in `adr/43-role-property-assignment-mechanism.md`, `adr/44-responsive-behaviour-mechanism.md`, `adr/45-htmltemplate-use.md`.
**Document A also references `adr-register.md` on:** lines 22, 79, 190, 241, 309.

**Document C:** `anvil-ui-build-prompt-template.md` line 25 says: "per ADR-1 in `adr-register.md`"

**Question for user:** Should all references to `adr-register.md` in the Standard and Template be replaced with references to the individual ADR files in `adr/`? If so, should the ADR numbers change from ADR-1/ADR-2/ADR-3 to ADR-43/ADR-44/ADR-45?

#### Developer's Resonse
1. Yes - all references to `adr-register.md` in the Standard and Template must be replaced with references to the individual ADR files in `adr/`
2. Yes - the ADR numbers change from ADR-1/ADR-2/ADR-3 to ADR-43/ADR-44/ADR-45
---

### DISCREPANCY #2 — ADR numbering in Standard changelog

**Document A:** `anvil-ui-build-standard.md` §Changelog (v2.2 → v2.3) line 15 says: "per ADR-2"
**Document A:** line 17 says: "per ADR-3"
**Document A:** §Changelog (v2.1 → v2.2) line 22 says: "ADR-1"

**Document B:** The current ADR files are numbered ADR-43 (role assignment), ADR-44 (responsive behaviour), ADR-45 (HTMLTemplate banned).

**Question for user:** Should the Standard's ADR references be updated from ADR-1/2/3 to ADR-43/44/45? Or were ADR-1/2/3 internal references within `adr-register.md` that should be removed entirely?

#### Developer's Resonse
1. the Standard's ADR references must be updated from ADR-1/2/3 to ADR-43/44/45
---

### DISCREPANCY #3 — DataRowPanel `drp_` prefix status

**Document A:** `anvil-ui-build-standard.md` §4 line 127 says: "`drp_user_row` *(adopted working convention — still needs formal addition to `nomenclature.md`)*"
**Document A:** "Remaining Open Items" table (line 354) says: "`drp_` prefix — Adopted in practice; still needs formal addition to `nomenclature.md` (administrative, non-blocking)"

**Document B:** `nomenclature.md` line 71 says: "`DataRowPanel` | `drp_` | `drp_comment_row`"

**Question for user:** The `drp_` prefix has been formally added to `nomenclature.md`. Should the Standard's "still needs formal addition" note and the Remaining Open Items entry be removed?

#### Developer's Resonse
1. the Standard's "still needs formal addition" note and the Remaining Open Items entry must be removed
---

### DISCREPANCY #4 — Navigation components: ADR-37 vs ADR-45

**Document A:** `adr/37-ui-customization-approach.md` §Consequences line 75 says: "M3 navigation components (NavigationDrawerLayout, NavigationRailLayout) are not used — custom layout approach is locked in"

**Document B:** `adr/45-htmltemplate-use.md` §Decision line 24 says: "Use stock Anvil M3 components and the two native M3 Layouts in all cases."

**Document C:** `anvil-ui-build-standard.md` §7 line 241 says: "use the native `NavigationRailLayout`/`NavigationDrawerLayout` for shell cases"

**Question for user:** ADR-37 says NavigationRailLayout/NavigationDrawerLayout are NOT used. ADR-45 says they ARE used. Which is correct? Has the project decided to use native M3 Layouts, overriding ADR-37's earlier decision?

#### Developer's Resonse
1. ADR-37 says NavigationRailLayout/NavigationDrawerLayout are NOT used Was created at an early discovery stage when it was assumed that these components conflict with the M3 standard . The project policy is Anvil material 3 theme M3 components is the standard throughout the application and their deviation may only occur when there is absolutely no solution within the official Anvil Material 3; M3 components framework. ADR 37 should be marked superseded (likely by ADR 45 ) 
---

### DISCREPANCY #5 — Role assignment in code (spec-material-3-theme.md)

**Document A:** `spec-material-3-theme.md` §3.2 lines 109-113 says: "Roles support multiple assignments. Use space-separated strings for compound patterns: `self.card.role = "surface-card elevated"`"

**Document B:** `adr/43-role-property-assignment-mechanism.md` §Decision says: "Set `role` via the **Designer Properties Panel**, not in code."

**Document C:** `anvil-ui-build-standard.md` §6.0 says: "Set `role` via the **Designer Properties Panel** — not in code"

**Question for user:** `spec-material-3-theme.md` still shows role assignment in code. Should this be updated to reflect ADR-43 (Designer Properties Panel)? Note: the file was partially updated (lines 103-107 reference ADR-43) but the code example on line 112 contradicts it.

#### Developer's Resonse
1. anvil.works docs rule . ADR 43 Is the later information which Was fetched from Anvil.works docs , however you should confirm this independently . after your confirmation , likely spec-material-3-theme.md would need to be updated
---

### DISCREPANCY #6 — Component list differences between Standard and ADR-15

**Document A:** `anvil-ui-build-standard.md` §6 line 184 lists: ColumnPanel, FlowPanel, Card, InteractiveCard, DataRowPanel, Heading, Text, Link, RichText, TextBox, TextArea, DropdownMenu, DatePicker, CheckBox, Switch, RadioButtonGroup, RadioButton, Slider, Button, IconButton, DataGrid, RepeatingPanel, Icon, Image, FileLoader, Plot, LinearProgressIndicator, CircularProgressIndicator (28 components)

**Document B:** `adr/15-material-3-theme-component-scope.md` lines 19-28 lists: m3.Button, m3.IconButton, m3.ToggleIconButton, m3.TextBox, m3.TextArea, m3.DropdownMenu, m3.Card, m3.InteractiveCard, m3.Avatar, m3.Checkbox, m3.RadioButton, m3.Switch, m3.Slider, m3.NavigationLink, m3.NavigationRailLayout, m3.NavigationDrawerLayout, m3.Divider, m3.LinearProgressIndicator, m3.CircularProgressIndicator, m3.ButtonMenu, m3.IconButtonMenu, m3.AvatarMenu, m3.MenuItem, m3.Text, m3.Heading, ColumnPanel, FlowPanel, RepeatingPanel, DataGrid, DataRowPanel (30 components)

**Components in Standard but NOT in ADR-15:** Link, RichText, RadioButtonGroup, Image, Plot
**Components in ADR-15 but NOT in Standard:** ToggleIconButton, Avatar, NavigationLink, NavigationRailLayout, NavigationDrawerLayout, Divider, ButtonMenu, IconButtonMenu, AvatarMenu, MenuItem

**Anvil docs confirm:** All components in both lists are real Anvil components.

**Question for user:** Should the Standard's approved list be updated to match ADR-15? Or should both lists be reconciled into a single comprehensive list?

#### Developer's Resonse
1. The deciding factor here is the Material Three theme - M3 approved components . The reason being when we write CSS we don't be want to be writing CSS for Anvil Native and for Anvil Material three theme . therefore animal material three theme , M3 dependency , components rules . any components that are approved by animal material three theme , M3 dependency components are approved. The use of anything else must be specifically an individually justified and approved separately . all documents should be updated to reflect the same . This is the standard 
---

### DISCREPANCY #7 — `tonal-card` role vs `tonal` appearance

**Document A:** `anvil-ui-build-standard.md` §6 line 186 says: "`tonal` is **not** a valid InteractiveCard appearance"
**Document A:** §6.0 role table line 195 lists `tonal-card` as a valid Card role.

**Document B:** `design-direction.md` §Card roles line 176 says: "`tonal-card` | filled | Background for content groups"

**Anvil docs confirm:** Card appearances are `elevated`, `filled`, `outlined`. There is no `tonal` appearance. InteractiveCard has the same three appearances.

**Question for user:** The Standard says `tonal` is not a valid InteractiveCard appearance (correct per Anvil docs), but lists `tonal-card` as a valid Card role. Is `tonal-card` a CSS role class name (not an Anvil appearance), while the actual Anvil appearance should be `filled`? If so, should the Standard clarify this distinction?

#### Developer's Resonse
1. Anvil.works doc is the standard Anything contradictory is a fail , all project doc should be updated to ensure that Anvil.works Material Three theme standard is the standard . There can be no deviation 

---

### DISCREPANCY #8 — Standard §6.0 known CSS gap

**Document A:** `anvil-ui-build-standard.md` §6.0 line 201 says: "only 16 of these 39 role names have confirmed matching CSS in `theme_patched.css` as of the last audit — several others (alerts, notifications, tags, chips, charts) line up with the project's own deferred Phase 2/3 plan rather than being an oversight, but `input-disabled`/`input-readonly` and the navigation roles don't have an obvious deferred reason and may need CSS added before role assignment on those specific types has any visible effect."

**Document B:** Phase 1 investigation (earlier today) found that `input-disabled`, `nav-item`, `nav-item-selected`, `app-bar-link` are all NATIVE COVERED by existing CSS (structural selectors or attribute selectors). `input-readonly` and `breadcrumb-item` are ASPIRATIONAL (no wireframe uses them yet).

**Question for user:** Should the Standard's §6.0 be updated to reflect that 4 of the 6 investigated roles are native-covered and don't need CSS added? The current wording implies they need CSS before they'll work.

#### Developer's Resonse
1. Anvil.works doc is the standard Anything contradictory is a fail , all project doc should be updated to ensure that Anvil.works Material Three theme standard is the standard . There can be no deviation 
2. the the application project policy is that we go with Anvil.works Anvil Material 3 theme, And will M3 dependency components and their specified properties, provided properties. Therefore discrepancies in this regard should be tested against the annual Material 3 theme documentation .
3. It is not clear to me if there is discrepancy between Anvil Native and Anvil Material 3 theme documentations . This should be a part of your investigation insofar as it affects our work 
---

### DISCREPANCY #9 — Max-width table completeness

**Document A:** `anvil-ui-build-standard.md` §11 lines 317-328 lists 10 form types with max-width values.

**Document B:** `screen-production-standard.md` should have the same table.

**Document C:** `design-direction.md` should have the same Content Width Constraints table.

**Question for user:** Should I verify that all three tables are identical? The Standard's table was updated earlier today. Need to confirm the other two match.

#### Developer's Resonse
1. I'm completely unclear about the situation . I have no idea . It seems to me that Anvil is not particularly clear about this either except that they do have information about "wrap-on" which was covered in one of my documents I may have that "wrap-on" term a little bit wrong. But you can do a grep for "wrap" . Although this might not in itself be the solution . failing all else you should consult online communities to determine with the common best practice and what the industry best practice dictates and let us then go with the best known proven solution  
2. once we have resolved this point we should update the mybizz project documents accordingly 
---

### DISCREPANCY #10 — Dark mode wording

**Document A:** `anvil-ui-build-standard.md` §8 line 250 says: "Deferred from V1 — don't build dark-mode variants unless explicitly asked."

**Document B:** `DESIGN.md` line 87 says: "Dark mode as the primary design target. (Dark mode is deferred to V2, not rejected. It will be addressed in a future design phase.)"

**Question for user:** The Standard says "deferred from V1". DESIGN.md says "deferred to V2". Are these the same thing (V1 is current, V2 is next)? Or do they conflict?

#### Developer's Resonse
1. "deferred from V1". and  "deferred to V2" Or indeed the same thing 
2. I'm really not yet settled on where the Dark Mode should be deferred from V1 to V2 . the dark mode option seems to be a universal standard throughout the Internet almost all app have the option to engage either light or dark mode . not having this feature may well count against us in the market . My philosophy is that if it does not add a tremendous amount of complexity , if it is doable and feasible and it adds value then it should be included especially it seems to be accepted university as the standard . I would welcome your opinion on the matter Please make a recommendation 
3. If we do decide to apply the Dark Mode aspect Then we would need to update our docs accordingly All of them 

---

### DISCREPANCY #11 — Standard §2 card radius vs MUI overlay

**Document A:** `anvil-ui-build-standard.md` §2 line 94 says: "Radius: `--radius-md:8px`"
**Document A:** §3 line 104 says: "Card radius | 8px"

**Document B:** `design-direction.md` §Border Radius Rules line 116 says: "Cards | 12px | Standard"

**Question for user:** The Standard says card radius is 8px (MUI overlay). `design-direction.md` says 12px. The MUI overlay in `theme_mui_patch.css` overrides the base 12px to 8px. Should `design-direction.md` be updated to say 8px (reflecting the MUI overlay), or does it intentionally show the base M3 value?

#### Developer's Resonse
1. Yes, `design-direction.md` must be updated to say 8px (reflecting the MUI overlay)
---

### DISCREPANCY #12 — Icon prefix in Standard vs nomenclature.md

**Document A:** `anvil-ui-build-standard.md` §4 line 137 lists Icon prefix as `ic_` / `ico_`.

**Document B:** `nomenclature.md` §Component Prefixes does not list Icon at all.

**Question for user:** Should Icon be added to `nomenclature.md`'s prefix table? If so, which prefix: `ic_`, `ico_`, or both?

#### Developer's Resonse
1. This discrepancy involves researching Anvil material three theme , M3 dependency components docs To establish if an icon component exists . Currently I have found in the designer that icon button exists . the project docs often refer to an icon component . so the first discrepancy to resolve is whether there is in fact an icon component in the M3 dependency components list . This is important because we often want to put an icon in a button so in that case I suppose we use an icon button component,  but we often want to use a component separate from a button and it is not clear how to do that we need to determine from what the anvil docs say about an Icon component per se . 
2. I have also seen in our docs that sometimes we use the prefix "icon_". If there is an icon the prefix should be "ico_", If there is not a standard prefix for icon button then we should create our own prefix "ibn_" Well the standard prefix for button  is "btn". 
3. This issue is resolved by referencing the official anvil material three theme , anvil M3 dependency components documentation . Once you have resolved this in the nomenclature file has got to be updated first because that will be our the source of truth . the nomenclature file should also state the authoritative source of the decision . Subsequently all other files our project should be updated accordingly 
---

### DISCREPANCY #13 — Standard §6 writeback list vs spec-component-properties.md

**Document A:** `anvil-ui-build-standard.md` §6.1 lines 207-216 lists 7 writeback-capable components.

**Document B:** `spec-component-properties.md` §Summary lines 374-379 lists 5 Designer-only properties (TextBox hide_text, DropdownMenu hide_text, DatePicker hide_text, RepeatingPanel template, HTMLTemplate anvil_slot_repeat).

**Anvil docs confirm:** Writeback is a Designer Data Bindings property. The Standard's list matches what Anvil documents.

**Question for user:** The Standard lists writeback for Checkbox, Switch, RadioGroupPanel, Slider in addition to TextBox, TextArea, DropdownMenu, DatePicker. `spec-component-properties.md` doesn't list Checkbox/Switch/RadioGroupPanel/Slider as writeback-capable. Should `spec-component-properties.md` be updated?

#### Developer's Resonse
1. This discrepancy involves researching Anvil material three theme , M3 dependency components docs To establish The authoritative list of components which have write-back capability. Our documentation should be updated throughout to reflect that list.  The reason for this measure is that if we miss the write back property on a component then that breaks the application . all the lists in our documents are a result of our research the only authoritative source is the source of truth which is the proprietary anvil documentation  
---

### DISCREPANCY #14 — Template references `adr-register.md`

**Document A:** `anvil-ui-build-prompt-template.md` line 25 says: "per ADR-1 in `adr-register.md`"

**Document B:** `adr-register.md` has been deleted.

**Question for user:** Should this reference be updated to point to `adr/43-role-property-assignment-mechanism.md` (ADR-43)?

#### Developer's Resonse
1. The ADR-register.MD was a precursor to creating ADR'S 43,44  and 45 . Now that these Adr's have been created, that file became obsolete and was deleted therefore all references should point to the relevant ADR 
---

## Section 3: Anvil Compliance Check

### Components confirmed as real Anvil M3 components (from Anvil.works docs)

| Component | Source |
|---|---|
| Text | Anvil M3 components page — Typography section |
| Heading | Anvil M3 components page — Typography section |
| Link | Anvil M3 components page — Typography section |
| Button | Anvil M3 components page — Buttons section |
| IconButton | Anvil M3 components page — Buttons section |
| ToggleIconButton | Anvil M3 components page — Buttons section |
| Card | Anvil M3 components page — Display section |
| InteractiveCard | Anvil M3 components page — Display section |
| Avatar | Anvil M3 components page — Display section |
| Divider | Anvil M3 components page — Display section |
| NavigationLink | Anvil M3 components page — Navigation section |
| ButtonMenu | Anvil M3 components page — Menus section |
| MenuItem | Anvil M3 components page — Menus section |
| IconButtonMenu | Anvil M3 components page — Menus section |
| AvatarMenu | Anvil M3 components page — Menus section |
| Checkbox | Anvil M3 components page — Form Input section |
| RadioButton | Anvil M3 components page — Form Input section |
| RadioGroupPanel | Anvil M3 components page — Form Input section |
| TextBox | Anvil M3 components page — Form Input section |
| TextArea | Anvil M3 components page — Form Input section |
| DropdownMenu | Anvil M3 components page — Form Input section |
| Switch | Anvil M3 components page — Form Input section |
| FileLoader | Anvil M3 components page — Form Input section |
| Slider | Anvil M3 components page — Form Input section |
| LinearProgressIndicator | Anvil M3 components page — Feedback section |
| CircularProgressIndicator | Anvil M3 components page — Feedback section |
| NavigationRailLayout | Anvil M3 layouts page — confirmed |
| NavigationDrawerLayout | Anvil M3 layouts page — confirmed |

### Components confirmed as standard Anvil components (not M3-specific)

| Component | Notes |
|---|---|
| ColumnPanel | Standard Anvil container — not on M3 page but used throughout Anvil docs |
| FlowPanel | Standard Anvil container — not on M3 page |
| DataGrid | Standard Anvil component — not on M3 page |
| RepeatingPanel | Standard Anvil component — not on M3 page |
| DataRowPanel | Standard Anvil component — used for DataGrid row templates |
| Image | Standard Anvil component — referenced in M3 Card examples |
| Plot | Standard Anvil component (Plotly integration) |
| RichText | Standard Anvil component |

### Properties confirmed from Anvil docs

| Property | Confirmation |
|---|---|
| `appearance` on Button | Confirmed: "five different pre-defined appearances" |
| `appearance` on Card | Confirmed: `elevated`, `filled`, `outlined` |
| `appearance` on InteractiveCard | Confirmed: `elevated`, `filled`, `outlined` (same as Card) |
| `appearance` on TextBox | Confirmed: `filled`, `outlined` |
| `appearance` on TextArea | Confirmed: `filled`, `outlined` |
| `appearance` on DropdownMenu | Confirmed: `filled`, `outlined` |
| `appearance` on FileLoader | Confirmed: "five pre-defined appearance options" |
| `role` property | Not explicitly documented on M3 page — documented in Anvil's Roles docs |
| `wrap_on` property | Not on M3 page — documented in Anvil's standard component docs |
| `navigate_to` on NavigationLink | Confirmed |
| `badge` / `badge_count` on NavigationLink | Confirmed |
| `show_sidesheet` on layouts | Confirmed |
| `navigation_rail_collapse_to` on NavigationRailLayout | Confirmed |

### Items that could NOT be verified against Anvil M3 docs

| Item | Status |
|---|---|
| `wrap_on` property | Not on M3 components page. May be in standard Anvil docs or forum. ADR-44 cites Anvil staff forum confirmation. |
| DataRowPanel | Not on M3 components page. Standard Anvil component. |
| Plot | Not on M3 components page. Standard Anvil component (Plotly). |
| RichText | Not on M3 components page. Standard Anvil component. |

#### Developer's Resonse
1. When Anvil adopted the M3 standard and created the Material Three theme , They did not adopt all the components that material Three has . Our obligation is to hold true to Anvil . The M3 standard that we apply is the M 3 standard which was adopted by Anvil , Therefore we cannot extend beyond Anvil and adopt other M3 components which are available under the M3 standard university because that would conflict with Anvil . However anvil has its own components Anvil is a mature ecosystem and we are working with that ecosystem but there are indeed aspects that are confusing and need to be properly researched and resolved. Some examples include :
- Anvil has a Label component but Material Three theme does not have a label component , The designer also does not have a label component therefore we assume that label component is not available because it's not in the designer .
- Anvil has a plot component but Material Three theme does not have a plot component yet the designer shows the plot component Therefore we assume that the plot component is available because it is available in the designer 
- Various documentation say various things about properties . Clearly every component has its set of properties and of those properties some cannot be programmatically, those must be set in the designer. I have held through all my documentation that we only set properties in the designer that cannot be set programmatically , The reason for this is so that we can keep track of what is needed to be set where . It is easy like this. We list all the properties that cannot be set programmatically and those must be set in the designer . so we are able to create a definitive list per component . The other side of this is that every property that can be set programmatically must be set programmatically . So having a hard rule like this makes planning easy .
- The situation occurs where we set properties in the designer which can be set programmatically , So the question becomes which overwrites which ? if the property is set in the designer can a programmatic setting over designer setting ? case in point the Hide text property on a text box , indivisible property on any component .
- If the lift switch you have provided above are definitive an authoritative from source of truth documentation Then they should be added Two our "C:\mybizz\project-library\docs\m3_component_mapping.md" file Like to go further and list the properties and identify all the properties that are required to be set in the designer because if those properties are not set in the designer then the end result may break our application or disturb the functionality 

---

## Section 4: Open Questions

1. **ADR-37 vs ADR-45 on NavigationRailLayout/NavigationDrawerLayout** — This is a genuine conflict between two ADRs. One says not used, the other says used. Needs user decision.
#### Developer's Resonse
1. ADR-37 says NavigationRailLayout/NavigationDrawerLayout are NOT used Was created at an early discovery stage when it was assumed that these components conflict with the M3 standard . The project policy is Anvil material 3 theme M3 components is the standard throughout the application and their deviation may only occur when there is absolutely no solution within the official Anvil Material 3; M3 components framework. ADR 37 should be marked superseded (likely by ADR 45 )
---
2. **Component list reconciliation** — The Standard and ADR-15 have different component lists. Neither is wrong (they cover different scopes), but they should be aligned.
#### Developer's Resonse
1. Section 3: Anvil Compliance Check above deals with this. see my response there
---
3. **`tonal-card` role vs `tonal` appearance** — The Standard correctly says `tonal` is not a valid InteractiveCard appearance, but lists `tonal-card` as a Card role. Need to confirm: is `tonal-card` a CSS class name (not an Anvil appearance property)?
#### Developer's Resonse
1. DISCREPANCY #7 — `tonal-card` role vs `tonal` appearance deals with this. see mty response there
----
4. **Card radius in design-direction.md** — Shows 12px (base M3), but MUI overlay changes it to 8px. Should the design doc show the effective value (8px) or the base value (12px)?
#### Developer's Resonse
1. show the effective value (8px)
---

*End of UI Build Audit Report*
