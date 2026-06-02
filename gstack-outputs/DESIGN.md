# Mybizz CS — DESIGN

**Status:** Reconciled canonical design system  
**Date:** 2026-06-02  
**Project:** Mybizz CS  
**Platform:** Anvil.works with Material Design 3 (M3)  
**Purpose:** Authoritative design system input for GStack workflows, especially `/design-shotgun` and `/design-html`.

---

## Authority and Source Documents

This file is the canonical design system document for the Mybizz CS project.
It exists to provide the single design reference that GStack expects as `DESIGN.md` in the workflow sequence.

This document is consolidated from the following approved design consultation outputs:

- `design-direction.md` — primary source for visual direction, tokens, typography, spacing, elevation, Anvil role map, and component styling rules.
- `design-consultation-plan-workflows.md` — source for workflow-specific component definitions, template inventory, implementation notes, schema additions, and design documentation expectations.
- `design-consultation-plan-descriprive.md` — source for workstream framing, palette strategy, blog template strategy, layout architecture, and client-facing customization model.

Where these source documents overlap, this file takes precedence as the authoritative consolidated reference.
If deeper implementation detail is needed, the source documents remain supporting references, but this file is the first design authority to consult.

### Reconciliation Notes (2026-06-02)

This file has been reconciled against:
- `sanity-audit-final-report.md` (2026-06-01)
- `plan-tune-verification-report.md` (2026-06-01)
- `wireframe-reconciliation-audit.md` (2026-06-01)
- `platform-overview.md` (current)
- `nomenclature.md` (current)

Key corrections applied:
- OnboardingPaletteForm removed from active inventory (obsolete).
- ClauseBuilder added as shared custom component.
- PaletteSettingsForm downgraded to local/form-specific.
- OnboardingForm, SettingsForm, PolicyEditorForm, PrivacyPolicyPage, TermsConditionsPage now explicitly covered.
- SettingsForm updated to 11-tab structure with RBAC governance.
- Onboarding/Settings architectural boundary reflected.
- Legal acknowledgement placed in onboarding scope.
- Payment gateway configuration placed in Settings scope.
- Mybizzmanagement treated as out-of-scope context only.
- privacy_policy and t_and_c tables added to schema expectations.

---

## Design Direction Summary

### Visual Direction: Clean Professional SaaS

Mybizz CS uses a refined, business-focused Material Design 3 system that emphasizes clarity, efficiency, trust, and professional polish.
The design must support consulting and services businesses such as salons, spas, therapists, consultants, and coaches.

The intended character is:

- Clean and structured, never playful or gimmicky.
- Professional and trustworthy, suitable for real business operations.
- Easy to scan and efficient in management contexts.
- Warm and inviting in public-facing contexts.
- Consistent across public pages, admin tools, onboarding, CRM, blog, bookings, and dashboard views.

### Core Principles

- Clarity over decoration.
- Consistency over experimentation.
- Reusable components over page-specific styling.
- Token-driven theming over hardcoded values.
- Layout variation through composition, not through parallel CSS systems.
- Public and management UI should feel related, but not identical in tone.

### Do Emulate

- Material 3 color roles and token-based hierarchy.
- Card-based content organization.
- Clear typography scale with predictable hierarchy.
- 4px spacing rhythm.
- Filled primary actions, outlined secondary actions, text-based tertiary actions.
- Left navigation rail on desktop and mobile-safe navigation patterns.
- Rounded but controlled corners.
- Elevated cards only where emphasis is needed.

### Do Not Emulate

- Bright gradients, flashy hero treatments, or decorative visual noise.
- Childish or overly playful interface language.
- Dark mode as the primary design target.
- Heavy animation, glassmorphism, or neumorphism.
- Complex charts or visual systems in V1 beyond what is operationally useful.
- Template-specific CSS divergence that breaks the shared design system.

---

## Dual-Context Product Tone

The product has two distinct but related presentation contexts.
Both contexts use one shared design system. The difference is in emphasis, density, and composition, not in building two separate design languages.

### Management UI

The management side should feel like an office tool.
Its principles are easy, obvious, and relevant.

Characteristics:

