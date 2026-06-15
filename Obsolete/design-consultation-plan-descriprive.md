# Design Consultation Plan — Mybizz CS (Descriptive)

**Status:** Reconciled  
**Date:** 2026-06-02  
**Reconciliation:** Updated against sanity-audit-final-report.md, plan-tune-verification-report.md, and DESIGN.md (2026-06-02).

---

## Executive Summary

This plan addresses four key design workstreams for the Mybizz CS platform:

1. **Design System Foundation** — Unified M3-based system for public and private contexts.
2. **Palette Selection** — 4 preset colour palettes for client customization. Palette selection is part of the OnboardingForm flow and the SettingsForm Palette tab.
3. **Blog Enhancement** — Tiled/list views and blog page template variants.
4. **Layout and Template Architecture** — Content panel pattern for UI variation without code duplication.

### Architecture Boundary

- Onboarding is a separate form (OnboardingForm), not part of Settings.
- Settings is a single tabbed form (SettingsForm) with 11 RBAC-governed tabs.
- Bidirectional navigation exists between Onboarding and Settings.
- Legal acknowledgement belongs to onboarding, not to mutable Settings configuration.
- Payment gateway configuration is a Settings concern, not an onboarding concern. See ADR-019.
- ClauseBuilder is a standalone shared custom component used by Privacy Policy and Terms and Conditions editing flows. See ADR-018.

---

## Workstream 1: Design System Foundation

### 1.1 Dual-Tone Design Direction

#### Management UI (Office Feel)

- **Principle:** Easy, Obvious, Relevant
- **Visual language:** Higher contrast, clearer hierarchy, obvious interactive elements.
- **Colour usage:** Neutral surfaces, primary colours for actions only.
- **Typography:** Roboto with clear weight differentiation.
- **Spacing:** Consistent 4px rhythm, slightly more generous padding for readability.
- **Components:** Filled buttons for primary actions, outlined for secondary, clear disabled states.

#### Public-Facing UI (Alluring and Accommodating)

- **Principle:** Comfort, Engagement, Warmth
- **Visual language:** Softer edges, warmer tones, inviting whitespace.
- **Colour usage:** Palette-driven, more expressive use of primary/secondary containers.
- **Typography:** Same Roboto scale but with warmer line heights.
- **Spacing:** More generous, breathing room between sections.
- **Components:** Larger CTAs, subtle elevations, inviting micro-interactions.

Both contexts use one shared design system. The difference is in emphasis, density, and composition, not in building two separate design languages.

### 1.2 Unified Design Tokens

All palettes derive from the same M3 token structure:

- **Primary Palette** — Changes per palette.
- **Secondary Palette** — Derives from primary.
- **Neutral Palette** — Fixed (surface, background, outline).
- **Typography** — Fixed (Roboto scale).
- **Spacing** — Fixed (4px rhythm).
- **Border Radius** — Fixed (12px cards, 100px buttons).

---

## Workstream 2: Palette Selection

### 2.1 Four Preset Palettes

