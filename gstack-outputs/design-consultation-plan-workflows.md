# Mybizz CS — Design Consultation Plan (Workflows)

**Status:** Reconciled  
**Date:** 2026-06-02  
**Vertical:** Consulting & Services  
**Platform:** Anvil.works with Material Design 3 (M3)  
**Reconciliation:** Updated against sanity-audit-final-report.md, plan-tune-verification-report.md, and DESIGN.md (2026-06-02).

---

## Executive Summary

This plan delivers a unified Material Design 3 design system for the Mybizz CS platform, addressing all user requirements while strictly adhering to Anvil M3 component constraints. The plan focuses on:

1. **Four preset colour palettes** with on-demand selection
2. **Enhanced blog system** with tiled/list views and template selection
3. **Custom M3 components** (carousel, parallax, clause builder)
4. **Footer bar** with all required elements
5. **Improved homepage templates**
6. **Blog comments system** with manual moderation
7. **Unified design language** across public and management interfaces
8. **Legal policy system** with ClauseBuilder component and public policy pages

**Key Decisions:**
- VideoDisplayComponent supports YouTube only (V1).
- Comments use manual moderation (no email verification for V1).
- Blog view counts tracked via Anvil's built-in analytics.
- SEO meta tags implemented in simple, obvious manner.
- Social sharing buttons included on all blog posts.
- Related posts determined by tag matching.
- ClauseBuilder is a standalone shared custom component used by Privacy Policy and Terms & Conditions editing flows.
- PaletteSettingsForm is local/form-specific (SettingsForm Palette tab only).
- OnboardingPaletteForm is obsolete — palette selection is part of OnboardingForm.
- AnimatedButtonComponent, AnimatedCardComponent, AnimatedLinkComponent, AnimatedImageComponent are removed from V1 scope.
- Onboarding is a separate form (OnboardingForm), not part of Settings.
- Settings is a single tabbed form (SettingsForm) with 11 RBAC-governed tabs.
- Legal acknowledgement belongs to onboarding, not to mutable Settings configuration.
- Payment gateway configuration is a Settings concern. See ADR-019.

---

## Workstream 1: Palette System

### Stage 1: Palette Definition

#### Task 1.1: Define Four M3-Compatible Palettes

**Location:** `Assets/theme.css`

**Palette Specifications:**

| Palette ID | Name | Primary Hex | Best For | Psychological Profile |
|------------|------|-------------|----------|----------------------|
| `palette_professional` | Professional Blue | `#0D6995` | Consultants, therapists, clinics | Trust, stability, professionalism |
| `palette_warm` | Warm Coral | `#E85A4F` | Salons, spas, beauty, lifestyle | Warmth, creativity, approachability |
| `palette_calm` | Calm Teal | `#2E7D6D` | Fitness trainers, wellness, physiotherapists | Balance, health, growth |
| `palette_sophisticated` | Sophisticated Purple | `#6750A4` | High-end consultants, executive coaches, luxury | Premium, exclusive, refined |

**CSS Token Structure (theme.css):**

```css
/* Palette: Professional Blue */
:root[data-palette="professional"] {
  --app-color-primary: #0D6995;
  --app-color-on-primary: #FFFFFF;
  --app-color-primary-container: #A3E4FC;
  --app-color-on-primary-container: #001F2E;
  --app-color-secondary: #675B70;
  --app-color-secondary-container: #E9DEFD;
  --app-color-tertiary: #625B71;
  --app-color-background: #FFFBFE;
  --app-color-surface: #FFFBFE;
  --app-color-surface-variant: #E7E0EC;
  --app-color-outline: #79747E;
}

/* Palette: Warm Coral */
:root[data-palette="warm"] {
  --app-color-primary: #E85A4F;
  --app-color-on-primary: #FFFFFF;
  --app-color-primary-container: #FFDAD6;
  --app-color-on-primary-container: #571510;
  --app-color-secondary: #775754;
  --app-color-secondary-container: #FFDAD6;
  --app-color-tertiary: #75594D;
  --app-color-background: #FFFBFE;
  --app-color-surface: #FFFBFE;
  --app-color-surface-variant: #F3E2E0;
  --app-color-outline: #8C7370;
}

/* Palette: Calm Teal */
:root[data-palette="calm"] {
  --app-color-primary: #2E7D6D;
  --app-color-on-primary: #FFFFFF;
  --app-color-primary-container: #A6F4E3;
  --app-color-on-primary-container: #00201A;
  --app-color-secondary: #5F6B62;
  --app-color-secondary-container: #E2EAD3;
  --app-color-tertiary: #576358;
  --app-color-background: #FFFBFE;
  --app-color-surface: #FFFBFE;
  --app-color-surface-variant: #E1E6DD;
  --app-color-outline: #73796F;
}

/* Palette: Sophisticated Purple */
:root[data-palette="sophisticated"] {
  --app-color-primary: #6750A4;
  --app-color-on-primary: #FFFFFF;
  --app-color-primary-container: #EADDFF;
  --app-color-on-primary-container: #21005D;
  --app-color-secondary: #625B71;
  --app-color-secondary-container: #E8DEFD;
  --app-color-tertiary: #7D5260;
  --app-color-background: #FFFBFE;
  --app-color-surface: #FFFBFE;
  --app-color-surface-variant: #E7E0EC;
  --app-color-outline: #79747E;
}
```

