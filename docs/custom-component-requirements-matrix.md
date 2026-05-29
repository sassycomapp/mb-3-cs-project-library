# Mybizz CS — Custom Component Requirements Matrix

**Date:** 2026-05-28  
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

| Component | Reused In | Justification |
|-----------|-----------|---------------|
| **VideoDisplayComponent** | BlogPostForm, HomePage | Native Anvil YouTube insufficient for multi-source video (YouTube, Vimeo, etc.) |
| **FooterComponent** | All public pages | Custom footer with logo, social links, policies, map, contact - not in native Anvil |
| **PaletteSettingsForm** | SettingsForm, Onboarding flow | Reusable palette selection form for client customization |
| **TimeLapseCarouselComponent** | HomePage, ServicesForm | Auto-rotating carousel for service/catalogue display - not in native Anvil |
| **ParallaxComponent** | Multiple public pages | Parallax scroll effect for hero sections - not in native Anvil |

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

### PaletteSettingsForm
- Palette selection with preview
- Must be implemented as a reusable form component
- Use in: SettingsForm, Onboarding flow

### TimeLapseCarouselComponent
- Auto-rotating carousel with manual navigation
- Must be implemented as a reusable component
- Use in: HomePage, ServicesForm

### ParallaxComponent
- Parallax scroll effect with background image
- Must be implemented as a reusable component
- Use in: Multiple public pages

---

## V1 Custom Component Count

| Component | Count |
|-----------|-------|
| **VideoDisplayComponent** | 1 |
| **FooterComponent** | 1 |
| **PaletteSettingsForm** | 1 |
| **TimeLapseCarouselComponent** | 1 |
| **ParallaxComponent** | 1 |
| **Total Custom Components** | **5** |

---

*End of file — Custom Component Requirements Matrix*
