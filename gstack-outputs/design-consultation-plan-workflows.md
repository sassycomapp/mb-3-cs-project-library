# Mybizz CS — Design Consultation Plan

**Status:** Final  
**Date:** 2026-05-23  
**Vertical:** Consulting & Services  
**Platform:** Anvil.works with Material Design 3 (M3)  
**Version:** 1.0 (Complete & Approved)

---

## Executive Summary

This plan delivers a unified Material Design 3 design system for the Mybizz CS platform, addressing all user requirements while strictly adhering to Anvil M3 component constraints. The plan focuses on:

1. **Four preset color palettes** with on-demand selection
2. **Enhanced blog system** with tiled/list views and template selection
3. **Custom M3 components** (carousel, parallax, animations)
4. **Footer bar** with all required elements
5. **Improved homepage templates**
6. **Blog comments system** with manual moderation
7. **Unified design language** across public and management interfaces

**Key Decisions:**
- VideoDisplayComponent supports YouTube only (V1)
- Comments use manual moderation (no email verification for V1)
- Blog view counts tracked via Anvil's built-in analytics
- SEO meta tags implemented in simple, obvious manner
- Social sharing buttons included on all blog posts
- Related posts determined by tag matching

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
- All 4 palettes defined as CSS custom properties
- Palette selector sets `data-palette` attribute on `:root` element via JavaScript
- All components reference tokens via `%color:Token%` syntax in Designer
- Zero hardcoded hex values in component code

#### Task 1.2: Palette Settings Screen

**Location:** Dashboard → Settings → Website → "Color Palette"

**Component Specification:**

**Form Name:** `PaletteSettingsForm`

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

**Code Behavior:**
```python
# In PaletteSettingsForm __init__
def __init__(self, **properties):
    self.init_components(**properties)
    self.preview_panel.visible = False
    self.current_palette_id = None

# Button click handler
def btn_palette_click(self, **event_args):
    palette_name = event_args['sender'].text
    palette_id = self._get_palette_id(palette_name)
    self._show_preview(palette_name, palette_id)

def _show_preview(self, palette_name, palette_id):
    # Update preview panel content
    self.preview_heading.text = palette_name
    self.preview_subtext.text = f"Best for {self._get_business_type(palette_name)}"
    self.preview_panel.visible = True
    self.apply_button.role = f"{palette_id}-button"

# Apply button handler
def btn_apply_click(self, **event_args):
    anvil.server.call('update_user_palette', self.current_palette_id)
    anvil.server.call('refresh_all_pages_with_palette', self.current_palette_id)
    anvil.show_info_message("Palette applied successfully")
```

**Server-Side Implementation:**
```python
# In server_code/server_settings/palette_service.py
import anvil

@anvil.server.callable
def update_user_palette(palette_id: str):
    """Update current user's palette preference"""
    user = anvil.users.get_user()
    if not user:
        raise Exception("Not authenticated")
    
    # Update business_profile table
    profile = app_tables.business_profile.get(instance_id=user)
    if profile:
        profile['palette'] = palette_id
        profile['palette_updated_at'] = datetime.datetime.now()
    
    return {'success': True, 'palette_id': palette_id}

@anvil.server.callable
def refresh_all_pages_with_palette(palette_id: str):
    """Refresh all public pages with new palette"""
    # This triggers client-side palette update via JavaScript
    return {'success': True}
```

#### Task 1.3: Onboarding Palette Selection

**Location:** Initial onboarding flow (first-time setup)

**Component Specification:**

**Form Name:** `OnboardingPaletteForm`

**Anvil Components Used:**
- `Heading` (headline-large) — Text: "Choose Your Color Palette"
- `Text` (body-large) — Text: "Select a color scheme that fits your business"
- `ColumnPanel` — Main container (padding: 32px)
  - `FlowPanel` — Palette selector row (horizontal orientation, gap: 16px)
    - `Button` (4 instances) — Same as Settings screen
  - `ColumnPanel` — Preview content panel (id: `preview_panel`, visible: `False`)
    - Same structure as Settings screen
    - `Button` (filled) — Text: "Continue with This Palette"
  - `Text` (caption) — Text: "You can change this later in Settings"

**Code Behavior:**
```python
# In OnboardingPaletteForm __init__
def __init__(self, **properties):
    self.init_components(**properties)
    self.preview_panel.visible = False
    self.current_palette_id = None

# Continue button handler
def btn_continue_click(self, **event_args):
    if not self.current_palette_id:
        anvil.show_info_message("Please select a palette")
        return
    
    # Save palette to business_profile
    anvil.server.call('save_onboarding_palette', self.current_palette_id)
    
    # Navigate to next onboarding step
    open_form('OnboardingBusinessInfoForm')
```

**Server-Side Implementation:**
```python
@anvil.server.callable
def save_onboarding_palette(palette_id: str):
    """Save palette during onboarding"""
    user = anvil.users.get_user()
    if not user:
        raise Exception("Not authenticated")
    
    profile = app_tables.business_profile.get(instance_id=user)
    if not profile:
        # Create new profile
        profile = app_tables.business_profile.add_row(
            instance_id=user,
            palette=palette_id,
            palette_updated_at=datetime.datetime.now()
        )
    else:
        profile['palette'] = palette_id
        profile['palette_updated_at'] = datetime.datetime.now()
    
    return {'success': True}
```

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

#### Task 2.1: Add Palette Selector to Settings Screen

**Location:** `SettingsForm` → "Website" tab

**Implementation:**
- Add new section to Settings form: "Color Palette"
- Embed `PaletteSettingsForm` as a sub-component
- Link to palette settings from main Settings navigation

**Anvil Components Used:**
- `Link` — Navigation item: "Color Palette" (icon: `palette`)
- `ColumnPanel` — Content container
  - `Placeholder` — Dynamic component loading
    - Component: `PaletteSettingsForm`

**Code Behavior:**
```python
# In SettingsForm
def build_navigation(self):
    # ... existing navigation ...
    self.nav_palette = Link(text="Color Palette", icon="palette")
    self.nav_palette.set_event_handler(
        'click',
        lambda e, form='PaletteSettingsForm': self._show_form(form)
    )
    self.sidebar_panel.add_component(self.nav_palette)

def _show_form(self, form_name):
    self.content_panel.clear()
    if form_name == 'PaletteSettingsForm':
        self.content_panel.add_component(PaletteSettingsForm())
```

#### Task 2.2: Add Palette Selector to Onboarding Flow

**Location:** Onboarding sequence (step 2 of 5)

**Implementation:**
- Insert `OnboardingPaletteForm` as step 2 in onboarding flow
- Store palette selection in `business_profile` table
- Continue to next step only after palette selection

**Onboarding Flow Structure:**
1. Account creation (email/password)
2. **Palette selection** ← New step
3. Business information (name, address, contact)
4. Service setup (add first service)
5. Completion

#### Task 2.3: Update All Public Pages to Respect Palette Tokens

**Location:** All public page templates

**Implementation:**
- Ensure all color references use `%color:Token%` syntax
- Verify no hardcoded hex values in Designer
- Test palette switching across all pages

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
- [ ] Hero sections use primary/primary-container colors
- [ ] Buttons use primary/on-primary colors
- [ ] Cards use surface/surface-variant colors
- [ ] Text uses on-surface colors
- [ ] Links use primary color
- [ ] Footers use surface-variant background

---

## Workstream 2: Blog Enhancement

### Stage 3: Blog List Views

#### Task 3.1: Tiled Grid View Component

**Component Name:** `BlogListTiledTemplate`

**Purpose:** Display blog posts in a responsive tiled grid layout

**Anvil Components Used:**
- `ColumnPanel` — Main content wrapper (id: `col_main`, padding: 24px)
- `FlowPanel` — Filter row (id: `flow_filters`, horizontal orientation, gap: 16px, padding: 16px 0)
  - `TextBox` — Search input (id: `txt_search`, appearance: `outlined`, role: `search-input`, placeholder: "Search posts...")
  - `DropdownMenu` — Category filter (id: `dd_category`, appearance: `outlined`, role: `filter-dropdown`)
    - Items: Set dynamically in code
  - `DropdownMenu` — Status filter (id: `dd_status`, appearance: `outlined`, role: `filter-dropdown`)
    - Items: ["All", "Published", "Draft", "Archived"]
- `FlowPanel` — Grid container (id: `flow_grid`, orientation: `vertical`, wrap: `True`, gap: 24px)
  - `Card` (elevated, id: `card_post`, border-radius: 12px) — Blog post card
    - `CardContentContainer` — Inner content (id: `col_card_content`, padding: 16px)
      - `Image` — Featured image (id: `img_featured`, width: `100%`, height: `200px`, object-fit: `cover`, border-radius: 8px, 16:9 aspect ratio)
      - `Text` (caption, role: `category-badge`, background: `Primary Container`, color: `On Primary Container`, padding: `4px 8px`, border-radius: `8px`, display: `inline-block`) — Category badge
      - `Heading` (title-medium, id: `hdg_title`, margin-top: 8px) — Post title
      - `Text` (body-medium, id: `txt_excerpt`, max-lines: `3`, overflow: `ellipsis`, margin-top: 8px) — Excerpt
      - `FlowPanel` — Meta row (id: `flow_meta`, horizontal orientation, gap: 16px, margin-top: 12px)
        - `Text` (caption, id: `txt_author`) — Author name
        - `Text` (caption, id: `txt_date`) — Published date
        - `Text` (caption, id: `txt_views`) — View count
      - `Button` (tertiary, id: `btn_read_more`, role: `read-more-button`, margin-top: 16px) — "Read More"
- `DataGrid` — Pagination controls (id: `dg_pagination`, visible: `False` on mobile)

**Responsive Behavior:**
- Desktop: 3-column grid (FlowPanel with max-width constraint, 3 cards per row)
- Tablet: 2-column grid (2 cards per row)
- Mobile: 1-column grid (1 card per row, full-width)

