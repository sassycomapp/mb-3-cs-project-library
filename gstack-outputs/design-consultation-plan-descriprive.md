Design Consultation Plan — Mybizz CS
Executive Summary
This plan addresses four key design workstreams for the Mybizz CS platform:

Design System Foundation — Unified M3-based system for public + private contexts
Onboarding Palette Selection — 4 preset color palettes for client customization
Blog Enhancement — Tiled/list views + blog page template variants
Layout & Template Architecture — Content panel pattern for UI variation without code duplication
Workstream 1: Design System Foundation
1.1 Dual-Tone Design Direction
Management UI (Office Feel)

Principle: Easy, Obvious, Relevant
Visual language: Higher contrast, clearer hierarchy, obvious interactive elements
Color usage: Neutral surfaces, primary colors for actions only
Typography: Roboto with clear weight differentiation
Spacing: Consistent 4px rhythm, slightly more generous padding for readability
Components: Filled buttons for primary actions, outlined for secondary, clear disabled states
Public-Facing UI (Alluring & Accommodating)

Principle: Comfort, Engagement, Warmth
Visual language: Softer edges, warmer tones, inviting whitespace
Color usage: Palette-driven, more expressive use of primary/secondary containers
Typography: Same Roboto scale but with warmer line heights
Spacing: More generous, breathing room between sections
Components: Larger CTAs, subtle elevations, inviting micro-interactions
1.2 Unified Design Tokens
All palettes derive from the same M3 token structure:

Primary Palette → Changes per palette
Secondary Palette → Derives from primary
Neutral Palette → Fixed (surface, background, outline)
Typography → Fixed (Roboto scale)
Spacing → Fixed (4px rhythm)
Border Radius → Fixed (12px cards, 100px buttons)
Workstream 2: Onboarding Palette Selection
2.1 Four Preset Palettes
Palette Name	Primary Hue	Best For	Psychological Profile
Professional Blue	Blue (#6750A4 → adjusted)	Consultants, therapists, clinics	Trust, stability, professionalism
Warm Coral	Orange/Pink (#E85A4F)	Salons, spas, beauty, lifestyle	Warmth, creativity, approachability
Calm Teal	Teal/Green (#2E7D6D)	Fitness trainers, wellness coaches, physiotherapists	Balance, health, growth
Sophisticated Purple	Deep Purple (#6750A4)	High-end consultants, executive coaches, luxury services	Premium, exclusive, refined
2.2 Palette Application Points
Onboarding screen: Visual palette selector with preview cards
Settings: Display current palette, option to switch (once per month limit?)
Scope: Applies to:
Primary button colors
Navigation rail active states
Card container backgrounds
Link colors
Accent elements (badges, highlights)
2.3 Palette Preview Component
Design a palette selector card showing:

Primary color swatch (large)
Sample button (filled + outlined)
Sample card (tonal background)
Sample text (links, headings)
Business type icons (salon, clinic, consulting, fitness)
Workstream 3: Blog Enhancement
3.1 Blog List View Variants
Current wireframe uses: DataGrid view (administrative)

New public-facing options:

Option A: Tiled Grid View

3-column grid (desktop), 2-column (tablet), 1-column (mobile)
Each card shows:
Featured image (16:9 aspect ratio)
Category badge
Post title (headline-small)
Excerpt (2-3 lines, body-medium)
Meta: author, date, view count (caption)
"Read more" link (tertiary button)
Spacing: 16px gaps, 24px section padding
Option B: List with Thumbnail View

2-column layout (image left, content right)
Each row shows:
Featured image (240px × 160px)
Category badge
Post title (title-medium)
Excerpt (3-4 lines, body-medium)
Meta: author, date (body-small)
Alternating row background (surface vs. surface-variant)
Spacing: 16px between rows, 24px section padding
Admin control: Settings → Blog Settings → "Default list view" dropdown (Tiled | List)

3.2 Blog Post Page Templates
Template A: Classic Centered

Hero: Title (headline-large), meta (body-medium), featured image (full-width, 800px max)
Content: Single column, max 720px width, centered
Sidebar: None
Best for: Long-form, editorial content
Template B: Two-Column with Sidebar

Hero: Title (headline-medium), meta (body-small), featured image (300px × 200px, left)
Content: Main column (60%), sidebar (40%)
Sidebar widgets: Author bio, related posts (3), category list, newsletter signup
Best for: Engagement, internal linking
Template C: Full-Width Immersive

Hero: Title (headline-large), full-width featured image (1200px), meta centered over image
Content: Full-width, max 900px, generous line height (1.8)
Pullquotes: Full-width, italic, primary container background
Best for: Visual storytelling, photography-heavy posts
Admin control: When creating/editing a blog page:

Dropdown: "Select blog template" (Classic | Two-Column | Full-Width)
Per-page template selection (not global default)
Workstream 4: Layout & Template Architecture
4.1 Content Panel Pattern
Architecture principle: Layouts are shells; content panels are swappable templates.

Implementation:

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
Client selection flow:

Dashboard → Settings → Website
"Homepage template" dropdown → shows 4 options with previews
"Blog list view" dropdown → Tiled | List
"Blog post template" dropdown → Classic | Two-Column | Full-Width
Save → changes apply immediately (no code deployment)
4.2 Component Consistency Rule
All templates use identical underlying components, just arranged differently:

Component	Reused Across All Templates
HeroSection	Yes (varies: full-width vs. contained)
Card	Yes (tiled view uses cards, two-column uses cards)
Button	Yes (all button variants)
Heading	Yes (all heading scales)
Text	Yes (all body scales)
Image	Yes (all image displays)
Footer	Yes (identical across all pages)
Benefit: UI changes propagate to all templates automatically. No template-specific CSS.

Deliverables
Phase 1: Design System Foundation (Week 1)
 Define 4 color palettes in theme.css as M3 tokens
 Create palette preview component (PaletteSelectorForm)
 Document palette application points in design-direction.md
 Create dual-tone styling guidelines (management vs. public)
Phase 2: Blog Enhancement (Week 2)
 Design tiled blog list template (BlogListTiledTemplate)
 Design list-with-thumbnail blog template (BlogListListTemplate)
 Design 3 blog post page templates (Classic, Two-Column, Full-Width)
 Add blog view/template settings to Settings form
 Update wireframe-blog.html with new variants
Phase 3: Layout Architecture (Week 3)
 Document content panel pattern in design-direction.md
 Create homepage template variants (reuse existing 4, document pattern)
 Implement palette switching logic (onboarding + settings)
 Create palette migration utility (switching palettes updates all UI)
Phase 4: Documentation & Handoff (Week 4)
 Update design-direction.md with all new templates
 Create blog-templates.md with detailed specifications
 Create palette-system.md with token definitions
 Wireframe review with user (validate all variants)
Open Questions for You
Palette switching frequency: Should clients be able to switch palettes freely, or limit to once per month/onboarding only?
Homepage templates: The platform overview mentions 4 homepage templates (Classic, Services, Booking, Minimalist). Should these be included in the palette design, or focus on blog templates first?
Blog post template default: Should there be a global default blog post template (Settings), or strictly per-page selection?
Admin control depth: Beyond palette + blog templates, what other UI customizations should clients have at V1? (e.g., logo upload, favicon, business hours display, footer links)