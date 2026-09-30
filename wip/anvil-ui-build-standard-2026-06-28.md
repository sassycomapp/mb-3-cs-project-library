---
title: "anvil-ui-build-standard-2026-06-28"
doc-id: "mb-3c752899"
---
# Anvil UI Build Standard

**Version:** 2.3
**Supersedes:** anvil-ui-build-standard.md (v2.2)
**Filename note:** This file intentionally has no version number in its name. Keep tracking
version/changelog inside the document, not the filename, so the prompt template never needs
its path reference updated when this file changes. If your Assets copy is currently named
`anvil-ui-build-standard-v1.md`, rename it (or update the path the Template references) —
your call.

---

## Changelog (v2.2 → v2.3)

- **§11 rewritten entirely** — `wrap_on` (per-container, `mobile`/`never`) replaces the breakpoint claim, per ADR-44. Max-width table replaced with final values (List/Dashboard now 1200, not 1100). Custom components now explicitly have no fixed width — they inherit from host context.
- **InteractiveCard added** to §6 approved list. Valid appearances: elevated/filled/outlined — `tonal` is invalid and should be corrected to `filled`.
- **HTMLTemplate banned** — §1 and §7, per ADR-45. Native M3 Layouts cited as the replacement for layout-shell cases.
- All three ADRs referenced in this document now live in `adr/` as individual files: `adr/43-role-property-assignment-mechanism.md`, `adr/44-responsive-behaviour-mechanism.md`, `adr/45-htmltemplate-use.md`.

## Changelog (v2.1 → v2.2)

- **Resolved §6 role-assignment decision** — see `adr/43-role-property-assignment-mechanism.md` (ADR-43, superseded). Original decision: Designer Properties Panel. Reversed by project rule: `role` is set programmatically in code (Anvil docs confirm it can be). New §6.0 updated to show code-based role assignment.

## Changelog (v2.0 → v2.1)

- Corrected typography scale in §2 — was the generic published M3 scale, not this
  project's actual scale (confirmed identically in `DESIGN.md` and `design-direction.md`).
- Added missing radius values to §2 (filled inputs, chips/badges).
- Added minimum mobile test width (375px) to §11.
- Expanded §8 Accessibility from one sentence to specifics: contrast ratios by text
  size, keyboard tab-order, ARIA requirements, touch-target minimums, colour-blind-safe
  pattern rule.
- §6.1 row-template property now named explicitly (`template`, not a generic
  description) — this was being called "row_template" informally in conversation; the
  actual Designer-only property name is `template`.
- §10 extended: added `icon_alignment` (leading/trailing) guidance for Buttons, and
  added `TextBox.leading_icon` as a previously-uncovered case — §10 only covered
  Button/IconButton before this.
- **Not yet applied — pending your decision, see flagged items below:** Anvil Role
  assignment (new section), §11's "M3 handles responsiveness, no spec needed" claim
  (overstated), max-width table conflict with `design-direction.md`, Card-as-native-
  component-vs-ColumnPanel question, Label-component exception for tags/badges,
  `hide_text` naming correction, and the InteractiveCard/SidesheetContent/HTMLTemplate
  approved-list addition. These are genuine contradictions between source documents or
  missing information only you have — not defaults I'm willing to silently pick.

## Changelog (v1.1 → v2.0)