**Code Implementation:**
```python
# In BlogListTiledTemplate __init__
def __init__(self, **properties):
    self.init_components(**properties)
    self._load_categories()
    self._load_posts()

def _load_categories(self):
    """Load blog categories into dropdown"""
    categories = app_tables.blog_categories.search(order_by='name')
    items = ["All Categories"] + [cat['name'] for cat in categories]
    self.dd_category.items = items

def _load_posts(self):
    """Load blog posts into grid"""
    self.flow_grid.clear()
    
    posts = app_tables.blog_posts.search(
        self._get_filters(),
        order_by='published_at desc'
    )
    
    for post in posts:
        card = self._create_post_card(post)
        self.flow_grid.add_component(card)

def _create_post_card(self, post):
    """Create a single post card"""
    card = Card()
    content = card.add_child(ColumnPanel(padding=16))
    
    # Featured image
    img = Image()
    img.url = post.get('featured_image')
    img.style = "width: 100%; height: 200px; object-fit: cover; border-radius: 8px;"
    content.add_component(img)
    
    # Category badge
    cat_text = Text()
    if post['category']:
        cat_text.text = post['category']['name']
        cat_text.role = "category-badge"
    content.add_component(cat_text)
    
    # Title
    hdg = Heading()
    hdg.text = post['title']
    hdg.text_size = "title/medium"
    hdg.font_weight = "500"
    content.add_component(hdg)
    
    # Excerpt
    txt = Text()
    txt.text = post.get('excerpt', '')[:150] + "..."
    txt.text_size = "body/medium"
    txt.max_lines = 3
    content.add_component(txt)
    
    # Meta row
    flow = FlowPanel(orientation='horizontal', gap=16)
    
    author = Text()
    author.text = f"By {post['author']}"
    author.text_size = "caption"
    flow.add_component(author)
    
    date = Text()
    date.text = post['published_at'].strftime('%b %d, %Y') if post['published_at'] else 'Draft'
    date.text_size = "caption"
    flow.add_component(date)
    
    views = Text()
    views.text = f"{post['view_count']} views"
    views.text_size = "caption"
    flow.add_component(views)
    
    content.add_component(flow)
    
    # Read more button
    btn = Button()
    btn.text = "Read More"
    btn.appearance = "text"
    btn.set_event_handler('click', lambda e, post_id=post['id']: self._open_post(post_id))
    content.add_component(btn)
    
    return card

def _get_filters(self):
    """Get current search and filter criteria"""
    q = app_tables.blog_posts.search()
    
    if self.txt_search.text:
        search_term = f"%{self.txt_search.text}%"
        q = q.any_of('title', 'excerpt', 'content', search_term)
    
    if self.dd_category.selected_index > 0:
        category = self.dd_category.selected_index
        q = q.equal('category_id', category)
    
    return q

def _open_post(self, post_id):
    """Open blog post detail page"""
    open_form('BlogPostForm', post_id=post_id)
```

#### Task 3.2: List with Thumbnail View Component

**Component Name:** `BlogListListTemplate`

**Purpose:** Display blog posts in a list layout with thumbnail images

**Anvil Components Used:**
- `ColumnPanel` — Main content wrapper (id: `col_main`, padding: 24px)
- `FlowPanel` — Filter row (id: `flow_filters`, horizontal orientation, gap: 16px, padding: 16px 0)
  - `TextBox` — Search input (id: `txt_search`, appearance: `outlined`)
  - `DropdownMenu` — Category filter (id: `dd_category`, appearance: `outlined`)
  - `DropdownMenu` — Status filter (id: `dd_status`, appearance: `outlined`)
- `RepeatingPanel` — Blog post list (id: `rp_posts`)
  - `FlowPanel` — Row container (id: `flow_row`, horizontal orientation, padding: 16px, border-bottom: `1px solid var(--app-color-outline)`, 20% opacity)
    - `Image` — Featured image (id: `img_thumbnail`, width: `240px`, height: `160px`, border-radius: `8px`, object-fit: `cover`)
    - `ColumnPanel` — Content column (flex: 1, margin-left: 16px)
      - `Text` (caption, role: `category-badge`) — Category badge
      - `Heading` (title-medium, id: `hdg_title`) — Post title
      - `Text` (body-medium, id: `txt_excerpt`, max-lines: `4`) — Excerpt
      - `FlowPanel` — Meta row (id: `flow_meta`, horizontal orientation, gap: 16px, margin-top: 8px)
        - `Text` (caption, id: `txt_author`) — Author name
        - `Text` (caption, id: `txt_date`) — Published date
      - `Button` (tertiary, id: `btn_read_more`) — "Read More" (aligned to right)
- `DataGrid` — Pagination controls (id: `dg_pagination`)

**Responsive Behavior:**
- Desktop: Full-width rows, image left (240px), content right
- Tablet: Full-width rows, image left (200px), content right
- Mobile: Stacked layout (image top 100% width, content bottom)

**Code Implementation:**
```python
# In BlogListListTemplate __init__
def __init__(self, **properties):
    self.init_components(**properties)
    self._load_categories()
    self._load_posts()

def _load_posts(self):
    """Load blog posts into repeating panel"""
    posts = app_tables.blog_posts.search(
        self._get_filters(),
        order_by='published_at desc'
    )
    
    self.rp_posts.item_source = posts

def bind_row(self, row_panel):
    """Bind data to row"""
    post = row_panel.item
    
    # Thumbnail
    img = row_panel.img_thumbnail
    img.url = post.get('featured_image')
    
    # Category badge
    cat_text = row_panel.find_element('category-badge')
    if post['category']:
        cat_text.text = post['category']['name']
    else:
        cat_text.text = ""
    
    # Title
    row_panel.hdg_title.text = post['title']
    
    # Excerpt
    row_panel.txt_excerpt.text = post.get('excerpt', '')[:200] + "..."
    
    # Author
    row_panel.txt_author.text = f"By {post['author']}"
    
    # Date
    if post['published_at']:
        row_panel.txt_date.text = post['published_at'].strftime('%b %d, %Y')
    else:
        row_panel.txt_date.text = 'Draft'
    
    # Read more button
    row_panel.btn_read_more.set_event_handler(
        'click',
        lambda e, post_id=post['id']: self._open_post(post_id)
    )

def _open_post(self, post_id):
    """Open blog post detail page"""
    open_form('BlogPostForm', post_id=post_id)
```

#### Task 3.3: Blog View Toggle in Settings

**Location:** Dashboard → Settings → Blog → "Default list view"

**Component Specification:**

**Form Name:** `BlogSettingsForm`

**Anvil Components Used:**
- `Heading` (headline-small) — Text: "Blog Settings"
- `ColumnPanel` — Main container (padding: 24px)
  - `FlowPanel` — View selector row
    - `Text` (body-medium) — Text: "Default list view:"
    - `DropdownMenu` — View selector (id: `dd_view_type`, appearance: `outlined`)
      - Items: ["Tiled Grid", "List with Thumbnail"]
      - Default: "Tiled Grid"
  - `Text` (caption) — Text: "This setting applies to all public blog listing pages"

**Code Implementation:**
```python
# In BlogSettingsForm __init__
def __init__(self, **properties):
    self.init_components(**properties)
    self._load_current_setting()

def _load_current_setting(self):
    """Load current blog view setting"""
    user = anvil.users.get_user()
    profile = app_tables.business_profile.get(instance_id=user)
    
    if profile and profile.get('blog_list_view'):
        view_type = profile['blog_list_view']
        index = ["Tiled Grid", "List with Thumbnail"].index(view_type)
        self.dd_view_type.selected_index = index

def btn_save_click(self, **event_args):
    """Save blog view setting"""
    user = anvil.users.get_user()
    profile = app_tables.business_profile.get(instance_id=user)
    
    view_type = self.dd_view_type.items[self.dd_view_type.selected_index]
    profile['blog_list_view'] = view_type
    
    anvil.show_info_message("Blog view setting saved")
```

**Server-Side Implementation:**
```python
@anvil.server.callable
def update_blog_list_view(view_type: str):
    """Update blog list view preference"""
    user = anvil.users.get_user()
    if not user:
        raise Exception("Not authenticated")
    
    profile = app_tables.business_profile.get(instance_id=user)
    if profile:
        profile['blog_list_view'] = view_type
    
    return {'success': True}
```

---

### Stage 4: Blog Post Templates

#### Task 4.1: Classic Centered Template

**Component Name:** `BlogPostClassicTemplate`

**Purpose:** Classic centered blog post layout with single-column content

**Anvil Components Used:**
- `ColumnPanel` — Main content wrapper (id: `col_main`, max-width: `1200px`, margin: `0 auto`, padding: `24px`)
  - `Heading` (headline-large, id: `hdg_title`, text-align: `center`, margin-bottom: `8px`) — Post title
  - `FlowPanel` — Meta row (id: `flow_meta`, horizontal orientation, gap: 16px, justify-content: `center`, margin-bottom: `24px`)
    - `Text` (body-medium, id: `txt_author`) — "By [Author]"
    - `Text` (body-medium, id: `txt_date`) — Published date
    - `Text` (body-medium, id: `txt_views`) — View count
  - `Image` — Featured image (id: `img_featured`, width: `100%`, max-width: `800px`, border-radius: `12px`, margin-bottom: `32px`)
  - `ColumnPanel` — Content column (id: `col_content`, max-width: `720px`, margin: `0 auto`)
    - `TextArea` — Post content (id: `txt_content`, appearance: `outlined`, disabled: `True`, line-height: `1.8`, margin-bottom: `32px`)
    - `FlowPanel` — Categories row (id: `flow_categories`, horizontal orientation, gap: 8px, margin-bottom: `32px`)
      - `Text` (caption, role: `category-badge`) — Category badge (multiple)
    - `ColumnPanel` — Comments section (id: `col_comments`, visible: `True` if comments enabled)
      - `Heading` (headline-small) — Text: "Comments"
      - `RepeatingPanel` — Comments list (id: `rp_comments`)
        - `ColumnPanel` — Comment card (role: `surface-card`, padding: `16px`, margin-bottom: `16px`)
          - `Text` (title-medium, id: `txt_commenter`) — Commenter name
          - `Text` (caption, id: `txt_comment_date`) — Date
          - `Text` (body-medium, id: `txt_comment_text`) — Comment text
      - `TextBox` — Commenter name (id: `txt_commenter_name`, appearance: `outlined`, placeholder: "Your name")
      - `TextBox` — Commenter email (id: `txt_commenter_email`, appearance: `outlined`, placeholder: "Your email")
      - `TextArea` — Comment body (id: `txt_comment_body`, appearance: `outlined`, placeholder: "Write your comment...")
      - `Button` (filled, id: `btn_post_comment`) — "Post Comment"
    - `FlowPanel` — Related posts row (id: `flow_related`, horizontal orientation, gap: 16px, margin-top: `32px`)
      - `Link` — Related post link (multiple, id: `lnk_related_*`)
  - `FlowPanel` — Social sharing row (id: `flow_share`, horizontal orientation, gap: 16px, margin-top: `32px`)
    - `IconButton` — Facebook (id: `btn_share_fb`, icon: `facebook`)
    - `IconButton` — Twitter (id: `btn_share_tw`, icon: `twitter`)
    - `IconButton` — LinkedIn (id: `btn_share_li`, icon: `linkedin`)

**Responsive Behavior:**
- Desktop: Centered single column, max-width 720px
- Tablet: Centered single column, max-width 640px
- Mobile: Full-width single column, max-width 100%