- Higher contrast and clearer hierarchy.
- Neutral surfaces with restrained primary-color emphasis.
- Clear affordances for actions, forms, filters, and status feedback.
- Slightly more compact and operationally efficient layouts.
- Obvious state changes for active, focused, disabled, and error elements.

### Public-Facing UI

The public side should feel more accommodating and engaging.
Its principles are comfort, trust, warmth, and approachability.

Characteristics:

- Softer visual rhythm and more generous whitespace.
- Palette-driven accents for brand expression.
- Clear CTAs with subtle elevation.
- Template flexibility without loss of component consistency.
- A more welcoming presentation suitable for websites, landing pages, and blog content.

### Consistency Rule

These two contexts must use the same underlying design system:

- Same typography family and scale.
- Same spacing rhythm.
- Same border radius rules.
- Same token structure.
- Same core components.
- Same role naming logic.

---

## Design Tokens

### Color System

The project follows a Material 3 token model.
All colors should resolve through Anvil theme tokens or documented CSS custom properties.
Hardcoded colors should be avoided unless explicitly approved.

#### Primary Palette

- Primary: `%color:Primary%`
- On Primary: `%color:On Primary%`
- Primary Container: `%color:Primary Container%`
- On Primary Container: `%color:On Primary Container%`

#### Secondary Palette

- Secondary: `%color:Secondary%`
- On Secondary: `%color:On Secondary%`
- Secondary Container: `%color:Secondary Container%`
- On Secondary Container: `%color:On Secondary Container%`

#### Neutral Palette

- Surface: `%color:Surface%`
- On Surface: `%color:On Surface%`
- Surface Variant: `%color:Surface Variant%`
- On Surface Variant: `%color:On Surface Variant%`
- Outline: `%color:Outline%`
- Background: `%color:Background%`
- On Background: `%color:On Background%`

#### Status Colors

- Error: `%color:Error%`
- On Error: `%color:On Error%`
- Disabled Container: `%color:Disabled Container%`
- On Disabled: `%color:On Disabled%`
- Light Overlay 1%: `%color:Light Overlay 1%`
- Light Overlay 2%: `%color:Light Overlay 2%`
- Dark Overlay 1%: `%color:Dark Overlay 1%`
- Dark Overlay 2%: `%color:Dark Overlay 2%`

#### Hardcoded Exception Policy

No hardcoded exception is currently approved as part of the baseline system.
If a hardcoded value becomes necessary, it must be documented explicitly in this file or in a supporting design decision note.

---

## Palette System

### Palette Strategy

The platform supports a unified palette system for client customization.
All palettes derive from the same M3 token structure.
Only the palette changes; the layout, typography, spacing, and interaction system remain fixed.

The palette system applies to:

- Primary button colors.
- Navigation active states.
- Accent surfaces and highlights.
- Link colors.
- Card emphasis and selected states where appropriate.

### Preset Palettes

#### Professional Blue

- Best for consultants, clinics, therapists.
- Intended effect: trust, professionalism, stability.

#### Warm Coral

- Best for salons, spas, lifestyle, beauty.
- Intended effect: warmth, creativity, approachability.

#### Calm Teal

- Best for fitness, wellness, physiotherapy, health services.
- Intended effect: health, balance, growth.

#### Sophisticated Purple

- Best for executive coaching, premium consulting, high-end services.
- Intended effect: premium, refined, exclusive.

### Palette Selector Requirements

The palette selector component should show:

- Primary color swatch.
- Sample filled button.
- Sample outlined button.
- Sample card treatment.
- Sample heading and link treatment.
- A clear label indicating target business fit.

### Palette Governance

Palette switching is a product customization concern and should remain token-driven.
Switching a palette must not require page-specific redesign or branching CSS logic.

---

## Typography

### Font Family

Primary family: Roboto  
Fallbacks: Noto Sans, Arial, sans-serif

### Type Scale

