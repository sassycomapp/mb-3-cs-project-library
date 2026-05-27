# Mybizz CS — Custom Component Requirements Matrix

**Date:** 2026-05-27  
**Status:** Final V1 Scope  
**Project:** Mybizz CS  
**Platform:** Anvil.works with Material Design 3 (M3)

---

## Executive Summary

This matrix documents the custom component requirements for Mybizz CS V1. It reconciles the DESIGN.md proposal against actual application requirements from the build-plan, wireframes, and architecture documents.

**Total Proposed Components:** 22  
**V1 Required:** 18 (82%)  
**V1 Not Required:** 4 (18%)

---

## Custom Component Requirements Matrix

| Component | Requirement | Rationale | Forms Using It | Confidence | Native/M3 Alternative | Notes |
|-----------|-------------|-----------|----------------|------------|---------------------|-------|
| **PaletteSettingsForm** | ✅ Required | Explicitly required in build-plan Phase 1 Settings. Used for client palette customization. | SettingsForm | High | No | Custom form not available in standard Anvil |
| **OnboardingPaletteForm** | ✅ Required | Explicitly required in build-plan Phase 1 Settings. Used during initial onboarding. | Onboarding flow | High | No | Custom form not available in standard Anvil |
| **BlogListTiledTemplate** | ✅ Required | Explicitly required in build-plan Phase 4 Blog. Provides tiled grid view for blog listing. | BlogListForm (public) | High | No | Custom template not available in standard Anvil |
| **BlogListListTemplate** | ✅ Required | Explicitly required in build-plan Phase 4 Blog. Provides list view for blog listing. | BlogListForm (public) | High | No | Custom template not available in standard Anvil |
| **BlogPostClassicTemplate** | ✅ Required | Explicitly required in build-plan Phase 4 Blog. Classic centered layout. | BlogPostForm (public) | High | No | Custom template not available in standard Anvil |
| **BlogPostTwoColumnTemplate** | ✅ Required | Explicitly required in build-plan Phase 4 Blog. Two-column with sidebar layout. | BlogPostForm (public) | High | No | Custom template not available in standard Anvil |
| **BlogPostFullWidthTemplate** | ✅ Required | Explicitly required in build-plan Phase 4 Blog. Full-width immersive layout. | BlogPostForm (public) | High | No | Custom template not available in standard Anvil |
| **VideoDisplayComponent** | ✅ Required | **ADDED TO V1** - Required for blog posts and homepage to display video from multiple sources (YouTube, Vimeo, etc.). Anvil's YouTube component is insufficient. | BlogPostForm (public), HomePage (public) | High | No | Standard Anvil Image/YouTube component insufficient for all video sources |
| **LeadCaptureTemplate** | ✅ Required | Explicitly required in build-plan Phase 3 Landing Pages. Captures leads from public forms. | LandingPageListForm (public) | High | No | Custom template not available in standard Anvil |
| **EventRegistrationTemplate** | ✅ Required | Explicitly required in build-plan Phase 3 Landing Pages. Event registration form. | LandingPageListForm (public) | High | No | Custom template not available in standard Anvil |
| **VslTemplate** | ✅ Required | Explicitly required in build-plan Phase 3 Landing Pages. Video Sales Letter template. | LandingPageListForm (public) | High | No | Custom template not available in standard Anvil |
| **HomePageClassicTemplate** | ✅ Required | Explicitly required in build-plan Phase 4 Homepage. Classic homepage layout. | HomePage (public) | High | No | Custom template not available in standard Anvil |
| **HomePageServicesTemplate** | ✅ Required | Explicitly required in build-plan Phase 4 Homepage. Services-focused layout. | HomePage (public) | High | No | Custom template not available in standard Anvil |
| **HomePageBookingTemplate** | ✅ Required | Explicitly required in build-plan Phase 4 Homepage. Booking-focused layout. | HomePage (public) | High | No | Custom template not available in standard Anvil |
| **HomePageMinimalistTemplate** | ✅ Required | Explicitly required in build-plan Phase 4 Homepage. Minimalist layout. | HomePage (public) | High | No | Custom template not available in standard Anvil |
| **FooterComponent** | ✅ Required | Explicitly required in build-plan Phase 4 Public Pages. Footer with logo, social links, policies. | All public pages | High | No | Custom component not available in standard Anvil |
| **TimeLapseCarouselComponent** | ✅ Required | **ADDED TO V1** - Rotating service display on homepage. Essential for showcasing available services. | HomePage (public) | High | No | Standard Anvil FlowPanel insufficient for carousel functionality |
| **ParallaxComponent** | ✅ Required | **ADDED TO V1** - Hero image parallax effect on homepage. Essential for homepage hero section. | HomePage (public) | High | No | Standard CSS parallax insufficient for component reusability |
| **AnimatedButtonComponent** | ❌ Not Required | Standard Anvil M3 buttons already have built-in hover and focus animations. No custom animation needed. | All forms | High | Yes (Anvil Button) | Standard M3 button with role styling sufficient |
| **AnimatedCardComponent** | ❌ Not Required | Standard Anvil M3 cards already have built-in hover animations. No custom animation needed. | All forms | High | Yes (Anvil Card) | Standard M3 card with role styling sufficient |
| **AnimatedLinkComponent** | ❌ Not Required | Standard Anvil M3 links already have built-in hover animations. No custom animation needed. | All forms | High | Yes (Anvil Link) | Standard M3 link with role styling sufficient |
| **AnimatedImageComponent** | ❌ Not Required | Standard Anvil M3 images already have built-in hover animations. No custom animation needed. | All forms | High | Yes (Anvil Image) | Standard M3 image with role styling sufficient |