**Code Implementation:**
```python
# In BlogPostClassicTemplate __init__
def __init__(self, post_id, **properties):
    self.init_components(**properties)
    self.post_id = post_id
    self._load_post()
    self._load_comments()
    self._load_related_posts()

def _load_post(self):
    """Load blog post data"""
    post = app_tables.blog_posts.get(id=self.post_id)
    if not post:
        raise Exception("Post not found")
    
    self.hdg_title.text = post['title']
    self.txt_author.text = f"By {post['author']}"
    
    if post['published_at']:
        self.txt_date.text = post['published_at'].strftime('%B %d, %Y')
    
    self.txt_views.text = f"{post['view_count']} views"
    
    # Update view count
    post['view_count'] = post['view_count'] + 1
    
    if post.get('featured_image'):
        self.img_featured.url = post['featured_image']
    
    self.txt_content.text = post['content']
    
    # Categories
    self.flow_categories.clear()
    if post['category']:
        badge = Text()
        badge.text = post['category']['name']
        badge.role = "category-badge"
        self.flow_categories.add_component(badge)

def _load_comments(self):
    """Load blog post comments"""
    user = anvil.users.get_user()
    profile = app_tables.business_profile.get(instance_id=user)
    
    # Check if comments are enabled
    if not profile.get('enable_comments', True):
        self.col_comments.visible = False
        return
    
    comments = app_tables.blog_comments.search(
        post_id=app_tables.blog_posts.get(id=self.post_id),
        status='approved',
        order_by='created_at asc'
    )
    
    self.rp_comments.item_source = comments

def bind_row(self, row_panel):
    """Bind comment data to row"""
    comment = row_panel.item
    
    row_panel.txt_commenter.text = comment['commenter_name']
    row_panel.txt_comment_date.text = comment['created_at'].strftime('%B %d, %Y')
    row_panel.txt_comment_text.text = comment['comment_text']

def _load_related_posts(self):
    """Load related posts by tag"""
    post = app_tables.blog_posts.get(id=self.post_id)
    
    if not post.get('tags'):
        return
    
    # Find posts with same tags
    related = app_tables.blog_posts.search(
        tags=post['tags'],
        id!=self.post_id,
        order_by='published_at desc'
    )[:3]
    
    self.flow_related.clear()
    for related_post in related:
        link = Link()
        link.text = related_post['title']
        link.set_event_handler(
            'click',
            lambda e, pid=related_post['id']: open_form('BlogPostClassicTemplate', post_id=pid)
        )
        self.flow_related.add_component(link)

def btn_post_comment_click(self, **event_args):
    """Post a new comment"""
    commenter_name = self.txt_commenter_name.text
    commenter_email = self.txt_commenter_email.text
    comment_text = self.txt_comment_body.text
    
    if not commenter_name or not commenter_email or not comment_text:
        anvil.show_info_message("Please fill in all fields")
        return
    
    # Add comment to database
    post = app_tables.blog_posts.get(id=self.post_id)
    app_tables.blog_comments.add_row(
        instance_id=anvil.users.get_user(),
        post_id=post,
        commenter_name=commenter_name,
        commenter_email=commenter_email,
        comment_text=comment_text,
        status='pending',  # Requires approval
        created_at=datetime.datetime.now()
    )
    
    anvil.show_info_message("Comment submitted for approval")
    
    # Clear form
    self.txt_commenter_name.text = ""
    self.txt_commenter_email.text = ""
    self.txt_comment_body.text = ""
```

#### Task 4.2: Two-Column with Sidebar Template

**Component Name:** `BlogPostTwoColumnTemplate`

**Purpose:** Blog post with main content column and sidebar

**Anvil Components Used:**
- `FlowPanel` — Main container (id: `flow_main`, horizontal orientation, gap: 32px, padding: `24px`)
  - `ColumnPanel` — Main content (id: `col_main`, flex: `2`, min-width: `0`)
    - `Heading` (headline-medium, id: `hdg_title`) — Post title
    - `FlowPanel` — Meta row (id: `flow_meta`, horizontal orientation, gap: 16px, margin-bottom: `16px`)
      - `Text` (body-medium, id: `txt_author`) — Author
      - `Text` (body-medium, id: `txt_date`) — Published date
      - `Text` (body-medium, id: `txt_views`) — View count
    - `Image` — Featured image (id: `img_featured`, width: `300px`, height: `200px`, float: `left`, margin: `0 24px 16px 0`, border-radius: `8px`)
    - `TextArea` — Post content (id: `txt_content`, appearance: `outlined`, disabled: `True`, line-height: `1.8`, margin-bottom: `32px`)
    - `FlowPanel` — Categories row (id: `flow_categories`, horizontal orientation, gap: 8px, margin-bottom: `32px`)
      - `Text` (caption, role: `category-badge`) — Category badge (multiple)
    - `ColumnPanel` — Comments section (id: `col_comments`)
      - Same structure as Classic template
    - `FlowPanel` — Related posts row (id: `flow_related`, horizontal orientation, gap: 16px, margin-top: `32px`)
      - `Link` — Related post link (multiple)
    - `FlowPanel` — Social sharing row (id: `flow_share`, horizontal orientation, gap: 16px, margin-top: `32px`)
      - `IconButton` — Facebook, Twitter, LinkedIn
  - `ColumnPanel` — Sidebar (id: `col_sidebar`, flex: `1`, min-width: `300px`)
    - `Card` (surface-card, id: `card_author`, margin-bottom: `24px`)
      - `CardContentContainer` — Inner content (padding: `16px`)
        - `Image` — Author avatar (id: `img_avatar`, width: `80px`, height: `80px`, border-radius: `50%`)
        - `Heading` (title-medium, id: `hdg_author_name`) — Author name
        - `Text` (body-medium, id: `txt_author_bio`) — Bio text
    - `Card` (surface-card, id: `card_related`, margin-bottom: `24px`)
      - `CardContentContainer` — Inner content (padding: `16px`)
        - `Heading` (title-medium) — Text: "Related Posts"
        - `ColumnPanel` — Related links list (id: `col_related_links`)
          - `Link` — Related post link (3-5 items)
    - `Card` (surface-card, id: `card_categories`, margin-bottom: `24px`)
      - `CardContentContainer` — Inner content (padding: `16px`)
        - `Heading` (title-medium) — Text: "Categories"
        - `ColumnPanel` — Category links (id: `col_category_links`)
          - `Link` — Category link (multiple)
    - `Card` (surface-card, id: `card_newsletter`)
      - `CardContentContainer` — Inner content (padding: `16px`)
        - `Heading` (title-medium) — Text: "Subscribe"
        - `Text` (body-medium) — Newsletter description
        - `TextBox` — Email input (id: `txt_email`, appearance: `outlined`, placeholder: "Enter your email")
        - `Button` (filled, id: `btn_subscribe`) — "Subscribe"

**Responsive Behavior:**
- Desktop: Two-column layout (60% main, 40% sidebar)
- Tablet: Two-column layout (65% main, 35% sidebar)
- Mobile: Stacked layout (main top, sidebar bottom, full-width)

**Code Implementation:**
```python
# In BlogPostTwoColumnTemplate __init__
def __init__(self, post_id, **properties):
    self.init_components(**properties)
    self.post_id = post_id
    self._load_post()
    self._load_sidebar()

def _load_post(self):
    """Load blog post data"""
    post = app_tables.blog_posts.get(id=self.post_id)
    
    self.hdg_title.text = post['title']
    self.txt_author.text = f"By {post['author']}"
    
    if post['published_at']:
        self.txt_date.text = post['published_at'].strftime('%B %d, %Y')
    
    self.txt_views.text = f"{post['view_count']} views"
    
    if post.get('featured_image'):
        self.img_featured.url = post['featured_image']
    
    self.txt_content.text = post['content']

def _load_sidebar(self):
    """Load sidebar content"""
    post = app_tables.blog_posts.get(id=self.post_id)
    
    # Author bio
    self.hdg_author_name.text = post['author']
    self.txt_author_bio.text = post.get('author_bio', 'Author biography')
    
    # Related posts
    self.col_related_links.clear()
    if post.get('tags'):
        related = app_tables.blog_posts.search(
            tags=post['tags'],
            id!=self.post_id,
            order_by='published_at desc'
        )[:3]
        
        for related_post in related:
            link = Link()
            link.text = related_post['title']
            link.set_event_handler(
                'click',
                lambda e, pid=related_post['id']: open_form('BlogPostTwoColumnTemplate', post_id=pid)
            )
            self.col_related_links.add_component(link)
    
    # Categories
    self.col_category_links.clear()
    categories = app_tables.blog_categories.search(order_by='name')
    for cat in categories:
        link = Link()
        link.text = cat['name']
        link.set_event_handler(
            'click',
            lambda e, cat_id=cat['id']: self._filter_by_category(cat_id)
        )
        self.col_category_links.add_component(link)

def _filter_by_category(self, category_id):
    """Filter posts by category"""
    open_form('BlogListTiledTemplate', category_id=category_id)
```

#### Task 4.3: Full-Width Immersive Template

**Component Name:** `BlogPostFullWidthTemplate`

**Purpose:** Full-width immersive blog post layout with hero section

**Anvil Components Used:**
- `ColumnPanel` — Main content wrapper (id: `col_main`, padding: `0`)
  - `ColumnPanel` — Hero section (id: `col_hero`, primary-container background, padding: `48px 24px`, text-align: `center`)
    - `Heading` (headline-large, id: `hdg_title`, color: `On Primary`) — Post title
    - `FlowPanel` — Meta row (id: `flow_meta`, horizontal orientation, gap: 16px, justify-content: `center`, margin-top: `16px`)
      - `Text` (body-medium, id: `txt_author`, color: `On Primary`) — "By [Author]"
      - `Text` (body-medium, id: `txt_date`, color: `On Primary`) — Published date
      - `Text` (body-medium, id: `txt_views`, color: `On Primary`) — View count
  - `Image` — Featured image (id: `img_featured`, width: `100%`, max-width: `1200px`, border-radius: `0`, margin: `0`)
  - `ColumnPanel` — Content column (id: `col_content`, max-width: `900px`, margin: `32px auto`, padding: `32px`)
    - `TextArea` — Post content (id: `txt_content`, appearance: `outlined`, disabled: `True`, line-height: `1.8`, margin-bottom: `32px`)
    - `ColumnPanel` — Pullquote (id: `col_pullquote`, tonal-card, padding: `24px`, margin: `24px 0`)
      - `Text` (body-large, italic, id: `txt_pullquote`) — Pullquote text
    - `FlowPanel` — Categories row (id: `flow_categories`, horizontal orientation, gap: 8px, margin-bottom: `32px`)
      - `Text` (caption, role: `category-badge`) — Category badge (multiple)
    - `ColumnPanel` — Comments section (id: `col_comments`)
      - Same structure as Classic template
    - `FlowPanel` — Related posts row (id: `flow_related`, horizontal orientation, gap: 16px, margin-top: `32px`)
      - `Link` — Related post link (multiple)
    - `FlowPanel` — Social sharing row (id: `flow_share`, horizontal orientation, gap: 16px, margin-top: `32px`)
      - `IconButton` — Facebook, Twitter, LinkedIn

**Responsive Behavior:**
- Desktop: Full-width hero, centered content max-width 900px
- Tablet: Full-width hero, centered content max-width 800px
- Mobile: Full-width hero, full-width content

**Code Implementation:**
```python
# In BlogPostFullWidthTemplate __init__
def __init__(self, post_id, **properties):
    self.init_components(**properties)
    self.post_id = post_id
    self._load_post()

def _load_post(self):
    """Load blog post data"""
    post = app_tables.blog_posts.get(id=self.post_id)
    
    self.hdg_title.text = post['title']
    self.txt_author.text = f"By {post['author']}"
    
    if post['published_at']:
        self.txt_date.text = post['published_at'].strftime('%B %d, %Y')
    
    self.txt_views.text = f"{post['view_count']} views"
    
    if post.get('featured_image'):
        self.img_featured.url = post['featured_image']
    
    self.txt_content.text = post['content']
    
    # Pullquote (if available)
    if post.get('pullquote'):
        self.col_pullquote.visible = True
        self.txt_pullquote.text = post['pullquote']
    else:
        self.col_pullquote.visible = False
```