| Scale | Size | Line Height | Weight | Use Case |
|---|---:|---:|---:|---|
| Display | 57px | 64px | 400 | Rare hero headlines |
| Headline Large | 32px | 40px | 400 | Page titles |
| Headline Small | 22px | 28px | 500 | Section headers |
| Title | 18px | 24px | 500 | Card titles |
| Body Large | 14px | 20px | 400 | Larger body copy |
| Body | 14px | 20px | 400 | Standard body text |
| Button | 14px | 14px | 500 | Button labels |
| Label | 12px | 16px | 500 | Form labels |
| Caption | 12px | 16px | 400 | Helper text and captions |

### Typography Rules

- Use Roboto consistently across public and management surfaces.
- Preserve hierarchy through scale and weight, not decorative typography.
- Page titles should use Headline Large.
- Section headings should use Headline Small.
- Card titles and emphasized local headings should use Title.
- Body text should remain compact, readable, and operationally efficient.
- Labels and captions should never become the dominant visual element.

---

## Spacing, Radius, Elevation, and Density

### Spacing Scale

| Token | Value | Use Case |
|---|---:|---|
| `--space-1` | 4px | Tiny spacing, tight gaps |
| `--space-2` | 8px | Small spacing, padding |
| `--space-3` | 12px | Medium spacing |
| `--space-4` | 16px | Large spacing, card padding |
| `--space-6` | 24px | Section gaps |
| `--space-8` | 32px | Large layout spacing |

Rules:

- The 4px rhythm is mandatory.
- Padding, gaps, and section spacing should resolve through this scale.
- Public pages may feel looser than management screens, but must still use the same underlying rhythm.

### Border Radius Rules

| Element | Radius | Notes |
|---|---:|---|
| Buttons | 100px | Pill shape |
| Cards | 12px | Standard card shape |
| Dialogs | 28px | Modal/dialog treatment |
| Inputs (Outlined) | 4px | Text boxes and dropdowns |
| Inputs (Filled) | 0px | Bottom-border-only pattern |
| Chips/Badges | 8px | Tags and chips |
| Navigation Items | 100px | Pill treatment |
| Data Grid | 24px | Header and container |

### Elevation Rules

| Element | Elevation | Notes |
|---|---:|---|
| Surface | 0dp | Flat base |
| Card (Elevated) | 2dp | Emphasized content |
| Card (Tonal) | 0dp | Tonal separation only |
| Dialog | 24dp | Modal emphasis |
| Navigation Rail | 0dp | Border-only structure |
| App Bar | 0dp | Border-only structure |

### Density Guidance

| Density | Row Height | Padding | Gap | Use Case |
|---|---:|---:|---|---|
| Compact | 40px | 8px | 4px to 8px | Data-heavy screens |
| Default | 48px | 12px | 8px to 16px | Standard forms and lists |
| Comfortable | 56px | 16px | 12px to 24px | Read-only and public screens |

---

## Input and State Rules

### Input States

| State | Border Color | Background | Text Color | Notes |
|---|---|---|---|---|
| Default | Outline | Surface Variant | On Surface | Normal state |
| Focused | Primary | Surface Variant | On Surface | Active/focused |
| Error | Error | Surface Variant | Error | Validation failure |
| Disabled | On Disabled | Dark Overlay 1% | On Disabled | Reduced emphasis |
| Read-only | Outline | Surface | On Surface Variant | Non-editable display |

### State Rules

- Focus state must be obvious and consistent.
- Error state must be visible without becoming visually noisy.
- Disabled state must clearly communicate non-interactivity.
- Read-only state must not be confused with disabled state.
- Inputs across all templates must follow the same state logic.

---

## Component and Role Map

### Buttons

| Role Name | Appearance | Use Case | Min Height |
|---|---|---|---:|
| `primary-button` | Filled | Primary actions | 40px |
| `secondary-button` | Outlined | Secondary actions | 40px |
| `tertiary-button` | Text | Low-emphasis actions | 40px |
| `icon-button` | Icon only | Utility actions | 36px |
| `fab-button` | Filled circular | Create-new action | 56px |

### Cards

| Role Name | Card Type | Use Case |
|---|---|---|
| `surface-card` | Outlined | Standard content |
| `elevated-card` | Elevated | Emphasized content |
| `tonal-card` | Filled/tonal | Grouped content |
| `data-card` | Outlined | Data rows or lists |
| `metric-card` | Elevated | KPI and dashboard metrics |