**Implementation Notes:**
- All 4 palettes defined as CSS custom properties.
- Palette selector sets `data-palette` attribute on `:root` element via JavaScript.
- All components reference tokens via `%color:Token%` syntax in Designer.
- Zero hardcoded hex values in component code.

#### Task 1.2: Palette Settings Screen

**Location:** SettingsForm > Palette tab

**Component Specification:**

**Form Name:** `PaletteSettingsForm` (local/form-specific component — used only in SettingsForm Palette tab)

**Anvil Components Used:**
- `Heading` (headline-small) — Text: "Color Palette"
- `Text` (body-medium) — Text: "Choose a color scheme that reflects your business"
- `ColumnPanel` — Main container (padding: 24px)
  - `FlowPanel` — Palette selector row (horizontal orientation, gap: 16px)
    - `Button` (4 instances) — One per palette
      - Properties:
        - appearance: `outlined`
        - min-height: `48px`
        - role: `palette-button`
        - Text: "Professional Blue" / "Warm Coral" / "Calm Teal" / "Sophisticated Purple"
        - icon: `palette` (same icon for all)
  - `ColumnPanel` — Preview content panel (id: `preview_panel`, visible: `False` initially)
    - `Card` (elevated, border-radius: 12px)
      - `CardContentContainer` — Inner content
        - `Heading` (title-medium) — Dynamic: Palette name
        - `Text` (body-medium) — Dynamic: "Best for [business type]"
        - `FlowPanel` — Preview components
          - `Button` (filled) — Sample primary button
          - `Button` (outlined) — Sample secondary button
          - `Card` (tonal-card) — Sample card with text
        - `Button` (filled, role: `primary-button`) — Text: "Apply Palette"
  - `Text` (caption) — Text: "Changes apply immediately across all pages"

**Note:** This is a local component. It does not qualify as shared because it is used only in the SettingsForm Palette tab.

#### Task 1.3: Onboarding Palette Selection

**Location:** OnboardingForm — step 3 of 6

Palette selection during onboarding is handled within the OnboardingForm itself. There is no separate OnboardingPaletteForm. The onboarding palette step reuses the same palette preview logic as PaletteSettingsForm but is embedded directly in the onboarding flow.

**OnboardingForm Palette Step:**
- `Heading` (headline-large) — Text: "Choose Your Color Palette"
- `Text` (body-large) — Text: "Select a color scheme that fits your business"
- Palette selector buttons (same 4 palettes as SettingsForm).
- Preview panel with sample components.
- `Button` (filled) — Text: "Continue with This Palette"
- `Text` (caption) — Text: "You can change this later in Settings"

**Note:** OnboardingPaletteForm is obsolete. It has been removed from the active component inventory.

#### Task 1.4: Client-Side Palette Application

**Location:** `Assets/theme.js` (or inline script in standard-page.html)

**JavaScript Implementation:**

```javascript
// Apply palette from business_profile
function applyPalette(paletteId) {
  document.documentElement.setAttribute('data-palette', paletteId);
  localStorage.setItem('mybizz_palette', paletteId);
}

// Load saved palette on page load
function loadSavedPalette() {
  const savedPalette = localStorage.getItem('mybizz_palette');
  if (savedPalette) {
    applyPalette(savedPalette);
  }
}

// Initialize on page load
document.addEventListener('DOMContentLoaded', function() {
  loadSavedPalette();
});

// Export for server calls
anvil.js.addLinkHandler('applyPalette', function(paletteId) {
  applyPalette(paletteId);
});
```

---

### Stage 2: Palette Integration

#### Task 2.1: Palette Tab in SettingsForm

**Location:** SettingsForm > Palette tab (one of 11 tabs)

The PaletteSettingsForm local component is embedded in the Palette tab of the 11-tab SettingsForm. Tab visibility is RBAC-governed (Owner, Manager).

**SettingsForm Tab Structure (11 tabs):**

| Tab | Content | RBAC |
|---|---|---|
| Business Profile | Name, email, phone, address, timezone, entity type | Owner, Manager |
| Palette | PaletteSettingsForm (local component) | Owner, Manager |
| System Currency | Currency display (locked after first transaction) | Owner only |
| Legal Acknowledgement | Read-only view of acknowledgements | Owner only |
| Display Currency | Optional per-service currency override | Owner, Manager |
| Payment Gateway | Stripe/Paystack configuration (API keys in Vault) | Owner only |
| Services | Service catalogue management | Owner, Manager, Admin |
| Users & Permissions | User management and RBAC | Owner only |
| Email Configuration | Brevo SMTP and API settings | Owner only |
| Theme | M3 theme customisation | Owner, Manager |
| Feature Flags | Feature activation toggles | Owner only |

**Key Design Rules:**
- Tab navigation uses M3 buttons, not a tab strip component.
- "Return to Onboarding" button in header — visible only when onboarding is in progress.
- Vault access (Payment Gateway tab) requires TOTP step-up per ADR-03.
- Legal Acknowledgement tab is read-only. The acknowledgement action belongs to onboarding.

#### Task 2.2: Palette Selection in OnboardingForm