#### Task 4.4: Blog Post Template Selector

**Location:** Blog Editor Form → "Select Post Template"

**Component Specification:**

**Form Name:** `BlogEditorForm` (enhanced)

**Anvil Components Used:**
- `Heading` (headline-small, id: `hdg_template_section`) — Text: "Post Template"
- `Text` (body-medium, id: `txt_template_description`) — Text: "Choose a layout for this blog post"
- `FlowPanel` — Template selector (id: `flow_template_selector`, horizontal orientation, gap: 16px, margin-bottom: `24px`)
  - `Button` (3 instances) — One per template
    - Button 1:
      - Text: "Classic"
      - icon: `description`
      - appearance: `outlined`
      - role: `template-button`
    - Button 2:
      - Text: "Two-Column"
      - icon: `sidebar`
      - appearance: `outlined`
      - role: `template-button`
    - Button 3:
      - Text: "Full-Width"
      - icon: `fullscreen`
      - appearance: `outlined`
      - role: `template-button`
- `ColumnPanel` — Content panel (id: `col_template_preview`, visible: `False`)
  - `Heading` (title-medium, id: `hdg_template_name`) — Template name
  - `Text` (body-medium, id: `txt_template_desc`) — Template description
  - `Card` (elevated, id: `card_preview`) — Preview card
    - `CardContentContainer` — Preview content (placeholder)
  - `Button` (filled, id: `btn_use_template`) — "Use This Template"

**Code Implementation:**
```python
# In BlogEditorForm __init__
def __init__(self, post=None, **properties):
    self.init_components(**properties)
    self.post = post
    self.current_template = None
    self.col_template_preview.visible = False

# Template button click handler
def btn_template_click(self, **event_args):
    template_name = event_args['sender'].text
    self._show_template_preview(template_name)

def _show_template_preview(self, template_name):
    """Show template preview"""
    self.col_template_preview.visible = True
    self.current_template = template_name.lower().replace('-', '')
    
    # Update preview content
    if template_name == "Classic":
        self.hdg_template_name.text = "Classic Centered"
        self.txt_template_desc.text = "Single-column layout with centered content. Best for long-form, editorial content."
        self.card_preview.role = "classic-preview"
    elif template_name == "Two-Column":
        self.hdg_template_name.text = "Two-Column with Sidebar"
        self.txt_template_desc.text = "Main content with sidebar for related posts, author bio, and categories."
        self.card_preview.role = "two-column-preview"
    elif template_name == "Full-Width":
        self.hdg_template_name.text = "Full-Width Immersive"
        self.txt_template_desc.text = "Full-width hero section with immersive reading experience."
        self.card_preview.role = "full-width-preview"

# Use template button handler
def btn_use_template_click(self, **event_args):
    if not self.current_template:
        anvil.show_info_message("Please select a template")
        return
    
    # Save template selection
    if self.post:
        self.post['template'] = self.current_template
    else:
        self.item['template'] = self.current_template
    
    # Load content editor with selected template
    self._load_content_editor()

def _load_content_editor(self):
    """Load content editor based on selected template"""
    # Clear current editor
    self.col_editor.clear()
    
    # Load appropriate template
    if self.current_template == 'classic':
        template = BlogPostClassicTemplate(post_id=self.post['id'] if self.post else None)
    elif self.current_template == 'twocolumn':
        template = BlogPostTwoColumnTemplate(post_id=self.post['id'] if self.post else None)
    elif self.current_template == 'fullwidth':
        template = BlogPostFullWidthTemplate(post_id=self.post['id'] if self.post else None)
    
    self.col_editor.add_component(template)
```

---

## Workstream 3: Custom M3 Components

### Stage 5: Custom Component Specifications

#### Task 5.1: TimeLapseCarouselComponent

**Component Name:** `TimeLapseCarouselComponent`

**Purpose:** Auto-rotating image carousel with manual navigation

**Anvil Components Used:**
- `ColumnPanel` — Carousel container (id: `col_carousel`, width: `100%`, position: `relative`, overflow: `hidden`)
- `FlowPanel` — Carousel track (id: `flow_track`, horizontal orientation, transition: `transform 0.3s ease`)
  - `Image` — Slides (multiple, id: `img_slide_*`, width: `100%`, height: `400px`, object-fit: `cover`)
- `IconButton` — Previous (id: `btn_prev`, icon: `chevron_left`, position: `absolute`, top: `50%`, left: `16px`, transform: `translateY(-50%)`)
- `IconButton` — Next (id: `btn_next`, icon: `chevron_right`, position: `absolute`, top: `50%`, right: `16px`, transform: `translateY(-50%)`)
- `FlowPanel` — Indicator dots (id: `flow_dots`, horizontal orientation, gap: 8px, position: `absolute`, bottom: `16px`, left: `50%`, transform: `translateX(-50%)`)
  - `Button` — Dot indicator (id: `btn_dot_*`, circular, 8px diameter, appearance: `text`)

**Properties:**
- `slide_images` — List of Image components
- `auto_rotate` — Boolean (default: `True`)
- `rotation_interval` — Integer (milliseconds, default: `5000`)
- `circular` — Boolean (default: `True`)

**Code Implementation:**
```python
# In TimeLapseCarouselComponent __init__
def __init__(self, slide_images=None, auto_rotate=True, rotation_interval=5000, circular=True, **properties):
    self.init_components(**properties)
    self.slide_images = slide_images or []
    self.auto_rotate = auto_rotate
    self.rotation_interval = rotation_interval
    self.circular = circular
    self.current_slide = 0
    self.timer = None
    
    self._setup_carousel()

def _setup_carousel(self):
    """Setup carousel with slides"""
    self.flow_track.clear()
    self.flow_dots.clear()
    
    for i, img in enumerate(self.slide_images):
        # Add slide image
        self.flow_track.add_component(img)
        
        # Add indicator dot
        dot = Button()
        dot.style = "width: 8px; height: 8px; border-radius: 50%; background-color: rgba(255,255,255,0.5);"
        dot.set_event_handler('click', lambda e, idx=i: self._go_to_slide(idx))
        self.flow_dots.add_component(dot)
    
    # Update active dot
    self._update_dots()
    
    # Setup navigation
    self.btn_prev.set_event_handler('click', lambda e: self._prev_slide())
    self.btn_next.set_event_handler('click', lambda e: self._next_slide())
    
    # Start auto-rotation
    if self.auto_rotate:
        self._start_rotation()

def _start_rotation(self):
    """Start auto-rotation"""
    if self.timer:
        self.timer.stop()
    
    self.timer = anvil.Timer()
    self.timer.interval = self.rotation_interval / 1000
    self.timer.set_event_handler('tick', lambda e: self._next_slide())
    self.timer.start()

def _next_slide(self):
    """Go to next slide"""
    total = len(self.slide_images)
    self.current_slide = (self.current_slide + 1) % total
    self._go_to_slide(self.current_slide)

def _prev_slide(self):
    """Go to previous slide"""
    total = len(self.slide_images)
    self.current_slide = (self.current_slide - 1) % total
    self._go_to_slide(self.current_slide)

def _go_to_slide(self, index):
    """Go to specific slide"""
    self.current_slide = index
    self._update_dots()
    
    # Move track
    offset = -index * 100
    self.flow_track.style = f"transform: translateX({offset}%)"

def _update_dots(self):
    """Update indicator dots"""
    dots = self.flow_dots.get_components()
    for i, dot in enumerate(dots):
        if i == self.current_slide:
            dot.style = "width: 8px; height: 8px; border-radius: 50%; background-color: white;"
        else:
            dot.style = "width: 8px; height: 8px; border-radius: 50%; background-color: rgba(255,255,255,0.5);"
```

#### Task 5.2: ParallaxComponent

**Component Name:** `ParallaxComponent`

**Purpose:** Parallax scroll effect with background image

**Anvil Components Used:**
- `ColumnPanel` — Parallax container (id: `col_parallax`, position: `relative`, overflow: `hidden`)
  - `Image` — Background image (id: `img_background`, position: `fixed`, width: `100%`, height: `400px`, object-fit: `cover`)
  - `ColumnPanel` — Foreground content (id: `col_foreground`, position: `relative`, z-index: `1`, padding: `48px 24px`)
    - `Heading` — Headline (id: `hdg_headline`)
    - `Text` — Body text (id: `txt_body`)
    - `Button` — CTA button (id: `btn_cta`)

**Properties:**
- `background_image` — Image component
- `parallax_speed` — Float (0.1-1.0, default: `0.5`)
- `height` — Integer (pixels, default: `400`)

**Code Implementation:**
```python
# In ParallaxComponent __init__
def __init__(self, background_image=None, parallax_speed=0.5, height=400, **properties):
    self.init_components(**properties)
    self.background_image = background_image
    self.parallax_speed = parallax_speed
    self.height = height
    
    self._setup_parallax()

def _setup_parallax(self):
    """Setup parallax effect"""
    if self.background_image:
        self.img_background.url = self.background_image.url
    
    self.col_parallax.style = f"height: {self.height}px;"
    
    # Add parallax JavaScript
    anvil.js.run_js("""
      (function() {
        var parallax = document.querySelector('.anvil-component-parallax');
        if (!parallax) return;
        
        var background = parallax.querySelector('.anvil-component-parallax-bg');
        var speed = {parallax_speed};
        
        window.addEventListener('scroll', function() {
          var scrolled = window.pageYOffset;
          var position = scrolled * speed;
          background.style.backgroundPosition = '50% ' + position + 'px';
        });
      })();
    """)
```

#### Task 5.3: AnimatedButtonComponent

**Component Name:** `AnimatedButtonComponent`

**Purpose:** Button with focus and selection animations

**Anvil Components Used:**
- `Button` — Base button component

**Properties:**
- `animate_on_focus` — Boolean (default: `True`)
- `animate_on_select` — Boolean (default: `True`)
- `animation_duration` — Float (seconds, default: `0.2`)

**Code Implementation:**
```python
# In AnimatedButtonComponent __init__
def __init__(self, animate_on_focus=True, animate_on_select=True, animation_duration=0.2, **properties):
    self.init_components(**properties)
    self.animate_on_focus = animate_on_focus
    self.animate_on_select = animate_on_select
    self.animation_duration = animation_duration
    
    self._setup_animations()

def _setup_animations(self):
    """Setup button animations"""
    # Add role for animations
    self.role = "animated-button"
    
    # Add focus event handler
    self.set_event_handler('got_focus', self._on_focus)
    self.set_event_handler('lost_focus', self._on_lost_focus)
    self.set_event_handler('selected', self._on_selected)
    self.set_event_handler('deselected', self._on_deselected)

def _on_focus(self, **event_args):
    """Handle focus event"""
    if self.animate_on_focus:
        self.style = f"transform: scale(1.05); box-shadow: 0 4px 8px rgba(0,0,0,0.2); transition: transform {self.animation_duration}s, box-shadow {self.animation_duration}s;"

def _on_lost_focus(self, **event_args):
    """Handle lost focus event"""
    if self.animate_on_focus:
        self.style = f"transform: scale(1); box-shadow: none; transition: transform {self.animation_duration}s, box-shadow {self.animation_duration}s;"

def _on_selected(self, **event_args):
    """Handle selected event"""
    if self.animate_on_select:
        self.style = f"background-color: var(--app-color-primary); color: var(--app-color-on-primary); transition: background-color {self.animation_duration}s, color {self.animation_duration}s;"

def _on_deselected(self, **event_args):
    """Handle deselected event"""
    if self.animate_on_select:
        self.style = f"background-color: ; color: ; transition: background-color {self.animation_duration}s, color {self.animation_duration}s;"
```