### Containers

| Role Name | Use Case |
|---|---|
| `page-container` | Main content wrapper |
| `section-container` | Section grouping |
| `form-container` | Form grouping |
| `card-container` | Card collections |
| `grid-container` | Responsive grids |

### Headings

| Role Name | Size | Use Case |
|---|---|---|
| `page-title` | Headline Large | Page headings |
| `section-title` | Headline Small | Section headings |
| `card-title` | Title | Card headings |
| `label-title` | Body Large | Form labels |

### Navigation

| Role Name | Use Case |
|---|---|
| `nav-item` | Standard navigation item |
| `nav-item-selected` | Active navigation item |
| `app-bar-link` | App bar navigation |
| `breadcrumb-item` | Breadcrumb navigation |

### Forms

| Role Name | Input Type | Use Case |
|---|---|---|
| `outlined-input` | TextBox, TextArea | Standard text input |
| `outlined-select` | DropdownMenu | Select input |
| `outlined-date` | DatePicker | Date selection |
| `input-error` | All inputs | Error state |
| `input-disabled` | All inputs | Disabled state |
| `input-readonly` | All inputs | Read-only state |

### Component Consistency Rule

All templates must reuse the same underlying components and role rules.
Template variation is allowed through arrangement, visibility, and composition, but not through design-system divergence.

---

## Forms and Pages

This section defines the primary implementation-facing forms and pages in the Mybizz CS application. Each entry states its purpose, architectural boundary, and key design constraints.

### OnboardingForm

**Purpose:** Guided first-run experience for new Owners.  
**Architecture:** Onboarding is a separate form, not part of Settings.  
**Wireframe:** `wireframe-onboarding-OnboardingForm.html` (created).

**Flow structure (6 steps):**
1. Welcome and orientation.
2. Business Profile — links to Settings Business Profile tab for detail editing.
3. Palette Selection — links to Settings Palette tab for detail editing.
4. System Currency — dropdown plus confirm button. Required. Immutable after first transaction. See ADR-016.
5. Legal Acknowledgement — text, checkbox, confirm button. Required. Logged in Mybizz_management. See ADR-018.
6. Completion — onboarding complete.

**Key design rules:**
- Progress indicator must be visible throughout.
- Bidirectional navigation (back and forward) between steps.
- "Go to Settings" links open the corresponding Settings tab and return to onboarding when done.
- Legal acknowledgement is a required action in onboarding, not a mutable Settings configuration.
- Payment gateway configuration is not part of onboarding. Onboarding links to Settings for gateway setup. See ADR-019.
- Onboarding state is tracked in `business_profile.onboarding_status`. See ADR-023, ADR-025.

### SettingsForm

**Purpose:** Single tabbed form for all post-onboarding configuration.  
**Architecture:** Settings is a single form with tab-based navigation using M3 buttons.  
**Wireframe:** `wireframe-settings-SettingsForm.html` (recreated with 11 tabs).

**Tab structure (11 tabs):**

| Tab | Content | RBAC Visibility |
|---|---|---|
| Business Profile | Name, email, phone, address, timezone, entity type | Owner, Manager |
| Palette | Brand colour selection (PaletteSettingsForm — local component) | Owner, Manager |
| System Currency | Currency display (locked after first transaction) | Owner only |
| Legal Acknowledgement | Read-only view of legal acknowledgements | Owner only |
| Display Currency | Optional per-service currency override | Owner, Manager |
| Payment Gateway | Stripe/Paystack configuration (API keys in Vault) | Owner only |
| Services | Service catalogue management | Owner, Manager, Admin |
| Users & Permissions | User management and RBAC | Owner only |
| Email Configuration | Brevo SMTP and API settings | Owner only |
| Theme | M3 theme customisation | Owner, Manager |
| Feature Flags | Feature activation toggles | Owner only |