**Location:** OnboardingForm — step 3 of 6

Palette selection is a step within the OnboardingForm. It is not a separate form.

**OnboardingForm Flow (6 steps):**
1. Welcome and orientation.
2. Business Profile — links to Settings Business Profile tab.
3. Palette Selection — links to Settings Palette tab.
4. System Currency — dropdown plus confirm (required, immutable after first transaction).
5. Legal Acknowledgement — text, checkbox, confirm (required, logged in Mybizz_management).
6. Completion — onboarding complete.

**Key Design Rules:**
- Progress indicator visible throughout.
- Bidirectional navigation (back and forward) between steps.
- "Go to Settings" links open the corresponding Settings tab and return to onboarding when done.
- Legal acknowledgement is a required action in onboarding, not a mutable Settings configuration.
- Payment gateway is not part of onboarding. Onboarding links to Settings for gateway setup.

#### Task 2.3: Update All Public Pages to Respect Palette Tokens

**Location:** All public page templates

**Implementation:**
- Ensure all colour references use `%color:Token%` syntax.
- Verify no hardcoded hex values in Designer.
- Test palette switching across all pages.

**Pages to Update:**
- `HomePageClassicTemplate`
- `HomePageServicesTemplate`
- `HomePageBookingTemplate`
- `HomePageMinimalistTemplate`
- `BlogListTiledTemplate`
- `BlogListListTemplate`
- `BlogPostClassicTemplate`
- `BlogPostTwoColumnTemplate`
- `BlogPostFullWidthTemplate`
- `FooterComponent`
- All landing page templates

**Verification Checklist:**
- [ ] Hero sections use primary/primary-container colours.
- [ ] Buttons use primary/on-primary colours.
- [ ] Cards use surface/surface-variant colours.
- [ ] Text uses on-surface colours.
- [ ] Links use primary colour.
- [ ] Footers use surface-variant background.

---

## Workstream 2: Blog Enhancement

### Stage 3: Blog List Views

#### Task 3.1: Tiled Grid View Component

**Component Name:** `BlogListTiledTemplate`

**Purpose:** Display blog posts in a responsive tiled grid layout.

**Anvil Components Used:**
- `ColumnPanel` — Main content wrapper (id: `col_main`, padding: 24px)
- `FlowPanel` — Filter row (id: `flow_filters`, horizontal orientation, gap: 16px, padding: 16px 0)
  - `TextBox` — Search input (id: `txt_search`, appearance: `outlined`, role: `search-input`, placeholder: "Search posts...")
  - `DropdownMenu` — Category filter (id: `dd_category`, appearance: `outlined`, role: `filter-dropdown`)
  - `DropdownMenu` — Status filter (id: `dd_status`, appearance: `outlined`, role: `filter-dropdown`)
- `FlowPanel` — Grid container (id: `flow_grid`, orientation: `vertical`, wrap: `True`, gap: 24px)
  - `Card` (elevated, id: `card_post`, border-radius: 12px) — Blog post card
    - `CardContentContainer` — Inner content (id: `col_card_content`, padding: 16px)
      - `Image` — Featured image (id: `img_featured`, width: `100%`, height: `200px`, object-fit: `cover`, border-radius: 8px, 16:9 aspect ratio)
      - `Text` (caption, role: `category-badge`, background: `Primary Container`, colour: `On Primary Container`, padding: `4px 8px`, border-radius: `8px`, display: `inline-block`) — Category badge
      - `Heading` (title-medium, id: `hdg_title`, margin-top: 8px) — Post title
      - `Text` (body-medium, id: `txt_excerpt`, max-lines: `3`, overflow: `ellipsis`, margin-top: 8px) — Excerpt
      - `FlowPanel` — Meta row (id: `flow_meta`, horizontal orientation, gap: 16px, margin-top: 12px)
        - `Text` (caption, id: `txt_author`) — Author name
        - `Text` (caption, id: `txt_date`) — Published date
        - `Text` (caption, id: `txt_views`) — View count
      - `Button` (tertiary, id: `btn_read_more`, role: `read-more-button`, margin-top: 16px) — "Read More"

**Responsive Behavior:**
- Desktop: 3-column grid.
- Tablet: 2-column grid.
- Mobile: 1-column grid (full-width).

#### Task 3.2: List with Thumbnail View Component

**Component Name:** `BlogListListTemplate`

**Purpose:** Display blog posts in a list layout with thumbnail images.

**Anvil Components Used:**
- `ColumnPanel` — Main content wrapper (id: `col_main`, padding: 24px)
- `FlowPanel` — Filter row (same as Tiled Grid)
- `RepeatingPanel` — Blog post list (id: `rp_posts`)
  - `FlowPanel` — Row container (id: `flow_row`, horizontal orientation, padding: 16px, border-bottom: `1px solid var(--app-color-outline)`)
    - `Image` — Featured image (id: `img_thumbnail`, width: `240px`, height: `160px`, border-radius: `8px`, object-fit: `cover`)
    - `ColumnPanel` — Content column (flex: 1, margin-left: 16px)
      - `Text` (caption, role: `category-badge`) — Category badge
      - `Heading` (title-medium, id: `hdg_title`) — Post title
      - `Text` (body-medium, id: `txt_excerpt`, max-lines: `4`) — Excerpt
      - `FlowPanel` — Meta row (id: `flow_meta`, horizontal orientation, gap: 16px, margin-top: 8px)
        - `Text` (caption, id: `txt_author`) — Author name
        - `Text` (caption, id: `txt_date`) — Published date
      - `Button` (tertiary, id: `btn_read_more`) — "Read More"