---

## Workstream 4: Footer Bar

### Stage 6: Footer Component

#### Task 6.1: Footer Component Specification

**Component Name:** `FooterComponent`

**Purpose:** Comprehensive footer with logo, credentials, social links, policies, map, and contact

**Anvil Components Used:**
- `ColumnPanel` — Footer container (id: `col_footer`, surface-variant background, padding: `48px 24px 24px`)
  - `FlowPanel` — Top row (id: `flow_top`, horizontal orientation, gap: 32px, flex-wrap: `wrap`, justify-content: `space-between`)
    - `ColumnPanel` — Logo section (id: `col_logo`, flex: `1`, min-width: `200px`)
      - `Image` — Business logo (id: `img_logo`, width: `150px`, height: `auto`, margin-bottom: `8px`)
      - `Text` (body-medium, id: `txt_business_name`) — Business name
    - `ColumnPanel` — Credentials section (id: `col_credentials`, flex: `1`, min-width: `200px`)
      - `Text` (caption, id: `txt_creds_1`) — Accreditation 1
      - `Text` (caption, id: `txt_creds_2`) — Accreditation 2
  - `FlowPanel` — Middle row (id: `flow_middle`, horizontal orientation, gap: 32px, flex-wrap: `wrap`, margin-top: `32px`)
    - `FlowPanel` — Social links (id: `flow_social`, horizontal orientation, gap: 16px)
      - `IconButton` — Facebook (id: `btn_fb`, icon: `facebook`)
      - `IconButton` — Instagram (id: `btn_ig`, icon: `instagram`)
      - `IconButton` — LinkedIn (id: `btn_li`, icon: `linkedin`)
      - `IconButton` — Twitter/X (id: `btn_tw`, icon: `twitter`)
    - `FlowPanel` — Policy links (id: `flow_policies`, horizontal orientation, gap: 16px, flex-wrap: `wrap`)
      - `Link` — Privacy Policy (id: `lnk_privacy`)
      - `Link` — Terms & Conditions (id: `lnk_terms`)
      - `Link` — Cookie Policy (id: `lnk_cookie`)
  - `FlowPanel` — Bottom row (id: `flow_bottom`, horizontal orientation, gap: 32px, flex-wrap: `wrap`, margin-top: `32px`)
    - `ColumnPanel` — Google Maps embed (id: `col_map`, flex: `1`, min-width: `300px`)
      - `HtmlTemplate` — Google Maps iframe (id: `html_map`, width: `100%`, height: `200px`)
    - `ColumnPanel` — Contact us section (id: `col_contact`, flex: `1`, min-width: `200px`)
      - `Heading` (title-medium, id: `hdg_contact`) — Text: "Contact Us"
      - `Text` (body-medium, id: `txt_email`) — Email address
      - `Text` (body-medium, id: `txt_phone`) — Phone number
      - `Button` (tertiary, id: `btn_contact_us`) — "Send Message" (links to Contact page)
  - `FlowPanel` — Copyright row (id: `flow_copyright`, horizontal orientation, gap: 16px, margin-top: `32px`, justify-content: `space-between`)
    - `Text` (caption, id: `txt_copyright`) — "© [Year] [Business Name]. All rights reserved."
    - `Link` — "Back to Top" (id: `lnk_back_top`) — Scrolls to top

**Settings Integration:**
- Logo upload: Dashboard → Settings → Business → "Business logo" (`FileLoader`)
- Credentials: Dashboard → Settings → Business → "Accreditations" (`TextArea`, one per line)
- Social links: Dashboard → Settings → Website → "Social media links" (`TextBox` fields for each platform)
- Google Maps: Dashboard → Settings → Website → "Google Maps embed URL" (`TextBox`)
- Contact info: Dashboard → Settings → Business → "Contact details" (`TextBox` fields for email and phone)

**Code Implementation:**
```python
# In FooterComponent __init__
def __init__(self, **properties):
    self.init_components(**properties)
    self._load_footer_data()

def _load_footer_data(self):
    """Load footer data from business_profile"""
    user = anvil.users.get_user()
    profile = app_tables.business_profile.get(instance_id=user)
    
    if not profile:
        return
    
    # Logo
    if profile.get('business_logo'):
        self.img_logo.url = profile['business_logo']
        self.img_logo.visible = True
    else:
        self.img_logo.visible = False
    
    # Business name
    self.txt_business_name.text = profile.get('business_name', 'Mybizz Business')
    
    # Credentials
    creds = profile.get('accreditations', '')
    if creds:
        cred_list = creds.split('\n')
        self.txt_creds_1.text = cred_list[0] if len(cred_list) > 0 else ''
        self.txt_creds_2.text = cred_list[1] if len(cred_list) > 1 else ''
        self.col_credentials.visible = True
    else:
        self.col_credentials.visible = False
    
    # Social links
    self._setup_social_links(profile)
    
    # Policies
    self.lnk_privacy.set_event_handler(
        'click',
        lambda e: open_form('PrivacyPolicyPage')
    )
    self.lnk_terms.set_event_handler(
        'click',
        lambda e: open_form('TermsPage')
    )
    self.lnk_cookie.set_event_handler(
        'click',
        lambda e: open_form('CookiePolicyPage')
    )
    
    # Google Maps
    map_url = profile.get('google_maps_embed_url', '')
    if map_url:
        self.html_template.code = f'<iframe src="{map_url}" width="100%" height="200" style="border:0;" allowfullscreen=""></iframe>'
    
    # Contact info
    self.txt_email.text = profile.get('contact_email', '')
    self.txt_phone.text = profile.get('contact_phone', '')
    
    self.btn_contact_us.set_event_handler(
        'click',
        lambda e: open_form('ContactPage')
    )
    
    # Copyright
    year = datetime.datetime.now().year
    self.txt_copyright.text = f"© {year} {profile.get('business_name', 'Mybizz Business')}. All rights reserved."
    
    # Back to top
    self.lnk_back_top.set_event_handler(
        'click',
        lambda e: anvil.js.run_js('window.scrollTo({top: 0, behavior: \'smooth\'});')
    )

def _setup_social_links(self, profile):
    """Setup social media links"""
    social_links = {
        'facebook': profile.get('social_facebook', ''),
        'instagram': profile.get('social_instagram', ''),
        'linkedin': profile.get('social_linkedin', ''),
        'twitter': profile.get('social_twitter', '')
    }
    
    for platform, url in social_links.items():
        btn = getattr(self, f'btn_{platform}', None)
        if btn and url:
            btn.set_event_handler('click', lambda e, url=url: anvil.js.open_url(url))
            btn.visible = True
        else:
            btn.visible = False
```

---

## Workstream 5: Homepage Template Improvements

### Stage 7: Enhanced Homepage Templates

#### Task 7.1: Improved Classic Template

**Component Name:** `HomePageClassicTemplate`

**Anvil Components Used:**
- `ColumnPanel` — Hero section (id: `col_hero`, padding: `64px 24px`, text-align: `center`)
  - `Heading` (headline-large, id: `hdg_headline`) — Welcome headline
  - `Text` (body-large, id: `txt_subheadline`) — Subheadline
  - `FlowPanel` — CTA buttons (id: `flow_cta`, horizontal orientation, gap: 16px, justify-content: `center`, margin-top: `24px`)
    - `Button` (filled, id: `btn_book_now`) — "Book Now"
    - `Button` (outlined, id: `btn_learn_more`) — "Learn More"
- `FlowPanel` — Services preview (id: `flow_services`, horizontal orientation, gap: 24px, flex-wrap: `wrap`, padding: `48px 24px`)
  - `Card` (elevated, id: `card_service_*`) — Service card (3 instances)
    - `CardContentContainer` — Inner content (padding: `16px`)
      - `Image` — Service image (id: `img_service`, width: `100%`, height: `200px`, object-fit: `cover`, border-radius: `8px`)
      - `Heading` (title-medium, id: `hdg_service_name`) — Service name
      - `Text` (body-medium, id: `txt_service_desc`) — Short description
      - `Button` (tertiary, id: `btn_view_details`) — "View Details"
- `ColumnPanel` — About section (id: `col_about`, padding: `48px 24px`)
  - `Heading` (headline-small, id: `hdg_about_title`) — "About Us"
  - `Text` (body-medium, id: `txt_about_text`) — About text
  - `Image` — Team/business image (id: `img_about`, width: `100%`, border-radius: `12px`, margin-top: `24px`)
- `ColumnPanel` — Testimonials (id: `col_testimonials`, surface-variant background, padding: `48px 24px`)
  - `Heading` (headline-small, id: `hdg_testimonials_title`) — "What Clients Say"
  - `RepeatingPanel` — Testimonial cards (id: `rp_testimonials`)
    - `Card` (tonal-card, id: `card_testimonial`)
      - `CardContentContainer` — Inner content (padding: `16px`)
        - `Text` (body-medium, italic, id: `txt_testimonial`) — Testimonial text
        - `FlowPanel` — Author row (id: `flow_author`, horizontal orientation, gap: 16px, margin-top: `16px`)
          - `Text` (title-medium, id: `txt_client_name`) — Client name
          - `Text` (caption, id: `txt_client_type`) — Business type
- `ColumnPanel` — CTA section (id: `col_cta`, primary-container background, padding: `48px 24px`, text-align: `center`)
  - `Heading` (headline-small, id: `hdg_cta_title`) — "Ready to Get Started?"
  - `Button` (filled, id: `btn_book_first`) — "Book Your First Session"

**Code Implementation:**
```python
# In HomePageClassicTemplate __init__
def __init__(self, **properties):
    self.init_components(**properties)
    self._load_homepage_data()

def _load_homepage_data(self):
    """Load homepage data from business_profile and services"""
    user = anvil.users.get_user()
    profile = app_tables.business_profile.get(instance_id=user)
    
    if not profile:
        return
    
    # Hero content
    self.hdg_headline.text = profile.get('hero_headline', 'Welcome to Our Business')
    self.txt_subheadline.text = profile.get('hero_subheadline', 'Professional services tailored to your needs')
    
    # Services preview
    self.flow_services.clear()
    services = app_tables.services.search(is_active=True)[:3]
    
    for service in services:
        card = Card()
        content = card.add_child(ColumnPanel(padding=16))
        
        img = Image()
        img.url = service.get('image')
        img.style = "width: 100%; height: 200px; object-fit: cover; border-radius: 8px;"
        content.add_component(img)
        
        hdg = Heading()
        hdg.text = service['name']
        hdg.text_size = "title/medium"
        content.add_component(hdg)
        
        txt = Text()
        txt.text = service.get('description', '')[:100] + "..."
        txt.text_size = "body/medium"
        content.add_component(txt)
        
        btn = Button()
        btn.text = "View Details"
        btn.appearance = "text"
        btn.set_event_handler(
            'click',
            lambda e, sid=service['id']: open_form('ServiceDetailForm', service_id=sid)
        )
        content.add_component(btn)
        
        self.flow_services.add_component(card)
    
    # About section
    self.txt_about_text.text = profile.get('about_text', '')
    
    if profile.get('about_image'):
        self.img_about.url = profile['about_image']
    
    # Testimonials
    self.rp_testimonials.item_source = app_tables.testimonials.search(order_by='created_at desc')[:3]
    
    # CTA
    self.hdg_cta_title.text = profile.get('cta_headline', 'Ready to Get Started?')
```