| Palette Name | Primary Hue | Best For | Psychological Profile |
|---|---|---|---|
| Professional Blue | Blue (#0D6995) | Consultants, therapists, clinics | Trust, stability, professionalism |
| Warm Coral | Orange/Pink (#E85A4F) | Salons, spas, beauty, lifestyle | Warmth, creativity, approachability |
| Calm Teal | Teal/Green (#2E7D6D) | Fitness trainers, wellness coaches, physiotherapists | Balance, health, growth |
| Sophisticated Purple | Deep Purple (#6750A4) | High-end consultants, executive coaches, luxury services | Premium, exclusive, refined |

### 2.2 Palette Application Points

Palette selection occurs in two places:

1. **OnboardingForm** — Palette selection is one of the 6 onboarding steps. The Owner selects a palette during initial setup. Links to the Settings Palette tab for detail editing.
2. **SettingsForm Palette tab** — Post-onboarding palette switching. Local component PaletteSettingsForm is embedded in this tab.

Palette applies to:
- Primary button colours.
- Navigation rail active states.
- Card container backgrounds.
- Link colours.
- Accent elements (badges, highlights).

### 2.3 Palette Preview Component

**PaletteSettingsForm** (local/form-specific component, used only in SettingsForm Palette tab):

- Primary colour swatch (large).
- Sample button (filled plus outlined).
- Sample card (tonal background).
- Sample text (links, headings).
- Business type icons (salon, clinic, consulting, fitness).

**Note:** OnboardingPaletteForm is obsolete. Palette selection during onboarding is handled within the OnboardingForm itself.

---

## Workstream 3: Blog Enhancement

### 3.1 Blog List View Variants

Current wireframe uses: DataGrid view (administrative).

New public-facing options:

#### Option A: Tiled Grid View

- 3-column grid (desktop), 2-column (tablet), 1-column (mobile).
- Each card shows: featured image (16:9 aspect ratio), category badge, post title (headline-small), excerpt (2-3 lines, body-medium), meta (author, date, view count), "Read more" link (tertiary button).
- Spacing: 16px gaps, 24px section padding.

#### Option B: List with Thumbnail View

- 2-column layout (image left, content right).
- Each row shows: featured image (240px x 160px), category badge, post title (title-medium), excerpt (3-4 lines, body-medium), meta (author, date).
- Alternating row background (surface vs surface-variant).
- Spacing: 16px between rows, 24px section padding.

Admin control: Settings tab for Blog Settings with "Default list view" dropdown (Tiled | List).

### 3.2 Blog Post Page Templates

#### Template A: Classic Centered

- Hero: Title (headline-large), meta (body-medium), featured image (full-width, 800px max).
- Content: Single column, max 720px width, centred.
- Sidebar: None.
- Best for: Long-form, editorial content.

#### Template B: Two-Column with Sidebar

- Hero: Title (headline-medium), meta (body-small), featured image (300px x 200px, left).
- Content: Main column (60%), sidebar (40%).
- Sidebar widgets: Author bio, related posts (3), category list, newsletter signup.
- Best for: Engagement, internal linking.

#### Template C: Full-Width Immersive

- Hero: Title (headline-large), full-width featured image (1200px), meta centred over image.
- Content: Full-width, max 900px, generous line height (1.8).
- Pullquotes: Full-width, italic, primary container background.
- Best for: Visual storytelling, photography-heavy posts.

Admin control: When creating/editing a blog page, dropdown "Select blog template" (Classic | Two-Column | Full-Width). Per-page template selection (not global default).

---

## Workstream 4: Layout and Template Architecture

### 4.1 Content Panel Pattern

Architecture principle: Layouts are shells; content panels are swappable templates.

Implementation:

```text
Public Layout (HtmlTemplate)
├── App Bar (fixed)
├── Navigation (fixed)
└── ContentPanel (dynamic)
    ├── HomePageTemplate
    ├── BlogListTemplate (tiled or list)
    ├── BlogPostTemplate (classic/two-column/full-width)
    ├── AboutPageTemplate
    ├── ContactPageTemplate
    └── ServicePageTemplate
```

Client selection flow:

- Dashboard > Settings > Website configuration.
- "Homepage template" dropdown > shows 4 options with previews.
- "Blog list view" dropdown > Tiled | List.
- "Blog post template" dropdown > Classic | Two-Column | Full-Width.
- Save > changes apply immediately (no code deployment).

### 4.2 Component Consistency Rule

All templates use identical underlying components, just arranged differently:

| Component | Reused Across All Templates |
|---|---|
| HeroSection | Yes (varies: full-width vs contained) |
| Card | Yes (tiled view uses cards, two-column uses cards) |
| Button | Yes (all button variants) |
| Heading | Yes (all heading scales) |
| Text | Yes (all body scales) |
| Image | Yes (all image displays) |
| Footer | Yes (identical across all pages) |

Benefit: UI changes propagate to all templates automatically. No template-specific CSS.

---

## Workstream 5: Legal Policy and Clause Builder

### 5.1 ClauseBuilder Architecture

The ClauseBuilder is a **standalone shared custom component** — not embedded in each PolicyEditorForm.

Rationale:
- Used in 2+ places (Privacy Policy editor, Terms and Conditions editor).
- Parameterized with document type (`privacy_policy` or `t_and_c`).
- Has its own data model (category, title, clause, sort_order, is_seeded, is_active).
- Has its own UI interactions (select from seeded list, add custom, organize by category).
- Easier to test, maintain, and reuse as a standalone component.

### 5.2 Data Model

Two tables in the authoritative schema:
- `privacy_policy` — clause data for the Owner's privacy policy page.
- `t_and_c` — clause data for the Owner's terms and conditions page.

Both tables share the same structure: `instance_id`, `category`, `title`, `clause`, `sort_order`, `is_seeded`, `is_active`, `created_at`, `updated_at`.

### 5.3 Policy Editor Flow

1. Owner navigates to Settings > Policy Editor.
2. Selects document type (Privacy Policy or Terms and Conditions).
3. ClauseBuilder loads seeded clauses for the selected document type.
4. Owner may edit seeded clauses, add custom clauses, reorder by category.
5. Save writes to the corresponding datatable and publishes to the public page.

### 5.4 Public Pages

- **PrivacyPolicyPage** — reads from `privacy_policy` table, renders clauses.
- **TermsConditionsPage** — reads from `t_and_c` table, renders clauses.
- Both use standard public page layout with FooterComponent.
- Only active clauses (`is_active = True`) are displayed.

---

## Deliverables

### Phase 1: Design System Foundation

- Define 4 colour palettes in theme.css as M3 tokens.
- Create palette preview component (PaletteSettingsForm — local).
- Document palette application points in design-direction.md.
- Create dual-tone styling guidelines (management vs public).

### Phase 2: Blog Enhancement

- Design tiled blog list template (BlogListTiledTemplate).
- Design list-with-thumbnail blog template (BlogListListTemplate).
- Design 3 blog post page templates (Classic, Two-Column, Full-Width).
- Add blog view/template settings to Settings form.
- Update wireframe-blog.html with new variants.

### Phase 3: Layout Architecture

- Document content panel pattern in design-direction.md.
- Create homepage template variants (reuse existing 4, document pattern).
- Implement palette switching logic (onboarding plus settings).

### Phase 4: Legal Policy System

- Implement ClauseBuilder as standalone shared component.
- Create PolicyEditorForm screen.
- Create PrivacyPolicyPage and TermsConditionsPage public pages.
- Add `privacy_policy` and `t_and_c` tables to authoritative schema.

### Phase 5: Documentation and Handoff

- Update design-direction.md with all new templates.
- Create blog-templates.md with detailed specifications.
- Create palette-system.md with token definitions.
- Wireframe review with user (validate all variants).

---

## Open Questions

All original questions have been resolved through the reconciliation process:

1. Palette switching: Owners may switch palettes freely from the Settings Palette tab.
2. Homepage templates: All 4 homepage templates (Classic, Services, Booking, Minimalist) are included in the palette design scope.
3. Blog post template default: Per-page template selection (not global default).
4. Admin control depth: Beyond palette and blog templates, clients have logo upload, accreditations, social links, Google Maps embed, and footer contact details.

---

*End of design-consultation-plan-descriprive.md — 2026-06-02*