**Responsive Behavior:**
- Desktop: Full-width rows, image left (240px), content right.
- Tablet: Full-width rows, image left (200px), content right.
- Mobile: Stacked layout (image top, content bottom).

#### Task 3.3: Blog View Toggle in Settings

**Location:** SettingsForm > Blog Settings section

Contains "Default list view" dropdown (Tiled Grid | List with Thumbnail) and "Enable comments" toggle.

---

### Stage 4: Blog Post Templates

#### Task 4.1: Classic Centered Template

**Component Name:** `BlogPostClassicTemplate`

**Purpose:** Classic centred blog post layout with single-column content.

**Key Layout:**
- Centred single column, max-width 720px.
- Title (headline-large), meta row, featured image (full-width, 800px max).
- Content area with categories, comments section, related posts, social sharing.

**Responsive Behavior:**
- Desktop: Centred single column, max-width 720px.
- Tablet: Centred single column, max-width 640px.
- Mobile: Full-width single column.

#### Task 4.2: Two-Column with Sidebar Template

**Component Name:** `BlogPostTwoColumnTemplate`

**Purpose:** Blog post with main content column and sidebar.

**Key Layout:**
- Main column (60%) plus sidebar (40%).
- Sidebar widgets: Author bio, related posts (3), categories, newsletter signup.

**Responsive Behavior:**
- Desktop: Two-column layout (60% main, 40% sidebar).
- Tablet: Two-column layout (65% main, 35% sidebar).
- Mobile: Stacked layout (main top, sidebar bottom).

#### Task 4.3: Full-Width Immersive Template

**Component Name:** `BlogPostFullWidthTemplate`

**Purpose:** Full-width immersive blog post layout with hero section.

**Key Layout:**
- Full-width hero with primary-container background.
- Content column, max-width 900px.
- Pullquote section (tonal-card).
- Categories, comments, related posts, social sharing.

**Responsive Behavior:**
- Desktop: Full-width hero, centred content max-width 900px.
- Tablet: Full-width hero, centred content max-width 800px.
- Mobile: Full-width hero, full-width content.

#### Task 4.4: Blog Post Template Selector

**Location:** Blog Editor Form > "Select Post Template"

Three template options: Classic, Two-Column, Full-Width. Per-page template selection (not global default).

---

## Workstream 3: Custom M3 Components

### Stage 5: Custom Component Specifications

#### Task 5.1: TimeLapseCarouselComponent

**Component Name:** `TimeLapseCarouselComponent`

**Purpose:** Auto-rotating image carousel with manual navigation.

**Anvil Components Used:**
- `ColumnPanel` — Carousel container (id: `col_carousel`, width: `100%`, position: `relative`, overflow: `hidden`)
- `FlowPanel` — Carousel track (id: `flow_track`, horizontal orientation, transition: `transform 0.3s ease`)
  - `Image` — Slides (multiple, width: `100%`, height: `400px`, object-fit: `cover`)
- `IconButton` — Previous (id: `btn_prev`, icon: `chevron_left`)
- `IconButton` — Next (id: `btn_next`, icon: `chevron_right`)
- `FlowPanel` — Indicator dots (id: `flow_dots`, horizontal orientation, gap: 8px)

**Properties:**
- `slide_images` — List of Image components.
- `auto_rotate` — Boolean (default: `True`).
- `rotation_interval` — Integer (milliseconds, default: `5000`).
- `circular` — Boolean (default: `True`).

#### Task 5.2: ParallaxComponent

**Component Name:** `ParallaxComponent`

**Purpose:** Parallax scroll effect with background image.

**Anvil Components Used:**
- `ColumnPanel` — Parallax container (id: `col_parallax`, position: `relative`, overflow: `hidden`)
  - `Image` — Background image (id: `img_background`, position: `fixed`, width: `100%`, height: `400px`, object-fit: `cover`)
  - `ColumnPanel` — Foreground content (id: `col_foreground`, position: `relative`, z-index: `1`, padding: `48px 24px`)
    - `Heading` — Headline
    - `Text` — Body text
    - `Button` — CTA button

**Properties:**
- `background_image` — Image component.
- `parallax_speed` — Float (0.1-1.0, default: `0.5`).
- `height` — Integer (pixels, default: `400`).

#### Task 5.3: ClauseBuilder (Shared Custom Component)

**Component Name:** `ClauseBuilder`

**Purpose:** Standalone reusable clause editor for legal policy documents.

**Architecture:** Shared custom component used by Privacy Policy and Terms & Conditions editing flows. Parameterized by document type (`privacy_policy` or `t_and_c`).

**Wireframe:** `wireframe-custom-component-ClauseBuilder.html`