#### Task 7.2: Improved Services Template

**Component Name:** `HomePageServicesTemplate`

**Anvil Components Used:**
- `ColumnPanel` — Hero section (id: `col_hero`, padding: `64px 24px`, text-align: `center`)
  - `Heading` (headline-large, id: `hdg_headline`) — "Our Services"
  - `Text` (body-large, id: `txt_subheadline`) — Services overview
- `FlowPanel` — Services grid (id: `flow_services_grid`, horizontal orientation, gap: 24px, flex-wrap: `wrap`, padding: `48px 24px`)
  - `Card` (elevated, id: `card_service_*`) — Service card (multiple)
    - `CardContentContainer` — Inner content (padding: `16px`)
      - `Image` — Service image (id: `img_service`, width: `100%`, height: `200px`, object-fit: `cover`, border-radius: `8px`)
      - `Heading` (title-medium, id: `hdg_service_name`) — Service name
      - `Text` (body-medium, id: `txt_service_desc`) — Service description
      - `FlowPanel` — Service details (id: `flow_details`, horizontal orientation, gap: 16px, margin-top: `8px`)
        - `Text` (caption, id: `txt_duration`) — Duration/Price
        - `Text` (caption, id: `txt_provider`) — Provider
      - `Button` (filled, id: `btn_book_service`) — "Book This Service"
- `ColumnPanel` — Why Choose Us (id: `col_why_us`, padding: `48px 24px`)
  - `Heading` (headline-small, id: `hdg_why_title`) — "Why Choose Us"
  - `FlowPanel` — Benefits row (id: `flow_benefits`, horizontal orientation, gap: 24px, flex-wrap: `wrap`)
    - `ColumnPanel` — Benefit card (id: `col_benefit_*`, flex: `1`, min-width: `250px`)
      - `m3.Icon` — Benefit icon (id: `icon_benefit`, size: `48px`, color: `Primary`)
      - `Heading` (title-medium, id: `hdg_benefit_title`) — Benefit title
      - `Text` (body-medium, id: `txt_benefit_desc`) — Benefit description
- `ColumnPanel` — CTA section (id: `col_cta`, primary-container background, padding: `48px 24px`, text-align: `center`)
  - `Heading` (headline-small, id: `hdg_cta_title`) — "Ready to Get Started?"
  - `Button` (filled, id: `btn_book_now`) — "Book Your First Session"

**Code Implementation:**
```python
# In HomePageServicesTemplate __init__
def __init__(self, **properties):
    self.init_components(**properties)
    self._load_services_data()

def _load_services_data(self):
    """Load services data"""
    user = anvil.users.get_user()
    profile = app_tables.business_profile.get(instance_id=user)
    
    if not profile:
        return
    
    # Hero
    self.hdg_headline.text = "Our Services"
    self.txt_subheadline.text = profile.get('services_overview', '')
    
    # Services grid
    self.flow_services_grid.clear()
    services = app_tables.services.search(is_active=True)
    
    for service in services:
        card = Card()
        content = card.add_child(ColumnPanel(padding=16))
        
        # Image
        img = Image()
        img.url = service.get('image')
        img.style = "width: 100%; height: 200px; object-fit: cover; border-radius: 8px;"
        content.add_component(img)
        
        # Name
        hdg = Heading()
        hdg.text = service['name']
        hdg.text_size = "title/medium"
        content.add_component(hdg)
        
        # Description
        txt = Text()
        txt.text = service.get('description', '')
        txt.text_size = "body/medium"
        content.add_component(txt)
        
        # Details
        flow = FlowPanel(orientation='horizontal', gap=16)
        
        duration = Text()
        if service['pricing_model'] == 'duration':
            duration.text = f"{service['duration']} mins"
        else:
            duration.text = f"${service['price']}"
        duration.text_size = "caption"
        flow.add_component(duration)
        
        # Book button
        btn = Button()
        btn.text = "Book This Service"
        btn.appearance = "filled"
        btn.set_event_handler(
            'click',
            lambda e, sid=service['id']: open_form('BookingForm', service_id=sid)
        )
        content.add_component(btn)
        
        self.flow_services_grid.add_component(card)
    
    # Why Choose Us
    self._load_benefits()
```

#### Task 7.3: Improved Booking Template

**Component Name:** `HomePageBookingTemplate`

**Anvil Components Used:**
- `ColumnPanel` — Hero section (id: `col_hero`, padding: `64px 24px`, text-align: `center`)
  - `Heading` (headline-large, id: `hdg_headline`) — "Book Your Appointment"
  - `Text` (body-large, id: `txt_subheadline`) — Booking CTA
- `Card` (elevated, id: `card_quick_booking`, max-width: `400px`, margin: `32px auto`)
  - `CardContentContainer` — Inner content (padding: `24px`)
    - `Heading` (title-medium, id: `hdg_quick_title`) — "Quick Booking"
    - `DropdownMenu` — Service selector (id: `dd_service`, appearance: `outlined`)
    - `DropdownMenu` — Provider selector (id: `dd_provider`, appearance: `outlined`)
    - `DatePicker` — Date picker (id: `dp_date`, appearance: `outlined`)
    - `Button` (filled, id: `btn_check_availability`) — "Check Availability"
- `ColumnPanel` — Services preview (id: `col_services_preview`, padding: `48px 24px`)
  - Same structure as Services template
- `ColumnPanel` — Testimonials (id: `col_testimonials`, surface-variant background, padding: `48px 24px`)
  - Same structure as Classic template
- `ColumnPanel` — CTA section (id: `col_cta`, primary-container background, padding: `48px 24px`, text-align: `center`)
  - Same structure as Classic template

**Code Implementation:**
```python
# In HomePageBookingTemplate __init__
def __init__(self, **properties):
    self.init_components(**properties)
    self._load_booking_data()

def _load_booking_data(self):
    """Load booking form data"""
    # Services dropdown
    services = app_tables.services.search(is_active=True)
    service_items = ["Select a service"] + [s['name'] for s in services]
    self.dd_service.items = service_items
    
    # Providers dropdown
    providers = app_tables.contacts.search(role='staff')
    provider_items = ["Select a provider"] + [p['name'] for p in providers]
    self.dd_provider.items = provider_items
    
    # Check availability button
    self.btn_check_availability.set_event_handler(
        'click',
        lambda e: self._check_availability()
    )

def _check_availability(self):
    """Check availability and navigate to booking"""
    service_idx = self.dd_service.selected_index
    provider_idx = self.dd_provider.selected_index
    
    if service_idx <= 0 or provider_idx <= 0:
        anvil.show_info_message("Please select both service and provider")
        return
    
    service = app_tables.services.search(is_active=True)[service_idx - 1]
    provider = app_tables.contacts.search(role='staff')[provider_idx - 1]
    
    open_form('BookingForm', service_id=service['id'], provider_id=provider['id'])
```

#### Task 7.4: Improved Minimalist Template

**Component Name:** `HomePageMinimalistTemplate`

**Anvil Components Used:**
- `ColumnPanel` — Hero section (id: `col_hero`, padding: `96px 24px`, text-align: `center`)
  - `Heading` (headline-large, id: `hdg_headline`) — Simple headline
  - `Text` (body-medium, id: `txt_subheadline`) — Minimal subheadline
  - `Button` (filled, id: `btn_primary_cta`) — Primary CTA (single button)
- `FlowPanel` — Services row (id: `flow_services`, horizontal orientation, gap: 24px, flex-wrap: `wrap`, padding: `48px 24px`)
  - `Card` (surface-card, id: `card_service_*`) — Service card (3 instances)
    - `CardContentContainer` — Inner content (padding: `16px`)
      - `Heading` (title-medium, id: `hdg_service_name`) — Service name
      - `Button` (tertiary, id: `btn_service_more`) — "Learn More"
- `ColumnPanel` — About snippet (id: `col_about`, padding: `48px 24px`, text-align: `center`)
  - `Heading` (headline-small, id: `hdg_about_title`) — "About"
  - `Text` (body-medium, id: `txt_about_text`) — Short about text (2-3 lines)
  - `Button` (tertiary, id: `btn_about_more`) — "Read More"
- `ColumnPanel` — Contact snippet (id: `col_contact`, padding: `48px 24px`, text-align: `center`)
  - `Heading` (headline-small, id: `hdg_contact_title`) — "Contact"
  - `Text` (body-medium, id: `txt_contact_email`) — Email address
  - `Text` (body-medium, id: `txt_contact_phone`) — Phone number
  - `Button` (tertiary, id: `btn_contact`) — "Get in Touch"

**Code Implementation:**
```python
# In HomePageMinimalistTemplate __init__
def __init__(self, **properties):
    self.init_components(**properties)
    self._load_minimalist_data()

def _load_minimalist_data(self):
    """Load minimalist homepage data"""
    user = anvil.users.get_user()
    profile = app_tables.business_profile.get(instance_id=user)
    
    if not profile:
        return
    
    # Hero
    self.hdg_headline.text = profile.get('minimalist_headline', 'Simple. Effective. Yours.')
    self.txt_subheadline.text = profile.get('minimalist_subheadline', '')
    
    # Services
    self.flow_services.clear()
    services = app_tables.services.search(is_active=True)[:3]
    
    for service in services:
        card = Card()
        content = card.add_child(ColumnPanel(padding=16))
        
        hdg = Heading()
        hdg.text = service['name']
        hdg.text_size = "title/medium"
        content.add_component(hdg)
        
        btn = Button()
        btn.text = "Learn More"
        btn.appearance = "text"
        btn.set_event_handler(
            'click',
            lambda e, sid=service['id']: open_form('ServiceDetailForm', service_id=sid)
        )
        content.add_component(btn)
        
        self.flow_services.add_component(card)
    
    # About
    self.hdg_about_title.text = "About"
    self.txt_about_text.text = profile.get('about_text', '')[:100] + "..."
    
    # Contact
    self.hdg_contact_title.text = "Contact"
    self.txt_contact_email.text = profile.get('contact_email', '')
    self.txt_contact_phone.text = profile.get('contact_phone', '')
```

#### Task 7.5: Homepage Template Selector

**Location:** Dashboard → Settings → Website → "Homepage template"

**Component Specification:**

**Form Name:** `HomepageTemplateSelectorForm`