**Key design rules:**
- Tab visibility is RBAC-governed. Different roles see different tabs.
- "Return to Onboarding" button in header — visible only when onboarding is in progress.
- Vault access (within Payment Gateway tab) requires TOTP step-up per ADR-03.
- Legal Acknowledgement tab is read-only. It displays acknowledgement history. The acknowledgement action itself belongs to onboarding.
- Payment gateway configuration is a Settings concern, not an onboarding concern. See ADR-019.
- PaletteSettingsForm is a local/form-specific component used only within the Palette tab.

### PolicyEditorForm

**Purpose:** Settings-hosted screen for editing Privacy Policy and Terms & Conditions using the ClauseBuilder component.  
**Architecture:** Accessed from Settings. Uses ClauseBuilder as a shared custom component.  
**Wireframe:** `wireframe-settings-PolicyEditorForm.html` (created).

**Key design rules:**
- Contains a document type selector (Privacy Policy vs Terms & Conditions).
- Embeds the ClauseBuilder component, parameterized by document type.
- Publish/update button writes to `privacy_policy` or `t_and_c` table and publishes to the corresponding public page.
- See ADR-018.

### ClauseBuilder (Shared Custom Component)

**Purpose:** Standalone reusable clause editor for legal policy documents.  
**Architecture:** Shared custom component used by Privacy Policy and Terms & Conditions editing flows.  
**Wireframe:** `wireframe-custom-component-ClauseBuilder.html` (created).

**Key design rules:**
- Parameterized by document type (`privacy_policy` or `t_and_c`).
- Category dropdown selector for clause grouping.
- Seeded clauses from master template (green border, editable text, not deletable).
- Custom clauses added by Owner (blue border, editable and deletable).
- "+ Add Custom Clause" button reveals add form.
- "Edit" link on each clause reveals edit form.
- "Save" button writes changes to datatable and publishes to public page.
- Has its own data model: `category`, `title`, `clause`, `sort_order`, `is_seeded`, `is_active`.
- Used in 2+ places (PrivacyPolicyEditor, TermsConditionsEditor). Qualifies as shared.

### PrivacyPolicyPage

**Purpose:** Public-facing page that renders the Owner's privacy policy.  
**Architecture:** Reads from `privacy_policy` table. Renders clauses grouped by category.  
**Wireframe:** To be created.

**Key design rules:**
- Uses standard public page layout with FooterComponent.
- Clauses rendered in category order, then sort_order within category.
- Only active clauses (`is_active = True`) are displayed.
- Follows public-facing UI tone (comfortable density, generous whitespace).

### TermsConditionsPage

**Purpose:** Public-facing page that renders the Owner's terms and conditions.  
**Architecture:** Reads from `t_and_c` table. Renders clauses grouped by category.  
**Wireframe:** To be created.

**Key design rules:**
- Uses standard public page layout with FooterComponent.
- Clauses rendered in category order, then sort_order within category.
- Only active clauses (`is_active = True`) are displayed.
- Follows public-facing UI tone (comfortable density, generous whitespace).

---

## Template and Layout Architecture

### Architecture Principle

Layouts are shells; content panels are swappable templates.
The layout system should support variation without duplicating the design system.

### Content Panel Pattern

```text
Public Layout (HtmlTemplate)
├── App Bar
├── Navigation
└── ContentPanel
    ├── HomePageTemplate
    ├── BlogListTemplate
    ├── BlogPostTemplate
    ├── AboutPageTemplate
    ├── ContactPageTemplate
    └── ServicePageTemplate
```

### Template Governance

- Templates must share the same tokens, roles, and interaction rules.
- Template differences should be structural and compositional.
- UI improvements to a shared component should propagate across all templates.
- Avoid template-specific CSS that breaks reuse.

### Homepage Template Variants

Supported homepage variants:

- Classic
- Services
- Booking
- Minimalist

These are variation patterns built on the same design system, not separate site themes.

---

## Blog Design System

### Blog List Views

Supported public-facing list patterns:

#### Tiled Grid View

- Multi-column grid on desktop.
- Card-based presentation.
- Featured image, category badge, title, excerpt, metadata, and read-more action.
- Intended for browse-friendly visual scanning.

#### List with Thumbnail View

- Row-based layout with image and content split.
- Better for denser reading and editorial scan patterns.
- Supports alternating row backgrounds where appropriate.

### Blog Post Templates