- Closed: responsive breakpoints (Anvil's M3 theme handles this natively — no manual spec needed).
- Closed: event handler naming (confirmed as Anvil's own default framework convention, not a project invention).
- Closed: formal accessibility spec (WCAG 2.1 AA adopted as the applicable industry baseline).
- Reframed: colour palette section — Professional Blue is the current default, not a fixed authority; a future user-selectable 4-palette system is planned.
- Reframed: max-width-per-form-type table — downgraded from binding rule to existing project convention, subject to best-practice judgment.
- Rewrote §10 (Icon Matching) — now uses the wireframe's native `mi:` notation; explicitly excludes DataGrid/RepeatingPanel row content (that belongs to a separate row-template Form).
- Added §6 Property Fidelity — elevates writeback, appearance/outline, hide_text, and row-template wiring to first-class, can't-fix-later build requirements.
- Added §9 Form-Boundary Rules — the wireframe's outer container is the full build scope; page chrome (sidebar/app bar/footer) and row-template content are explicitly out of scope.
- Added §0 Guiding Principle and §12 Final QA Gate.

---

## 0. Guiding Principle

Every decision in this build process is judged against four criteria — **Simple, Secure, Robust, Fit-for-purpose.**

- **Simple:** use the most straightforward approved-component solution. Do not add unrequested cleverness or restructure beyond what's declared.
- **Secure:** respect RBAC/auth decorators; never expose secrets; only genuine Designer-set writeback creates real data bindings — don't fake it in code.
- **Robust:** a Form that looks right but has unset Designer properties (writeback, row-template reference) isn't actually built correctly. Wiring matters as much as appearance.
- **Fit-for-purpose:** match the wireframe's declared structure and the screenshot's visual intent exactly — no more, no less.

**Best-practice default rule:** This project's own planning documents were produced by a first-time, non-specialist author working with AI assistance, with no claim to authority. Where this Standard is silent, ambiguous, or untested, apply the established Anvil framework convention or general industry UI/UX best practice — not an invented or creative alternative. Keep any such judgment call narrow and minimal; this is not licence for redesign.

---

## 1. Hard Prohibitions (check first — most common failure modes)

- **No hardcoded colour values.** Every colour resolves through a CSS custom property in `theme_patched.css`. No literal hex/`rgb()`/`rgba()`/`hsl()` — except the MUI card-shadow `hsla()` values in §3.
- **No legacy `Label` component.** Use `Text` instead.
- **No component outside the approved M3 list** (§6).
- **No `HTMLTemplate`.** Banned per ADR-45 (see `adr/45-htmltemplate-use.md`). Use stock M3 components instead; for layout-shell cases specifically, use the native `NavigationRailLayout` or `NavigationDrawerLayout`.
- **No MUI React components, no `sx` props, no MUI imports, no MUI CDN links — ever.** MUI is a CSS aesthetic overlay only.
- **App bar must use `var(--surface)`** — never black, dark grey, or hardcoded dark colour.
- **Wireframe HTML is the structural authority within its own boundary** (see §9). Build only what's declared inside the wireframe's outermost container.
- **Designer-only properties cannot be fixed by code afterward** (see §6). If `writeback` isn't set in the Designer, no server-side Python compensates for it.

---

## 2. Visual Tokens

**Colour:** Professional Blue is the *current default* token set (19 CSS custom properties in `theme_patched.css`), not a fixed authority — a future settings page will let users choose among four palettes. The only binding rule for now: colours always resolve through a CSS custom property, never a literal value, so the future palette-swap works without touching component code. Status colours (green/orange/red for success/warning/error) remain an accepted semantic exception outside the palette system.

**Typography:** `'Roboto', sans-serif`. Project scale (confirmed identically in `DESIGN.md` and `design-direction.md` — this is the actual project scale, not the generic published M3 scale): Display 57/64/400 · Headline Large 32/40/400 · Headline Small 22/28/500 · Title 18/24/500 (single Title role — no Large/Medium split) · Body 14/20/400 (single Body role) · Button 14/14/500 · Label 12/16/500 (single Label role) · Caption 12/16/400.

**Spacing (4px base grid):** `--space-xs:4px` `--space-sm:8px` `--space-md:16px` `--space-lg:24px` `--space-xl:32px` `--space-2xl:48px`
**Radius:** `--radius-sm:4px` `--radius-md:8px` `--radius-lg:12px` `--radius-xl:16px` `--radius-full:100px` · Filled inputs: 0px (bottom-border-only pattern) · Chips/Badges: 8px.

---

## 3. MUI Aesthetic Overlay

| # | Aspect | MUI value | M3 default |
|---|---|---|---|
| 1 | Surface distinction | Two-level: body `var(--surface)`, panels/nav `var(--surface-container-low)` | Single surface |
| 2 | Card shadow | `hsla(220,30%,5%,0.07) 0 4px 16px 0, hsla(220,25%,10%,0.07) 0 8px 16px -5px` | M3 elevation |
| 3 | Card radius | 8px | 12px |
| 4 | Heading weight | 600 | 400 |
| 5 | Dividers | outline-variant @ 40% opacity | Solid |
| 6 | Hover state opacity | 0.08 | M3 default |
| 7 | Pressed/focused opacity | 0.12 | M3 default |
| 8 | App bar | `var(--surface)`, 1px hairline bottom border @ outline-variant 40% | M3 default |
| 9 | Left nav | `var(--surface-container-low)`, 1px hairline right border @ outline-variant 40% | M3 default |

**Preserved from M3:** Button radius 100px · Dialog radius 28px · Input radius 4px · Nav item radius 100px · Data grid radius 24px · Font family Roboto · 4px spacing rhythm · M3 colour-role structure.

**Bootstrap process (do this once, not per-UI):** A complete, deployment-ready `theme_patched.css` already exists with this overlay built in (plus reduced-motion support) — replace this app's `Assets/theme.css` with that file directly. This is a manual one-time file replacement, not a prompt to Anvil's AI agent — there's nothing here for the agent to author. See the Template's bootstrap section for the exact steps.

---

## 4. Naming Conventions

**Component prefixes:**

| Component | Prefix | Example |
|---|---|---|
| ColumnPanel | `col_` | `col_email_config` |
| FlowPanel | `flow_` | `flow_sender_fields` |
| Card | `card_` | `card_sender` |
| DataRowPanel | `drp_` | `drp_user_row` |
| Heading | `hdg_` | `hdg_email_title` |
| Text | `txt_` | `txt_email_subtitle` |
| TextBox / TextArea | `txt_` | `txt_from_email` |
| DropdownMenu | `dd_` | `dd_timezone` |
| Button / IconButton | `btn_` | `btn_save`, `btn_edit` |
| CheckBox | `cb_` | `cb_gateway_connected` |
| Switch | `sw_` | `sw_test_mode` |
| RepeatingPanel | `rp_` | `rp_secrets` |
| DataGrid | `dg_` | `dg_users` |
| Icon | `ico_` | `ico_empty_vault` (maps to `IconButton` used decoratively) |
| Link | `nav_` | `nav_forgot_password` |
| DatePicker | `dp_` | `dp_date_from` |
| Plot | `plot_` | `plot_revenue_trend` |
| Image | `img_` | `img_service` |
| RadioButtonGroup | `rbg_` | `rbg_meeting_type` |
| FileLoader | `fu_` | `fu_attachment` |

**File naming:** `wireframe-{package}-{FormName}.html` / `screen-{package}-{FormName}.html` / `wireframe-{package}-{FormName}-RowTemplate.html`. No hyphens in Anvil file/component names — PascalCase the `{FormName}` portion.

**Event handler naming:** `{component_name}_{event}` — e.g. `btn_save_click`, `dd_timezone_change`, `txt_email_change`. This is Anvil's own default framework convention (matches what the Designer auto-generates), not a project-specific invention — apply it as the established standard, no further confirmation needed.

---

## 5. HTML → Anvil Component Mapping

| HTML Element | Anvil Component | Notes |
|---|---|---|
| `<h1>`, `<h2>` | Heading | `hdg_` |
| `<p>` | Text | `txt_` |
| `<input type="text">` | TextBox | `appearance="outlined"` |
| `<input type="email">` | TextBox | |
| `<input type="password">` | TextBox | `hide_text = True` |
| `<textarea>` | TextArea | |
| `<select>` | DropdownMenu | |
| `<input type="date">` | DatePicker | |
| `<input type="checkbox">` | CheckBox | |
| `<button>` | Button | |
| `<a>` | Link | |
| `<div>` (vertical stack) | ColumnPanel | |
| `<div>` (horizontal flow) | FlowPanel | |
| `<div>` (card) | Card | |
| `<table>` / list grid | DataGrid | see §9 — row content out of scope |
| Repeating list | RepeatingPanel | see §9 — row content out of scope |
| Toggle | Switch | |
| Radio buttons | RadioButtonGroup | |
| Chart | Plot | |
| Icon span | Icon | Material Icons ligature, `class="material-icons"` |
| File upload | FileLoader | |

**Structural rule:** FlowPanel = horizontal, ColumnPanel = vertical, DataRowPanel = DataGrid row templates. Containers must be built as actual containers with components nested inside.

---

## 6. Approved Components & Property Fidelity

**Approved M3 components (nothing outside this list):**
ColumnPanel, FlowPanel, Card, InteractiveCard, DataRowPanel, Heading, Text, Link, RichText, TextBox, TextArea, DropdownMenu, DatePicker, CheckBox, Switch, RadioButtonGroup, RadioButton, Slider, Button, IconButton, DataGrid, RepeatingPanel, Icon (maps to `IconButton` used decoratively — no click event), Image, FileLoader, Plot, LinearProgressIndicator, CircularProgressIndicator.

**InteractiveCard** — valid appearances: `elevated`, `filled`, `outlined` (same as Card). `tonal` is **not** a valid InteractiveCard appearance — if a wireframe specifies `tonal`, treat it as an error and use `filled` instead.

### 6.0 Role assignment (Designer Properties Panel)

**Decided — see `adr/43-role-property-assignment-mechanism.md` (ADR-43, superseded).** The project-wide property-setting rule applies: **if a property can be set programmatically, it must be set programmatically.** Anvil's Roles documentation confirms `role` can be set in code (`self.button_1.role = "submit"`), so role assignment is done in code, not in the Designer Properties Panel.

Every MUI overlay rule in `theme_patched.css`/`theme_mui_patch.css` targets `.anvil-role-*` selectors. Those classes only attach if the component's `role` property is explicitly set to a matching name. Set `role` in code using the name from the table below matching the component's declared type/purpose in the wireframe.

```python
# Role assignment in code
self.save_button.role = "primary-button"
self.info_card.role = "surface-card elevated"
```

Components can have multiple roles (space-separated string or list of strings).

| Component type | Role names |
|---|---|
| Button | `primary-button` (filled, primary actions) · `secondary-button` (outlined, secondary actions) · `tertiary-button` (text, low-emphasis) · `icon-button` · `fab-button` |
| Card | `surface-card` (outlined, standard) · `elevated-card` (emphasised) · `tonal-card` (grouped) · `data-card` (rows/lists) · `metric-card` (KPIs) |
| ColumnPanel (page-level) | `page-container` · `section-container` · `form-container` · `card-container` · `grid-container` |
| Heading | `page-title` · `section-title` · `card-title` · `label-title` |
| Link/NavigationLink | `nav-item` · `nav-item-selected` · `app-bar-link` · `breadcrumb-item` |
| TextBox/TextArea/DropdownMenu/DatePicker | `outlined-input` / `outlined-select` / `outlined-date` as applicable; `input-error` for visible error states |

**CSS coverage status:** Phase 1 investigation (2026-06-28) confirmed that `input-disabled`, `nav-item`, `nav-item-selected`, and `app-bar-link` are all native-covered by existing CSS (structural selectors or attribute selectors). `input-readonly` and `breadcrumb-item` are aspirational (no wireframe uses them yet). No additional CSS is needed for the native-covered roles.

### 6.1 Cannot be fixed in code afterward — verify these explicitly

**Writeback** — must be ticked in the Designer's Data Bindings panel. No server-side Python compensates for a missed writeback flag; the binding simply doesn't exist.

| Component | Writeback property |
|---|---|
| TextBox / TextArea | `text` |
| Checkbox | `checked` |
| Switch | `selected` |
| RadioGroupPanel | `selected_value` |
| Slider | `value` |
| DropdownMenu | `selected_value` |
| DatePicker | `selected_date` |

Not writeback-capable: RadioButton, FileLoader. Display-only: Text, Link, RichText, Heading, LinearProgressIndicator, CircularProgressIndicator.

**Row-template reference** — when a DataGrid is placed, Anvil auto-scaffolds both the RepeatingPanel and a row-template Form. The agent does not need to create either from scratch. It does need to set the RepeatingPanel's Designer-only `template` property to point at the correct already-scaffolded Form (commonly named `{FormName}RowTemplate`, per §4 file naming). This is an in-scope wiring task even though the row template's own contents are out of scope (§9).

### 6.2 Must visually match the screenshot — verify, correct if wrong

**Outline/appearance** — TextBox, TextArea, DropdownMenu, DatePicker, and Button all have an `appearance` property (e.g. `outlined`, `filled`). Check every input-style component against the screenshot and set/correct `appearance` to match what's actually rendered — this is a common, easy-to-miss visual mismatch.

**`hide_text`** for password-style fields, where applicable.

### 6.3 Out of scope for this build stage

Event handler logic and server-side data-binding code are not part of wireframe-to-Form conversion. The wireframe/screen pair governs structure and Designer properties only (§0 Robust, §9). Behaviour belongs in separate specification documents per the project's existing wireframe rules.

---

## 7. Custom HTML Components

Only these five shared custom components exist — never invent a new one for a single form:

`ClauseBuilder`, `FooterComponent`, `VideoDisplayComponent`, `TimeLapseCarouselComponent`, `ParallaxComponent`

Use only when (a) it's one of the five above, and (b) it serves a reusable purpose across multiple forms.

**Not on this list and not a substitute for it:** `HTMLTemplate` — banned (§1, ADR-45). A wireframe calling for a custom layout shell or arbitrary HTML/CSS wrapper should be built with stock M3 components, or with the native `NavigationRailLayout`/`NavigationDrawerLayout` for shell cases — not with `HTMLTemplate`, and not as a sixth custom component.

---

## 8. Data, Empty States, RBAC & Accessibility

- **Sample/placeholder data:** South African consulting/services vertical, ZAR currency, plausible business data. Never "Lorem ipsum."
- **Empty state (mandatory on every data-driven form):** exactly 4 components — Icon, Heading, Body text, Primary action button.
- **RBAC:** Roles are Owner, Manager, Admin, Staff, Customer. Every server function carries `@authenticated_endpoint`.
- **Dark mode:** Deferred from V1 to V2 — don't build dark-mode variants unless explicitly asked. CSS architecture supports future addition via `@media (prefers-color-scheme: dark)` blocks with alternate token values.
- **Accessibility:** WCAG 2.1 AA, per `design-direction.md`'s Accessibility section:
  - **Contrast:** ≥4.5:1 for normal text; ≥3:1 for large text (≥18px, or ≥14px bold) and for UI components.
  - **Keyboard:** every function operable via keyboard (Tab, Enter, Escape); logical tab order (top-left → bottom-right); modals/sidesheets trap focus until closed and return focus to the trigger element on close.
  - **ARIA:** icon-only buttons require `aria-label`; form inputs require an associated `<label>`; dynamic content updates use `aria-live="polite"` where applicable.
  - **Touch targets:** 44×44px minimum, 48×48px recommended, minimum 8px spacing between adjacent targets.
  - **Colour-blind safety:** never rely on colour alone — error/success/status states pair colour with an icon and a text label, not colour by itself.

---

## 9. Build Process & Form-Boundary Rules

1. One Form built per conversation — no batching.
2. Authority chain: Architecture → Wireframes → Screens. The wireframe wins on structure; the screenshot wins on visual styling, subject to the exceptions below.
3. Material Icons font must be imported on any Form using navigation or icon components.

### 9.1 The wireframe's outermost container is the full build boundary

A screenshot is a capture of the *entire rendered page* — it commonly includes a left sidebar, top app bar, notification/profile icons, and footer/sign-out elements that belong to the app's shared shell, not to the Form being built. None of that exists in the wireframe. **Anything in the screenshot outside the wireframe's declared outer container (e.g. `col_main`) is out of scope** — do not build it, do not reference it.

### 9.2 DataGrid / RepeatingPanel row content is a separate Form's responsibility

A screenshot showing a populated DataGrid (rows of real-looking data, action icons, tags) is rendering the *row-template Form*, not the Form currently being built. That row-template Form has its own future wireframe and screenshot, produced as a separate task. **Do not build row content now.** The only in-scope action is the row-template wiring described in §6.1.

---

## 10. Icon Matching

The wireframe's own notation already has a syntax for icons — `mi:<icon_name>` — seen on `IconButton` (e.g. `mi:event_busy`). Some wireframes omit this annotation on Buttons that visibly carry an icon in the screenshot anyway.

**Rule, scoped:** within the build boundary defined by §9.1 (excludes page chrome) and excluding anything covered by §9.2 (DataGrid/RepeatingPanel row content) — if the screenshot shows an icon on a Button or IconButton that belongs to *this* Form, and the wireframe text doesn't declare it, build it anyway using the closest matching icon below, in the wireframe's own `mi:` notation.

| Button label / context | Icon |
|---|---|
| Save / Save X | `mi:save` |
| Block / Block X | `mi:block` |
| Add / Add X | `mi:add` |
| Edit | `mi:edit` |
| Cancel / Close | `mi:close` |
| Search | `mi:search` |
| Filter | `mi:filter_list` |
| Download | `mi:download` |
| Upload | `mi:upload` |
| Send | `mi:send` |
| Back | `mi:arrow_back` |
| Next / Continue | `mi:arrow_forward` |

**Explicitly excluded:** delete/trash icons and any other icon-only control rendered *inside* a DataGrid row belong to the row-template Form (§9.2), not this one — do not build them now regardless of how clearly they appear in the screenshot.

If a screenshot icon doesn't match this table, use the closest standard Material Icons name (`mi:` prefix) and note the choice.

**`icon_alignment`** — Button has a real `icon_alignment` property (`"leading"`/`"trailing"`). Every icon observed so far in screenshots has been leading (icon before text) — set this explicitly to match the screenshot rather than leaving it at default.

**TextBox icons** — this section's scope isn't limited to Button/IconButton. TextBox can carry its own `leading_icon` property (e.g. `leading_icon="mi:email"`). If a wireframe shows a TextBox with a visible icon in the screenshot but no `mi:` annotation in the text, the same rule in this section applies: build it anyway, using the closest matching icon.

---

## 11. Layout Width & Responsiveness

**Responsiveness: `wrap_on`, not breakpoints.** Per ADR-44 (`adr/44-responsive-behaviour-mechanism.md`), Anvil's actual responsive mechanism is the `wrap_on` property, set per-container (ColumnPanel, FlowPanel, DataGrid all support it) — not a CSS breakpoint/media-query system. Set `wrap_on` to `mobile` where the wireframe shows components stacking/wrapping on narrow screens, or `never` where it shows a fixed layout that should hold its shape regardless of screen width. There is no breakpoint table to apply — judge this per-container from what the wireframe and screenshot actually show.

Separately, and automatically — no instruction needed: the nav rail/drawer collapses to a modal drawer or app bar on small screens natively.

**Minimum mobile width:** 375px (`design-direction.md`). Treat this as the floor any build should remain usable at.

**Max-width by form type** — final values:

| Form type | Max-width |
|---|---|
| Auth forms | 400px, centred |
| List forms | 1200px |
| Dashboard | 1200px |
| Editor / Viewer forms | 900px |
| Public pages | 900px |
| Settings | 1000px |
| Onboarding | 600px, centred |
| Card (grid item) | 320px |
| Modal — form | 480px |
| Modal — detail | 640px |

**Custom components have no fixed width — they inherit from their host context.** Do not apply a max-width to `FooterComponent`, `ParallaxComponent`, `TimeLapseCarouselComponent`, `VideoDisplayComponent`, or `ClauseBuilder` independent of where they're placed. `FooterComponent` and `ParallaxComponent` are typically full-bleed/full-width by design; the others take whatever width their host Form provides at that point in the layout.

---

## 12. Final QA Gate

Before presenting any build as complete, confirm:

- [ ] Simple / Secure / Robust / Fit-for-purpose (§0) — does the build hold up against all four?
- [ ] Only approved M3 components used (§6); no legacy `Label`
- [ ] Writeback set for every writeback-capable component declared in the wireframe (§6.1)
- [ ] Row-template reference set correctly on every DataGrid/RepeatingPanel (§6.1) — contents not built (§9.2)
- [ ] Appearance/outline matches the screenshot for every input-style component (§6.2)
- [ ] Nothing built from page chrome outside the wireframe's outer container (§9.1)
- [ ] Icons matched per §10, correctly scoped (not from DataGrid rows)
- [ ] No hardcoded colours; MUI overlay present in `theme.css`
- [ ] Naming prefixes and event handler names follow §4

---

## Remaining Open Items

| Item | Status |
|---|---|
| 4-palette colour system | Deferred — future settings-page feature, not yet built |
| Dark mode | Deferred to V2 |

*End of v2.3.*