**Anvil Components Used:**
- `Heading` (headline-small, id: `hdg_template_title`) — Text: "Homepage Template"
- `Text` (body-medium, id: `txt_template_description`) — Text: "Choose a template for your homepage"
- `FlowPanel` — Template selector (id: `flow_template_selector`, horizontal orientation, gap: 16px, margin-bottom: `24px`)
  - `Button` (4 instances) — One per template
    - Button 1:
      - Text: "Classic"
      - icon: `home`
      - appearance: `outlined`
      - role: `template-button`
    - Button 2:
      - Text: "Services"
      - icon: `services`
      - appearance: `outlined`
      - role: `template-button`
    - Button 3:
      - Text: "Booking"
      - icon: `event`
      - appearance: `outlined`
      - role: `template-button`
    - Button 4:
      - Text: "Minimalist"
      - icon: `minimize`
      - appearance: `outlined`
      - role: `template-button`
- `ColumnPanel` — Preview content panel (id: `col_preview`, visible: `False`)
  - `Heading` (title-medium, id: `hdg_preview_title`) — Template name
  - `Text` (body-medium, id: `txt_preview_desc`) — Template description
  - `Card` (elevated, id: `card_preview`) — Preview card
    - `CardContentContainer` — Preview content (placeholder image/text)
  - `Button` (filled, id: `btn_apply_template`) — "Apply Template"

**Code Implementation:**
```python
# In HomepageTemplateSelectorForm __init__
def __init__(self, **properties):
    self.init_components(**properties)
    self.current_template = None
    self.col_preview.visible = False

# Template button click handler
def btn_template_click(self, **event_args):
    template_name = event_args['sender'].text
    self._show_preview(template_name)

def _show_preview(self, template_name):
    """Show template preview"""
    self.col_preview.visible = True
    self.current_template = template_name.lower()
    
    # Update preview content
    if template_name == "Classic":
        self.hdg_preview_title.text = "Classic"
        self.txt_preview_desc.text = "Traditional layout with hero, services, about, testimonials, and CTA sections."
    elif template_name == "Services":
        self.hdg_preview_title.text = "Services"
        self.txt_preview_desc.text = "Service-focused layout with prominent service showcase."
    elif template_name == "Booking":
        self.hdg_preview_title.text = "Booking"
        self.txt_preview_desc.text = "Quick booking form in hero section for fast conversions."
    elif template_name == "Minimalist":
        self.hdg_preview_title.text = "Minimalist"
        self.txt_preview_desc.text = "Clean, minimal design with essential information only."

# Apply template button handler
def btn_apply_template_click(self, **event_args):
    if not self.current_template:
        anvil.show_info_message("Please select a template")
        return
    
    # Save template selection
    user = anvil.users.get_user()
    profile = app_tables.business_profile.get(instance_id=user)
    
    profile['homepage_template'] = self.current_template
    
    anvil.show_info_message(f"{self.current_template.capitalize()} template applied")
    
    # Reload homepage
    anvil.js.run_js('window.location.reload();')
```

---

## Workstream 6: Blog Comments & SEO

### Stage 8: Blog Comments System

#### Task 8.1: Comments Database Schema

**New Table:** `blog_comments`

| Column | Type | Notes |
|--------|------|-------|
| `id` | Row ID | Auto-generated primary key |
| `instance_id` | Link to `users` | Tenant isolation (linked to `users` table) |
| `post_id` | Link to `blog_posts` | Parent post (linked to `blog_posts` table) |
| `commenter_name` | Text | Commenter's name (required) |
| `commenter_email` | Text | Commenter's email (required) |
| `comment_text` | Text | Comment body (required) |
| `status` | Text | "pending", "approved", "spam", "deleted" |
| `created_at` | Date and Time | UTC timestamp |
| `updated_at` | Date and Time | UTC timestamp |

**Index:** `post_id + status` for efficient filtering

#### Task 8.2: Comments Toggle in Settings

**Location:** Dashboard → Settings → Blog → "Enable comments"

**Component Specification:**

**Form Name:** `BlogSettingsForm` (enhanced)

**Anvil Components Used:**
- `Heading` (headline-small, id: `hdg_blog_settings`) — Text: "Blog Settings"
- `ColumnPanel` — Main container (padding: `24px`)
  - `FlowPanel` — Comments toggle row (id: `flow_comments_toggle`, horizontal orientation, gap: 16px, padding: `16px 0`)
    - `Text` (body-medium, id: `txt_comments_label`) — Text: "Enable comments on blog posts"
    - `m3.Switch` — Comments toggle (id: `sw_comments`, role: `settings-switch`)
  - `Text` (caption, id: `txt_comments_desc_1`) — Text: "Allow readers to post comments on blog posts"
  - `Text` (caption, id: `txt_comments_desc_2`) — Text: "Comments require manual approval before publishing"

**Code Implementation:**
```python
# In BlogSettingsForm __init__
def __init__(self, **properties):
    self.init_components(**properties)
    self._load_current_setting()

def _load_current_setting(self):
    """Load current comments setting"""
    user = anvil.users.get_user()
    profile = app_tables.business_profile.get(instance_id=user)
    
    if profile and profile.get('enable_comments') is not None:
        self.sw_comments.checked = profile['enable_comments']
    else:
        # Default: enabled
        self.sw_comments.checked = True

def sw_comments_checked_change(self, **event_args):
    """Handle comments toggle change"""
    user = anvil.users.get_user()
    profile = app_tables.business_profile.get(instance_id=user)
    
    profile['enable_comments'] = self.sw_comments.checked
    
    anvil.show_info_message("Comments setting saved")
```

#### Task 8.3: Comment Moderation Workflow

**Location:** Dashboard → Blog → Comments (new form)

**Component Specification:**

**Form Name:** `CommentModerationForm`

**Anvil Components Used:**
- `Heading` (headline-large, id: `hdg_moderation_title`) — Text: "Moderate Comments"
- `FlowPanel` — Filter row (id: `flow_filters`, horizontal orientation, gap: 16px, padding: `16px 0`)
  - `DropdownMenu` — Status filter (id: `dd_status_filter`, appearance: `outlined`)
    - Items: ["All", "Pending", "Approved", "Spam"]
    - Default: "Pending"
  - `DropdownMenu` — Post filter (id: `dd_post_filter`, appearance: `outlined`)
    - Items: Dynamically loaded from blog_posts
- `DataGrid` — Comments list (id: `dg_comments`)
  - Columns:
    - Post Title (text)
    - Commenter (text)
    - Comment Text (text, max 50 chars)
    - Status (text)
    - Date (date)
    - Actions (button menu)
- `RepeatingPanel` — Comments detail (id: `rp_comment_detail`)
  - `ColumnPanel` — Comment card (role: `surface-card`, padding: `16px`, margin-bottom: `16px`)
    - `Heading` (title-medium, id: `hdg_commenter_name`) — Commenter name
    - `Text` (caption, id: `txt_post_title`) — Post title
    - `Text` (body-medium, id: `txt_comment_text`) — Comment text
    - `Text` (caption, id: `txt_comment_date`) — Date
    - `FlowPanel` — Action buttons (id: `flow_actions`, horizontal orientation, gap: 8px)
      - `Button` (filled, id: `btn_approve`) — "Approve"
      - `Button` (outlined, id: `btn_delete`) — "Delete"
      - `Button` (outlined, id: `btn_spam`) — "Mark as Spam"

**Code Implementation:**
```python
# In CommentModerationForm __init__
def __init__(self, **properties):
    self.init_components(**properties)
    self._load_comments()
    self._load_posts()

def _load_comments(self):
    """Load comments based on filter"""
    status_filter = self.dd_status_filter.items[self.dd_status_filter.selected_index]
    
    if status_filter == "All":
        comments = app_tables.blog_comments.search(order_by='created_at desc')
    else:
        comments = app_tables.blog_comments.search(status=status_filter.lower(), order_by='created_at desc')
    
    self.dg_comments.item_source = comments

def bind_row(self, row_panel):
    """Bind comment data to data grid row"""
    comment = row_panel.item
    
    # Post title
    if comment['post_id']:
        row_panel['Post Title'] = comment['post_id']['title']
    else:
        row_panel['Post Title'] = 'Deleted Post'
    
    # Commenter
    row_panel['Commenter'] = comment['commenter_name']
    
    # Comment text (truncated)
    text = comment['comment_text']
    row_panel['Comment Text'] = text[:50] + "..." if len(text) > 50 else text
    
    # Status
    row_panel['Status'] = comment['status'].capitalize()
    
    # Date
    row_panel['Date'] = comment['created_at'].strftime('%Y-%m-%d')

def _load_posts(self):
    """Load posts for filter dropdown"""
    posts = app_tables.blog_posts.search(order_by='title')
    post_items = ["All Posts"] + [post['title'] for post in posts]
    self.dd_post_filter.items = post_items

def btn_approve_click(self, **event_args):
    """Approve selected comment"""
    comment = self.dg_comments.selected_row
    if comment:
        comment['status'] = 'approved'
        comment['updated_at'] = datetime.datetime.now()
        anvil.show_info_message("Comment approved")

def btn_delete_click(self, **event_args):
    """Delete selected comment"""
    comment = self.dg_comments.selected_row
    if comment:
        comment.delete()
        anvil.show_info_message("Comment deleted")

def btn_spam_click(self, **event_args):
    """Mark comment as spam"""
    comment = self.dg_comments.selected_row
    if comment:
        comment['status'] = 'spam'
        comment['updated_at'] = datetime.datetime.now()
        anvil.show_info_message("Comment marked as spam")
```

---

## Workstream 7: SEO & Social Sharing

### Stage 9: SEO Meta Tags

#### Task 9.1: Simple SEO Meta Tag Implementation

**Location:** Blog Editor Form → "SEO Settings" section

**Design Principle:** Simple, obvious, easy to use

**Component Specification:**

**Form Name:** `BlogEditorForm` (enhanced)

**Anvil Components Used:**
- `Heading` (headline-small, id: `hdg_seo_section`) — Text: "SEO Settings"
- `ColumnPanel` — SEO container (padding: `16px`, margin-top: `24px`, tonal-card)
  - `TextBox` — Meta title (id: `txt_meta_title`, appearance: `outlined`, placeholder: "Page title (max 60 characters)")
    - `Text` (caption, id: `txt_meta_title_count`) — Character count (e.g., "45/60")
  - `TextBox` — Meta description (id: `txt_meta_description`, appearance: `outlined`, placeholder: "Page description (max 160 characters)")
    - `Text` (caption, id: `txt_meta_desc_count`) — Character count (e.g., "120/160")
  - `TextBox` — Keywords/tags (id: `txt_keywords`, appearance: `outlined`, placeholder: "comma-separated tags")
    - `Text` (caption, id: `txt_keywords_hint`) — Text: "Helps search engines understand your content"