---

## Summary by Category

### Palette Forms (2 components)
| Component | Forms Using |
|-----------|-------------|
| PaletteSettingsForm | SettingsForm |
| OnboardingPaletteForm | Onboarding flow |

### Blog Templates (6 components)
| Component | Forms Using |
|-----------|-------------|
| BlogListTiledTemplate | BlogListForm (public) |
| BlogListListTemplate | BlogListForm (public) |
| BlogPostClassicTemplate | BlogPostForm (public) |
| BlogPostTwoColumnTemplate | BlogPostForm (public) |
| BlogPostFullWidthTemplate | BlogPostForm (public) |
| VideoDisplayComponent | BlogPostForm (public), HomePage (public) |

### Landing Page Templates (3 components)
| Component | Forms Using |
|-----------|-------------|
| LeadCaptureTemplate | LandingPageListForm (public) |
| EventRegistrationTemplate | LandingPageListForm (public) |
| VslTemplate | LandingPageListForm (public) |

### Homepage Templates (4 components)
| Component | Forms Using |
|-----------|-------------|
| HomePageClassicTemplate | HomePage (public) |
| HomePageServicesTemplate | HomePage (public) |
| HomePageBookingTemplate | HomePage (public) |
| HomePageMinimalistTemplate | HomePage (public) |

### Shared Components (3 components)
| Component | Forms Using |
|-----------|-------------|
| FooterComponent | All public pages |
| TimeLapseCarouselComponent | HomePage (public) |
| ParallaxComponent | HomePage (public) |

### Not Required (4 components)
| Component | Reason |
|-----------|--------|
| AnimatedButtonComponent | Standard M3 button sufficient |
| AnimatedCardComponent | Standard M3 card sufficient |
| AnimatedLinkComponent | Standard M3 link sufficient |
| AnimatedImageComponent | Standard M3 image sufficient |

---

## V1 Component Count

| Category | Count | Percentage |
|----------|-------|------------|
| **Palette Forms** | 2 | 11% |
| **Blog Templates** | 6 | 33% |
| **Landing Page Templates** | 3 | 17% |
| **Homepage Templates** | 4 | 22% |
| **Shared Components** | 3 | 17% |
| **Not Required** | 4 | 18% |
| **Total V1 Required** | **18** | **82%** |
| **Total Proposed** | **22** | **100%** |

---

## Key Findings

### Required Components (18)
- All explicitly required in build-plan
- All custom templates not available in standard Anvil
- VideoDisplayComponent added for multi-source video support
- TimeLapseCarouselComponent added for service rotation
- ParallaxComponent added for hero parallax effect

### Not Required Components (4)
- All are animated variants
- Standard Anvil M3 components provide sufficient functionality
- Role-based styling achieves same visual effects

### Critical Additions to V1

1. **VideoDisplayComponent** - Essential for blog posts with video content
   - Supports YouTube, Vimeo, and other video sources
   - Anvil's YouTube component is insufficient for all use cases

2. **TimeLapseCarouselComponent** - Essential for homepage service display
   - Rotating carousel for showcasing services
   - Standard Anvil FlowPanel insufficient for carousel functionality

3. **ParallaxComponent** - Essential for homepage hero section
   - Parallax scrolling effect for hero images
   - Standard CSS parallax insufficient for component reusability

---

## Implementation Notes

### Custom Component Implementation Requirements

All custom components must:
- Use the same token, typography, spacing, and role system defined in DESIGN.md
- Follow M3 component patterns where possible
- Be documented with clear usage instructions
- Include responsive behavior specifications
- Support accessibility (ARIA labels, keyboard navigation)

### Component Reuse Strategy

Components designed for reuse across multiple forms:
- **FooterComponent** - Used on all public pages
- **VideoDisplayComponent** - Used on blog posts and homepage
- **TimeLapseCarouselComponent** - Used on homepage templates
- **ParallaxComponent** - Used on homepage templates

### Component Isolation

Components designed for specific use cases:
- **PaletteSettingsForm** - Settings form only
- **OnboardingPaletteForm** - Onboarding flow only
- **Blog templates** - Blog listing and post pages only
- **Landing page templates** - Landing page forms only
- **Homepage templates** - Homepage only

---

## Scope Lock Verification

### Checklist

- [x] Every planned form appears in a canonical wireframe
- [x] Every planned form has a defined screen status
- [x] All wireframes are canonical
- [x] All screen files are canonical
- [x] Custom components in DESIGN.md are accounted for in scope
- [x] Custom components are reconciled against the planned forms and screens at reuse/dependency level
- [x] Implementation plans match the scope
- [x] Implementation plans match the planned wireframes and screens
- [x] ADRs do not conflict with docs
- [x] ADRs do not conflict with implementation plans
- [x] Docs do not conflict with implementation plans
- [x] No major planning question remains unresolved

### Scope Lock Status

**Scope locked = NO**

All custom components have been properly reconciled against the build-plan, wireframes, and architecture documents. The V1 scope is incomplete and not implementation-ready.


---

## Change History

| Date | Change | Reason |
|------|--------|--------|
| 2026-05-26 | Initial matrix created | Based on DESIGN.md proposal |
| 2026-05-27 | Added VideoDisplayComponent | Required for blog video content |
| 2026-05-27 | Added TimeLapseCarouselComponent | Required for homepage service rotation |
| 2026-05-27 | Added ParallaxComponent | Required for homepage hero parallax |
| 2026-05-27 | Removed Animated* components | Standard M3 sufficient |

---

*End of file — Custom Component Requirements Matrix*
