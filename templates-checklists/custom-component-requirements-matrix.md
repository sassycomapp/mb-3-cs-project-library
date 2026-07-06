# Mybizz CS — Custom Component Requirements Matrix

**Date:** 2026-06-01
**Status:** Final V1 Scope
**Project:** Mybizz CS
**Platform:** Anvil.works with Material Design 3 (M3)

---

## Definition

A custom component is a reusable interactive UI element that:
1. Is not adequately provided by native Anvil Material 3 components
2. Is intended for use in more than one location

---

## Custom Component Matrix

| Component | Reused In | Usage Count | Justification |
|-----------|-----------|-------------|---------------|
| **VideoDisplayComponent** | BlogPostForm, HomePage | 2 | Native Anvil YouTube insufficient for multi-source video (YouTube, Vimeo, etc.) |
| **FooterComponent** | All public pages (6+) | 6+ | Custom footer with logo, social links, policies, map, contact — not in native Anvil |
| **TimeLapseCarouselComponent** | HomePage, ServicesForm | 2 | Auto-rotating carousel for service/catalogue display — not in native Anvil |
| **ParallaxComponent** | Multiple public pages | 2+ | Parallax scroll effect for hero sections — not in native Anvil |
| **ClauseBuilder** | PrivacyPolicyEditor, TermsConditionsEditor | 2 (planned) | Reusable clause-level editor for policy documents — not provided by native M3. Parameterized with document type (Privacy Policy vs T&Cs). |

### Downgraded Components

| Component | Previous Status | Current Status | Reason |
|-----------|----------------|----------------|--------|
| **PaletteSettingsForm** | Shared custom component | Local/form-specific UI | Only used in SettingsForm. Does not meet the shared component criteria of use in more than one place. |
| **OnboardingPaletteForm** | Shared custom component | Removed (obsolete) | Onboarding no longer has a separate palette form. Palette selection is done via Settings → Palette tab. |

---

## Implementation Notes

### VideoDisplayComponent
- Supports YouTube, Vimeo, and other video sources
- Must be implemented as a reusable component
- Use in: BlogPostForm, HomePage

### FooterComponent
- Contains: logo, social links, policies, map, contact
- Must be implemented as a reusable component
- Use in: All public pages (HomePage, BlogList, BlogPost, ContactPage, AboutPage, ServicePage)

### TimeLapseCarouselComponent
- Auto-rotating carousel with manual navigation
- Must be implemented as a reusable component
- Use in: HomePage, ServicesForm

### ParallaxComponent
- Parallax scroll effect with background image
- Must be implemented as a reusable component
- Use in: Multiple public pages

### ClauseBuilder
- Reusable clause-level editor for policy documents (Privacy Policy and T&Cs)
- Parameterized with document type (`privacy_policy` or `t_and_c`)
- Supports: category selection, clause selection from seeded list, custom clause creation, clause editing, clause ordering
- On save: writes changes to the appropriate data source (`privacy_policy` or `t_and_c` table)
- Must be implemented as a standalone custom component (not embedded in each PolicyEditorForm)
- Use in: PrivacyPolicyEditor, TermsConditionsEditor (both in Settings → Website)

### PaletteSettingsForm (Local/Form-Specific)
- Palette selection with preview
- Local to SettingsForm — not a shared custom component
- Use in: SettingsForm only

---

## V1 Custom Component Count

| Component | Count |
|-----------|-------|
| **VideoDisplayComponent** | 1 |
| **FooterComponent** | 1 |
| **TimeLapseCarouselComponent** | 1 |
| **ParallaxComponent** | 1 |
| **ClauseBuilder** | 1 |
| **Total Shared Custom Components** | **5** |

---

*End of file — Custom Component Requirements Matrix*