**Anvil Components Used:**
- `ColumnPanel` — Main container (padding: 24px)
  - `Heading` (headline-small) — Text: "Clause Editor"
  - `DropdownMenu` — Category selector (id: `dd_category`, appearance: `outlined`)
    - Items: Set dynamically from clause categories for the selected document type.
  - `RepeatingPanel` — Clause list (id: `rp_clauses`)
    - `Card` — Clause card
      - `CardContentContainer` — Inner content (padding: 16px)
        - `Text` (caption, id: `txt_category`) — Category name
        - `Heading` (title-medium, id: `hdg_title`) — Clause title
        - `Text` (body-medium, id: `txt_clause`) — Clause text
        - `FlowPanel` — Action row (horizontal orientation, gap: 8px)
          - `Link` (id: `lnk_edit`) — "Edit"
          - `Link` (id: `lnk_delete`) — "Delete" (hidden for seeded clauses)
  - `Button` (outlined, id: `btn_add_clause`) — "+ Add Custom Clause"
  - `ColumnPanel` — Add/Edit form (id: `col_clause_form`, visible: `False`)
    - `TextBox` — Title input (id: `txt_title`, appearance: `outlined`)
    - `DropdownMenu` — Category selector (id: `dd_form_category`, appearance: `outlined`)
    - `TextArea` — Clause text (id: `txt_clause_text`, appearance: `outlined`)
    - `FlowPanel` — Form actions (horizontal orientation, gap: 8px)
      - `Button` (filled, id: `btn_save_clause`) — "Save"
      - `Button` (outlined, id: `btn_cancel_clause`) — "Cancel"
  - `Button` (filled, id: `btn_publish`) — "Publish Changes"

**Visual Distinction:**
- Seeded clauses: green border, editable text, not deletable.
- Custom clauses: blue border, editable and deletable.

**Data Model:**
- Two tables: `privacy_policy` and `t_and_c`.
- Shared structure: `instance_id`, `category`, `title`, `clause`, `sort_order`, `is_seeded`, `is_active`, `created_at`, `updated_at`.

**Key Behaviours:**
- Category dropdown filters clause list.
- "+ Add Custom Clause" button reveals add form.
- "Edit" link on each clause reveals edit form pre-populated with clause data.
- "Save" button writes changes to datatable.
- "Publish Changes" button publishes to the corresponding public page.
- Seeded clauses cannot be deleted, only edited.

---

## Workstream 4: Footer Bar

### Stage 6: Footer Component

#### Task 6.1: Footer Component Specification

**Component Name:** `FooterComponent`

**Purpose:** Comprehensive footer with logo, credentials, social links, policies, map, and contact.

**Anvil Components Used:**
- `ColumnPanel` — Footer container (id: `col_footer`, surface-variant background, padding: `48px 24px 24px`)
  - `FlowPanel` — Top row (id: `flow_top`, horizontal orientation, gap: 32px, flex-wrap: `wrap`, justify-content: `space-between`)
    - `ColumnPanel` — Logo section (id: `col_logo`, flex: `1`, min-width: `200px`)
      - `Image` — Business logo (id: `img_logo`, width: `150px`, height: `auto`)
      - `Text` (body-medium, id: `txt_business_name`) — Business name
    - `ColumnPanel` — Credentials section (id: `col_credentials`, flex: `1`, min-width: `200px`)
      - `Text` (caption, id: `txt_creds_1`) — Accreditation 1
      - `Text` (caption, id: `txt_creds_2`) — Accreditation 2
  - `FlowPanel` — Middle row (id: `flow_middle`, horizontal orientation, gap: 32px, flex-wrap: `wrap`, margin-top: `32px`)
    - `FlowPanel` — Social links (id: `flow_social`, horizontal orientation, gap: 16px)
      - `IconButton` — Facebook, Instagram, LinkedIn, Twitter/X
    - `FlowPanel` — Policy links (id: `flow_policies`, horizontal orientation, gap: 16px, flex-wrap: `wrap`)
      - `Link` — Privacy Policy (links to PrivacyPolicyPage)
      - `Link` — Terms & Conditions (links to TermsConditionsPage)
      - `Link` — Cookie Policy
  - `FlowPanel` — Bottom row (id: `flow_bottom`, horizontal orientation, gap: 32px, flex-wrap: `wrap`, margin-top: `32px`)
    - `ColumnPanel` — Google Maps embed (id: `col_map`, flex: `1`, min-width: `300px`)
      - `HtmlTemplate` — Google Maps iframe
    - `ColumnPanel` — Contact us section (id: `col_contact`, flex: `1`, min-width: `200px`)
      - `Heading` (title-medium) — Text: "Contact Us"
      - `Text` (body-medium) — Email address
      - `Text` (body-medium) — Phone number
      - `Button` (tertiary) — "Send Message" (links to Contact page)
  - `FlowPanel` — Copyright row (id: `flow_copyright`, horizontal orientation, gap: 16px, margin-top: `32px`, justify-content: `space-between`)
    - `Text` (caption) — Copyright text
    - `Link` — "Back to Top"

**Settings Integration (within SettingsForm tabs):**
- Logo upload: Business Profile tab > "Business logo" (`FileLoader`).
- Credentials: Business Profile tab > "Accreditations" (`TextArea`, one per line).
- Social links: Business Profile tab > "Social media links" (`TextBox` fields).
- Google Maps: Business Profile tab > "Google Maps embed URL" (`TextBox`).
- Contact info: Business Profile tab > "Contact details" (`TextBox` fields).