Supported post templates:

#### Classic Centered

- Centered single-column article layout.
- Best for long-form editorial content.

#### Two-Column with Sidebar

- Main article plus sidebar.
- Sidebar may include author bio, related posts, categories, and newsletter content.
- Best for engagement and internal linking.

#### Full-Width Immersive

- Full-width visual treatment.
- Best for image-rich or storytelling content.
- Pullquotes and generous line height may be used.

### Blog Admin Controls

Blog settings should support:

- Default blog list view.
- Per-post template selection.
- Comment moderation controls.
- SEO meta inputs.
- Related post strategy by tags.

---

## Custom Components Inventory

### Shared Custom Components (6)

| Component | Where Used | Justification |
|---|---|---|
| VideoDisplayComponent | BlogPostForm, HomePage | Multi-source video support not in native M3. Supports YouTube (V1). |
| FooterComponent | All public pages (6+) | Custom footer with logo, credentials, social links, policies, map, contact. Not in native M3. |
| TimeLapseCarouselComponent | HomePage, ServicesForm | Auto-rotating carousel not in native M3. |
| ParallaxComponent | Multiple public pages | Parallax scroll effect not in native M3. |
| ClauseBuilder | PrivacyPolicyEditor, TermsConditionsEditor | Reusable clause editor parameterized by document type. Used in 2+ places. Not in native M3. |
| BlogListTiledTemplate | Public blog listing | Tiled grid blog list view. Not in native M3. |
| BlogListListTemplate | Public blog listing | List-with-thumbnail blog list view. Not in native M3. |
| BlogPostClassicTemplate | Public blog post | Classic centered blog post layout. |
| BlogPostTwoColumnTemplate | Public blog post | Two-column blog post layout with sidebar. |
| BlogPostFullWidthTemplate | Public blog post | Full-width immersive blog post layout. |
| HomePageClassicTemplate | Public homepage | Classic homepage variant. |
| HomePageServicesTemplate | Public homepage | Services-focused homepage variant. |
| HomePageBookingTemplate | Public homepage | Booking-focused homepage variant. |
| HomePageMinimalistTemplate | Public homepage | Minimalist homepage variant. |
| LeadCaptureTemplate | Landing pages | Lead capture landing page. |
| EventRegistrationTemplate | Landing pages | Event registration landing page. |
| VslTemplate | Landing pages | Video sales letter landing page. |

### Local/Form-Specific Components (1)

| Component | Where Used | Reason |
|---|---|---|
| PaletteSettingsForm | SettingsForm Palette tab only | Only used in one form. Does not meet shared criteria. |

### Removed from V1 Scope

| Component | Reason |
|---|---|
| OnboardingPaletteForm | Obsolete. Onboarding palette selection is now part of OnboardingForm. |
| AnimatedButtonComponent | Standard M3 components provide sufficient hover/focus animations via role-based styling. |
| AnimatedCardComponent | Standard M3 components provide sufficient hover/focus animations via role-based styling. |
| AnimatedLinkComponent | Standard M3 components provide sufficient hover/focus animations via role-based styling. |
| AnimatedImageComponent | Standard M3 components provide sufficient hover/focus animations via role-based styling. |

All custom components must remain governed by the same token, typography, spacing, and role system defined in this document.

---

## Motion and Interaction Guidance

Motion in V1 should remain subtle and functional.
It should support clarity and polish, not spectacle.

Rules:

- Avoid decorative animation with no product purpose.
- Use motion for transitions, emphasis, reveal, or orientation only where it improves comprehension.
- Public-facing templates may use subtle micro-interactions.
- Management UI should remain restrained and operational.
- Animated custom components must still respect M3 clarity and professional tone.

Examples of acceptable motion:

- Subtle hover and press feedback on buttons.
- Gentle carousel transitions.
- Minimal parallax effects where they do not impair readability.
- Progressive reveal for public-facing sections when appropriate.

Examples to avoid:

- Bouncy or playful transitions.
- Large animated gradients.
- Heavy page-load effects.
- Motion that interferes with speed or focus.

---

## Implementation Notes

### Styling Implementation

All styling should use:

- Anvil M3 theme tokens.
- Built-in M3 component properties.
- Anvil roles for semantic styling.
- CSS custom properties defined in `theme.css` where needed.

### JavaScript Usage

JavaScript should remain minimal and purposeful.
Approved uses include:

- Palette application.
- Parallax scroll effects.
- Social sharing interactions.
- Character counters for SEO fields.
- Back-to-top interactions.

### Performance Guidance

- Use lazy loading for comments and other deferred content.
- Optimize image sizes and dimensions.
- Keep template switching lightweight.
- Avoid unnecessary runtime complexity in presentation layers.

### Storage Guidance

Client-side palette caching may be used where appropriate, but implementation should remain predictable and easy to reason about.

---

## Design-Related Schema Expectations

The design consultation established schema expectations tied to presentation and customization.
These should be treated as approved design-support requirements, subject to alignment with the authoritative schema and implementation plan.

### business_profile additions

Expected fields include:

- `palette`
- `palette_updated_at`
- `blog_list_view`
- `enable_comments`
- `homepage_template`
- `business_logo`
- `accreditations`
- `social_facebook`
- `social_instagram`
- `social_linkedin`
- `social_twitter`
- `google_maps_embed_url`
- `contact_email`
- `contact_phone`

### blog_posts additions

Expected fields include:

- `template`
- `meta_title`
- `meta_description`
- `keywords`
- `pullquote`

### blog_comments table

Expected fields include:

- `instance_id`
- `post_id`
- `commenter_name`
- `commenter_email`
- `comment_text`
- `status`
- `created_at`
- `updated_at`

### privacy_policy table

Expected fields include:

- `instance_id`
- `category`
- `title`
- `clause`
- `sort_order`
- `is_seeded`
- `is_active`
- `created_at`
- `updated_at`

### t_and_c table

Expected fields include:

- `instance_id`
- `category`
- `title`
- `clause`
- `sort_order`
- `is_seeded`
- `is_active`
- `created_at`
- `updated_at`

---

## Mybizz_management Context

Mybizz_management is **out of scope** for this project. The following is recorded as architectural context only. Mybizz_management is a separate Anvil application for platform operator tooling — client provisioning, health monitoring, billing automation, and update distribution. It is not part of master_template and is built only after the master template reaches launch.

Relevant architectural context is documented in `platform-overview.md` section 17.

---

## GStack Workflow Usage

This file exists specifically to satisfy the workflow requirement that `/design-consultation` produces `DESIGN.md`.
For GStack purposes, this file should be treated as the design-system checkpoint output.

### Upstream Position

This file depends on:

- completed `/office-hours`
- completed `/plan-ceo-review`
- completed `/design-consultation`

### Downstream Use

This file is the design authority for:

- wireframe updates where styling implications matter
- `/design-shotgun`
- `/design-html`
- downstream design review and UI implementation decisions

### Operational Rule

Do not rerun `/design-consultation` merely because the original run did not create a file literally named `DESIGN.md`.
If the consultation outputs exist and are approved, consolidate them here and continue.
Only rerun `/design-consultation` if scope, direction, or design requirements materially changed.

---

## Maintenance Rule

This document should be updated when any of the following change:

- visual direction
- design tokens
- palette system
- typography scale
- spacing or radius rules
- template architecture
- component roles
- approved custom components
- design-related schema requirements
- form or page boundaries
- onboarding or settings architecture

When updated, this file remains the canonical design reference and supporting documents remain secondary references.

---

## Source Reference Mapping

This section records how the source files map into this consolidated design authority.

| Source file | Primary contribution to this document |
|---|---|
| `design-direction.md` | Core visual direction, tokens, spacing, typography, elevations, role map |
| `design-consultation-plan-workflows.md` | Workflow details, custom components, schema additions, implementation notes |
| `design-consultation-plan-descriprive.md` | Workstreams, palette model, blog template strategy, layout architecture |

---

## Final Instruction for GStack and OpenCode

Treat `DESIGN.md` as the authoritative design system document for Mybizz CS.
If a downstream command requires the design consultation output, use this file first.
Use the supporting design consultation documents only for deeper detail where needed.