**Code Implementation:**
```python
# In BlogEditorForm __init__
def __init__(self, post=None, **properties):
    self.init_components(**properties)
    self.post = post
    self._load_seo_data()
    self._setup_character_counters()

def _load_seo_data(self):
    """Load existing SEO data"""
    if self.post:
        self.txt_meta_title.text = self.post.get('meta_title', '')
        self.txt_meta_description.text = self.post.get('meta_description', '')
        self.txt_keywords.text = ','.join(self.post.get('keywords', []))
    else:
        self.txt_meta_title.text = ''
        self.txt_meta_description.text = ''
        self.txt_keywords.text = ''

def _setup_character_counters(self):
    """Setup character count watchers"""
    self.txt_meta_title.set_event_handler('change', self._update_meta_title_count)
    self.txt_meta_description.set_event_handler('change', self._update_meta_desc_count)

def _update_meta_title_count(self, **event_args):
    """Update meta title character count"""
    count = len(self.txt_meta_title.text)
    self.txt_meta_title_count.text = f"{count}/60"
    
    if count > 60:
        self.txt_meta_title_count.role = "error-text"
    else:
        self.txt_meta_title_count.role = "caption"

def _update_meta_desc_count(self, **event_args):
    """Update meta description character count"""
    count = len(self.txt_meta_description.text)
    self.txt_meta_desc_count.text = f"{count}/160"
    
    if count > 160:
        self.txt_meta_desc_count.role = "error-text"
    else:
        self.txt_meta_desc_count.role = "caption"

def btn_save_click(self, **event_args):
    """Save blog post with SEO data"""
    # Save SEO fields
    if self.post:
        self.post['meta_title'] = self.txt_meta_title.text[:60]
        self.post['meta_description'] = self.txt_meta_description.text[:160]
        keywords = [k.strip() for k in self.txt_keywords.text.split(',') if k.strip()]
        self.post['keywords'] = keywords
    else:
        self.item['meta_title'] = self.txt_meta_title.text[:60]
        self.item['meta_description'] = self.txt_meta_description.text[:160]
        keywords = [k.strip() for k in self.txt_keywords.text.split(',') if k.strip()]
        self.item['keywords'] = keywords
    
    # Continue with save...
```

**Public Page SEO Integration:**

**Location:** All blog post templates

**Implementation:**
```python
# In BlogPostClassicTemplate __init__
def _load_post(self):
    """Load blog post with SEO data"""
    post = app_tables.blog_posts.get(id=self.post_id)
    
    # Set page title
    page_title = post.get('meta_title', post['title'])
    anvil.js.run_js(f'document.title = "{page_title}";')
    
    # Set meta description
    meta_desc = post.get('meta_description', post.get('excerpt', '')[:160])
    anvil.js.run_js(f'''
      var metaDesc = document.querySelector('meta[name="description"]');
      if (!metaDesc) {{
        metaDesc = document.createElement('meta');
        metaDesc.name = 'description';
        document.head.appendChild(metaDesc);
      }}
      metaDesc.content = '{meta_desc}';
    ''')
    
    # Set keywords
    keywords = post.get('keywords', [])
    if keywords:
        anvil.js.run_js(f'''
          var metaKeywords = document.querySelector('meta[name="keywords"]');
          if (!metaKeywords) {{
            metaKeywords = document.createElement('meta');
            metaKeywords.name = 'keywords';
            document.head.appendChild(metaKeywords);
          }}
          metaKeywords.content = '{','.join(keywords)}';
        ''')
```

---

### Stage 10: Social Sharing Buttons

#### Task 10.1: Social Sharing Implementation

**Location:** All blog post templates (Classic, Two-Column, Full-Width)

**Component Specification:**

**Anvil Components Used:**
- `FlowPanel` — Social sharing row (id: `flow_share`, horizontal orientation, gap: 16px, margin-top: `32px`)
  - `IconButton` — Facebook (id: `btn_share_fb`, icon: `facebook`, role: `social-icon`)
  - `IconButton` — Twitter (id: `btn_share_tw`, icon: `twitter`, role: `social-icon`)
  - `IconButton` — LinkedIn (id: `btn_share_li`, icon: `linkedin`, role: `social-icon`)

**Code Implementation:**
```python
# In BlogPostClassicTemplate __init__
def _load_post(self):
    """Load blog post and setup social sharing"""
    post = app_tables.blog_posts.get(id=self.post_id)
    
    # Get current page URL
    page_url = anvil.js.run_js('window.location.href;')
    
    # Facebook share
    fb_url = f'https://www.facebook.com/sharer/sharer.php?u={page_url}'
    self.btn_share_fb.set_event_handler(
        'click',
        lambda e, url=fb_url: anvil.js.open_url(url, '_blank')
    )
    
    # Twitter share
    tw_url = f'https://twitter.com/intent/tweet?url={page_url}&text={post["title"]}'
    self.btn_share_tw.set_event_handler(
        'click',
        lambda e, url=tw_url: anvil.js.open_url(url, '_blank')
    )
    
    # LinkedIn share
    li_url = f'https://www.linkedin.com/shareArticle?mini=true&url={page_url}&title={post["title"]}'
    self.btn_share_li.set_event_handler(
        'click',
        lambda e, url=li_url: anvil.js.open_url(url, '_blank')
    )
```

---

## Final Deliverables

### Phase 1: Palette System Foundation

**Stage 1: Palette Definition**
- [x] Task 1.1: Define 4 M3-compatible palettes in `theme.css`
- [x] Task 1.2: Create palette selector component (`PaletteSettingsForm`)
- [x] Task 1.3: Implement palette switching logic in `business_profile` table

**Stage 2: Palette Integration**
- [x] Task 2.1: Add palette selector to Settings screen
- [x] Task 2.2: Add palette selector to onboarding flow
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

**Stage 6: Block Pages**
- [x] Task 6.1: Create `VideoDisplayComponent` component (YouTube only)
- [x] Task 6.2: Create `LeadCaptureTemplate` component
- [x] Task 6.3: Create `EventRegistrationTemplate` component
- [x] Task 6.4: Create `VslTemplate` component

### Phase 3: Homepage Improvements

**Stage 7: Enhanced Templates**
- [x] Task 7.1: Improve `HomePageClassicTemplate` component
- [x] Task 7.2: Improve `HomePageServicesTemplate` component
- [x] Task 7.3: Improve `HomePageBookingTemplate` component
- [x] Task 7.4: Improve `HomePageMinimalistTemplate` component
- [x] Task 7.5: Implement homepage template selector

### Phase 4: Footer and Custom Components

**Stage 8: Footer Component**
- [x] Task 8.1: Create `FooterComponent` component
- [x] Task 8.2: Integrate footer into all public pages
- [x] Task 8.3: Add footer settings to Dashboard

**Stage 9: Custom M3 Components**
- [x] Task 9.1: Create `TimeLapseCarouselComponent` component
- [x] Task 9.2: Create `ParallaxComponent` component
- [x] Task 9.3: Create `AnimatedButtonComponent` component
- [x] Task 9.4: Create `AnimatedCardComponent` component
- [x] Task 9.5: Create `AnimatedLinkComponent` component
- [x] Task 9.6: Create `AnimatedImageComponent` component

### Phase 5: SEO & Social

**Stage 10: SEO Implementation**
- [x] Task 10.1: Implement simple SEO meta tags in Blog Editor
- [x] Task 10.2: Add SEO data to blog post templates
- [x] Task 10.3: Add character counters for meta fields

**Stage 11: Social Sharing**
- [x] Task 11.1: Add social sharing buttons to all blog post templates
- [x] Task 11.2: Implement Facebook, Twitter, LinkedIn sharing

### Phase 6: Documentation

**Stage 12: Design Documentation**
- [x] Task 12.1: Update `design-direction.md` with palette system
- [x] Task 12.2: Create `blog-templates.md` with detailed specifications
- [x] Task 12.3: Create `footer-component.md` with component specs
- [x] Task 12.4: Create `custom-components.md` with animation specs
- [x] Task 12.5: Update `wireframe-blog.html` with new variants

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

### Custom Components Created

| Component | Type | Dependencies |
|-----------|------|--------------|
| `PaletteSettingsForm` | Custom Component | Button, Heading, Text, Card, ColumnPanel |
| `OnboardingPaletteForm` | Custom Component | Button, Heading, Text, Card, ColumnPanel |
| `BlogListTiledTemplate` | Custom Component | Card, Image, Heading, Text, Button, TextBox, DropdownMenu |
| `BlogListListTemplate` | Custom Component | RepeatingPanel, Image, Heading, Text, Button, TextBox, DropdownMenu |
| `BlogPostClassicTemplate` | Custom Component | TextArea, Image, Heading, Text, Button, Link, m3.Icon |
| `BlogPostTwoColumnTemplate` | Custom Component | Image, TextArea, Heading, Text, Link, m3.Switch, Card |
| `BlogPostFullWidthTemplate` | Custom Component | TextArea, Image, Heading, Text, Button, Link |
| `VideoDisplayComponent` | Custom Component | Image, HtmlTemplate, Button |
| `LeadCaptureTemplate` | Custom Component | VideoDisplayComponent, TextBox, TextArea, Button |
| `EventRegistrationTemplate` | Custom Component | VideoDisplayComponent, TextBox, TextArea, Button |
| `VslTemplate` | Custom Component | VideoDisplayComponent, TextBox, TextArea, Button |
| `HomePageClassicTemplate` | Custom Component | Image, Heading, Text, Button, Card, RepeatingPanel |
| `HomePageServicesTemplate` | Custom Component | Image, Heading, Text, Button, Card, m3.Icon |
| `HomePageBookingTemplate` | Custom Component | DatePicker, DropdownMenu, Image, Heading, Text, Button |
| `HomePageMinimalistTemplate` | Custom Component | Image, Heading, Text, Button |
| `FooterComponent` | Custom Component | Image, IconButton, Link, HtmlTemplate, Button, Text |
| `TimeLapseCarouselComponent` | Custom Component | Image, IconButton, Button, Timer |
| `ParallaxComponent` | Custom Component | Image, ColumnPanel, Heading, Text, Button |
| `AnimatedButtonComponent` | Custom Component | Button |
| `AnimatedCardComponent` | Custom Component | Card, ColumnPanel |
| `AnimatedLinkComponent` | Custom Component | Link |
| `AnimatedImageComponent` | Custom Component | Image |

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
- Anvil M3 theme tokens (`%color:Token%`)
- Built-in M3 component properties
- Anvil roles for semantic styling
- CSS custom properties defined in `theme.css`

### JavaScript Usage

Minimal JavaScript used for:
- Palette application (setting `data-palette` attribute)
- Parallax scroll effect
- Social sharing (opening URLs in new tabs)
- Character counters for SEO fields
- Back to top scroll

### Performance Considerations

- Lazy loading for blog post comments
- Pagination for comment moderation
- Optimized image loading with proper dimensions
- Client-side palette caching via localStorage

---

## Open Questions

All questions have been answered in the user's response:

1. ✅ Video Display: YouTube only
2. ✅ Comments Moderation: Manual moderation
3. ✅ Analytics: Anvil built-in analytics for view counts
4. ✅ SEO Meta Tags: Simple, obvious implementation with character counters
5. ✅ Social Sharing: Facebook, Twitter, LinkedIn buttons included
6. ✅ Related Posts: By tag matching

---

## Next Steps

1. **Review and Approve:** Review this plan and confirm all specifications
2. **Implementation:** Begin with Workstream 1 (Palette System) as foundation
3. **Testing:** Test palette switching across all pages
4. **Documentation:** Update all design docs with new components
5. **Deployment:** Deploy to test instance for user validation

---

*End of Design Consultation Plan*