---

## Workstream 5: Homepage Template Improvements

### Stage 7: Enhanced Homepage Templates

#### Task 7.1: Improved Classic Template

**Component Name:** `HomePageClassicTemplate`

**Key Sections:**
- Hero section with headline, subheadline, and CTA buttons.
- Services preview (3 cards).
- About section with image.
- Testimonials section.
- CTA section with primary-container background.

#### Task 7.2: Improved Services Template

**Component Name:** `HomePageServicesTemplate`

**Key Sections:**
- Hero section with "Our Services" headline.
- Services grid (all active services).
- "Why Choose Us" section with benefit cards.
- CTA section.

#### Task 7.3: Improved Booking Template

**Component Name:** `HomePageBookingTemplate`

**Key Sections:**
- Hero section with "Book Your Appointment" headline.
- Quick booking card (service selector, provider selector, date picker, check availability button).
- Services preview.
- Testimonials.
- CTA section.

#### Task 7.4: Improved Minimalist Template

**Component Name:** `HomePageMinimalistTemplate`

**Key Sections:**
- Hero section with simple headline and single CTA.
- Services row (3 cards, minimal).
- About snippet.
- Contact snippet.

#### Task 7.5: Homepage Template Selector

**Location:** SettingsForm > Theme tab (or Website configuration section)

Four template options: Classic, Services, Booking, Minimalist. Shows preview descriptions. "Apply Template" saves to `business_profile.homepage_template`.

---

## Workstream 6: Blog Comments and SEO

### Stage 8: Blog Comments System

#### Task 8.1: Comments Database Schema

**New Table:** `blog_comments`

| Column | Type | Notes |
|--------|------|-------|
| `id` | Row ID | Auto-generated primary key |
| `instance_id` | Link to `users` | Tenant isolation |
| `post_id` | Link to `blog_posts` | Parent post |
| `commenter_name` | Text | Commenter's name (required) |
| `commenter_email` | Text | Commenter's email (required) |
| `comment_text` | Text | Comment body (required) |
| `status` | Text | "pending", "approved", "spam", "deleted" |
| `created_at` | Date and Time | UTC timestamp |
| `updated_at` | Date and Time | UTC timestamp |

#### Task 8.2: Comments Toggle in Settings

**Location:** SettingsForm > Blog Settings section

Toggle: "Enable comments on blog posts" (`m3.Switch`). Default: enabled. Comments require manual approval before publishing.

#### Task 8.3: Comment Moderation Workflow

**Location:** Dashboard > Blog > Comments (CommentModerationForm)

Filter by status (All, Pending, Approved, Spam) and by post. Actions: Approve, Delete, Mark as Spam.

---

## Workstream 7: SEO and Social Sharing

### Stage 9: SEO Meta Tags

#### Task 9.1: Simple SEO Meta Tag Implementation

**Location:** Blog Editor Form > "SEO Settings" section

Fields:
- Meta title (TextBox, max 60 characters, with character counter).
- Meta description (TextBox, max 160 characters, with character counter).
- Keywords/tags (TextBox, comma-separated, with hint text).

#### Task 9.2: Public Page SEO Integration

All blog post templates set `document.title` and meta description/keywords from blog post SEO fields on page load.

---

### Stage 10: Social Sharing Buttons

#### Task 10.1: Social Sharing Implementation

**Location:** All blog post templates (Classic, Two-Column, Full-Width)

Three icon buttons: Facebook, Twitter, LinkedIn. Each opens the corresponding share URL in a new tab.

---

## Workstream 8: Policy Editor and Public Policy Pages

### Stage 11: Policy Editor

#### Task 11.1: PolicyEditorForm

**Location:** Settings > Policy Editor

**Purpose:** Settings-hosted screen for editing Privacy Policy and Terms & Conditions.

**Components:**
- Document type selector (Privacy Policy | Terms & Conditions).
- ClauseBuilder component (shared, parameterized by document type).
- Publish/Update button.

**Data Flow:**
- Reads from `privacy_policy` or `t_and_c` table based on selected document type.
- Writes changes via ClauseBuilder save actions.
- Publish button updates the corresponding public page.

#### Task 11.2: PrivacyPolicyPage

**Purpose:** Public-facing page that renders the Owner's privacy policy.

**Data Source:** `privacy_policy` table.

**Rendering:**
- Clauses grouped by category, ordered by `sort_order` within category.
- Only active clauses (`is_active = True`) displayed.
- Standard public page layout with FooterComponent.
- Follows public-facing UI tone (comfortable density, generous whitespace).

#### Task 11.3: TermsConditionsPage

**Purpose:** Public-facing page that renders the Owner's terms and conditions.

**Data Source:** `t_and_c` table.

**Rendering:**
- Same rendering rules as PrivacyPolicyPage.
- Clauses grouped by category, ordered by `sort_order`.
- Only active clauses displayed.
- Standard public page layout with FooterComponent.

---

## Final Deliverables

### Phase 1: Palette System Foundation

**Stage 1: Palette Definition**
- [x] Task 1.1: Define 4 M3-compatible palettes in `theme.css`
- [x] Task 1.2: Create palette selector component (`PaletteSettingsForm` — local)
- [x] Task 1.3: Palette selection embedded in OnboardingForm (no separate form)

**Stage 2: Palette Integration**
- [x] Task 2.1: Palette tab in SettingsForm (11-tab structure, RBAC-governed)
- [x] Task 2.2: Palette selection step in OnboardingForm
- [x] Task 2.3: Update all public pages to respect palette tokens

### Phase 2: Blog Enhancement

**Stage 3: Blog List Views**
- [x] Task 3.1: Create `BlogListTiledTemplate` component
- [x] Task 3.2: Create `BlogListListTemplate` component
- [x] Task 3.3: Add blog view toggle to Settings

**Stage 4: Blog Post Templates**
- [x] Task 4.1: Create `BlogPostClassicTemplate` component
- [x] Task 4.2: Create `BlogPostTwoColumnTemplate` component
- [x] Task 4.3: Create `BlogPostFullWidthTemplate` component
- [x] Task 4.4: Implement template selector in Blog Editor

**Stage 5: Blog Comments**
- [x] Task 5.1: Create `blog_comments` table schema
- [x] Task 5.2: Add comments toggle to Settings
- [x] Task 5.3: Create comment moderation workflow

### Phase 3: Homepage Improvements

**Stage 7: Enhanced Templates**
- [x] Task 7.1: Improve `HomePageClassicTemplate` component
- [x] Task 7.2: Improve `HomePageServicesTemplate` component
- [x] Task 7.3: Improve `HomePageBookingTemplate` component
- [x] Task 7.4: Improve `HomePageMinimalistTemplate` component
- [x] Task 7.5: Implement homepage template selector

### Phase 4: Footer and Custom Components

**Stage 6: Footer Component**
- [x] Task 6.1: Create `FooterComponent` component

**Stage 5: Custom M3 Components**
- [x] Task 5.1: Create `TimeLapseCarouselComponent` component
- [x] Task 5.2: Create `ParallaxComponent` component
- [x] Task 5.3: Create `ClauseBuilder` shared custom component

### Phase 5: SEO and Social

**Stage 9: SEO Implementation**
- [x] Task 9.1: Implement simple SEO meta tags in Blog Editor
- [x] Task 9.2: Add SEO data to blog post templates

**Stage 10: Social Sharing**
- [x] Task 10.1: Add social sharing buttons to all blog post templates

### Phase 6: Policy Editor and Public Pages

**Stage 11: Policy System**
- [x] Task 11.1: Create `PolicyEditorForm` screen
- [x] Task 11.2: Create `PrivacyPolicyPage` public page
- [x] Task 11.3: Create `TermsConditionsPage` public page

---

## Component Inventory

### Anvil M3 Native Components Used

| Component | Category | Usage Count |
|-----------|----------|-------------|
| `Heading` | Typography | 50+ instances |
| `Text` | Typography | 100+ instances |
| `Button` | Buttons | 40+ instances |
| `IconButton` | Buttons | 15+ instances |
| `m3.Switch` | Inputs | 5+ instances |
| `TextBox` | Inputs | 30+ instances |
| `TextArea` | Inputs | 15+ instances |
| `DropdownMenu` | Inputs | 20+ instances |
| `DatePicker` | Inputs | 5+ instances |
| `Image` | Display | 40+ instances |
| `Link` | Navigation | 30+ instances |
| `ColumnPanel` | Containers | 60+ instances |
| `FlowPanel` | Containers | 50+ instances |
| `Card` | Display | 40+ instances |
| `CardContentContainer` | Display | 40+ instances |
| `DataGrid` | Display | 10+ instances |
| `RepeatingPanel` | Containers | 15+ instances |
| `FileLoader` | Inputs | 5+ instances |
| `m3.Icon` | Display | 20+ instances |
| `HtmlTemplate` | Integrations | 10+ instances |
| `Timer` | Feedback | 5+ instances |

### Custom Components — Shared (17)

| Component | Type | Where Used |
|-----------|------|------------|
| `VideoDisplayComponent` | Shared | BlogPostForm, HomePage |
| `FooterComponent` | Shared | All public pages (6+) |
| `TimeLapseCarouselComponent` | Shared | HomePage, ServicesForm |
| `ParallaxComponent` | Shared | Multiple public pages |
| `ClauseBuilder` | Shared | PrivacyPolicyEditor, TermsConditionsEditor |
| `BlogListTiledTemplate` | Shared | Public blog listing |
| `BlogListListTemplate` | Shared | Public blog listing |
| `BlogPostClassicTemplate` | Shared | Public blog post |
| `BlogPostTwoColumnTemplate` | Shared | Public blog post |
| `BlogPostFullWidthTemplate` | Shared | Public blog post |
| `HomePageClassicTemplate` | Shared | Public homepage |
| `HomePageServicesTemplate` | Shared | Public homepage |
| `HomePageBookingTemplate` | Shared | Public homepage |
| `HomePageMinimalistTemplate` | Shared | Public homepage |
| `LeadCaptureTemplate` | Shared | Landing pages |
| `EventRegistrationTemplate` | Shared | Landing pages |
| `VslTemplate` | Shared | Landing pages |

### Custom Components — Local/Form-Specific (1)

| Component | Type | Where Used | Reason |
|-----------|------|------------|--------|
| `PaletteSettingsForm` | Local | SettingsForm Palette tab only | Used in one form only |

### Removed from V1 Scope (5)

| Component | Reason |
|-----------|--------|
| `OnboardingPaletteForm` | Obsolete. Palette selection is part of OnboardingForm. |
| `AnimatedButtonComponent` | Standard M3 components provide sufficient animations via role-based styling. |
| `AnimatedCardComponent` | Standard M3 components provide sufficient animations via role-based styling. |
| `AnimatedLinkComponent` | Standard M3 components provide sufficient animations via role-based styling. |
| `AnimatedImageComponent` | Standard M3 components provide sufficient animations via role-based styling. |

---

## Database Schema Changes

### New Tables

**blog_comments**

| Column | Type | Required | Notes |
|--------|------|----------|-------|
| `id` | Row ID | Auto | Primary key |
| `instance_id` | Link to users | Yes | Tenant isolation |
| `post_id` | Link to blog_posts | Yes | Parent post |
| `commenter_name` | Text | Yes | Commenter name |
| `commenter_email` | Text | Yes | Commenter email |
| `comment_text` | Text | Yes | Comment body |
| `status` | Text | Yes | pending/approved/spam/deleted |
| `created_at` | Date and Time | Auto | UTC timestamp |
| `updated_at` | Date and Time | Auto | UTC timestamp |

**privacy_policy**

| Column | Type | Required | Notes |
|--------|------|----------|-------|
| `instance_id` | Link to users | Yes | Tenant isolation |
| `category` | Text | Yes | Clause category |
| `title` | Text | Yes | Clause title |
| `clause` | Text | Yes | Clause text |
| `sort_order` | Number | Yes | Display order within category |
| `is_seeded` | True/False | Yes | True for master template clauses |
| `is_active` | True/False | Yes | True to display on public page |
| `created_at` | Date and Time | Auto | UTC timestamp |
| `updated_at` | Date and Time | Auto | UTC timestamp |

**t_and_c**

| Column | Type | Required | Notes |
|--------|------|----------|-------|
| `instance_id` | Link to users | Yes | Tenant isolation |
| `category` | Text | Yes | Clause category |
| `title` | Text | Yes | Clause title |
| `clause` | Text | Yes | Clause text |
| `sort_order` | Number | Yes | Display order within category |
| `is_seeded` | True/False | Yes | True for master template clauses |
| `is_active` | True/False | Yes | True to display on public page |
| `created_at` | Date and Time | Auto | UTC timestamp |
| `updated_at` | Date and Time | Auto | UTC timestamp |

### Modified Tables

**business_profile**

| Column | Type | Required | Notes |
|--------|------|----------|-------|
| `palette` | Text | No | Current palette ID |
| `palette_updated_at` | Date and Time | No | Last palette change |
| `blog_list_view` | Text | No | tiled-grid / list-thumbnail |
| `enable_comments` | True/False | No | Comments toggle (default: True) |
| `homepage_template` | Text | No | classic/services/booking/minimalist |
| `business_logo` | Media | No | Business logo image |
| `accreditations` | Text | No | Newline-separated accreditation list |
| `social_facebook` | Text | No | Facebook URL |
| `social_instagram` | Text | No | Instagram URL |
| `social_linkedin` | Text | No | LinkedIn URL |
| `social_twitter` | Text | No | Twitter/X URL |
| `google_maps_embed_url` | Text | No | Google Maps iframe URL |
| `contact_email` | Text | No | Contact email address |
| `contact_phone` | Text | No | Contact phone number |

**blog_posts**

| Column | Type | Required | Notes |
|--------|------|----------|-------|
| `template` | Text | No | classic/twocolumn/fullwidth |
| `meta_title` | Text | No | SEO title (max 60 chars) |
| `meta_description` | Text | No | SEO description (max 160 chars) |
| `keywords` | Simple Object | No | Array of keyword strings |
| `pullquote` | Text | No | Optional pullquote for full-width template |

---

## Implementation Notes

### No Additional CSS Required

All styling uses:
- Anvil M3 theme tokens (`%color:Token%`).
- Built-in M3 component properties.
- Anvil roles for semantic styling.
- CSS custom properties defined in `theme.css`.

### JavaScript Usage

Minimal JavaScript used for:
- Palette application (setting `data-palette` attribute).
- Parallax scroll effect.
- Social sharing (opening URLs in new tabs).
- Character counters for SEO fields.
- Back to top scroll.

### Performance Considerations

- Lazy loading for blog post comments.
- Pagination for comment moderation.
- Optimized image loading with proper dimensions.
- Client-side palette caching via localStorage.

---

## Open Questions

All questions have been resolved:

1. Video Display: YouTube only (V1).
2. Comments Moderation: Manual moderation.
3. Analytics: Anvil built-in analytics for view counts.
4. SEO Meta Tags: Simple, obvious implementation with character counters.
5. Social Sharing: Facebook, Twitter, LinkedIn buttons included.
6. Related Posts: By tag matching.

---

*End of design-consultation-plan-workflows.md — 2026-06-02*
