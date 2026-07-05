# Compliance Checklist — screen-blog-BlogPostForm

**Screen:** `/mnt/c/_mb2-cs-app/project-library/screens/blog/BlogPostForm/screen-blog-BlogPostForm.html`
**Wireframe:** `/mnt/c/_mb2-cs-app/project-library/wireframes/blog/BlogPostForm/wireframe-blog-BlogPostForm.html`
**Date:** 2026-07-02
**Revised:** 2026-07-02 — honest audit, 3 issues found and fixed

---

## A. Wireframe fidelity

- [x] Every component in the wireframe's Section 2 appears in the screen
  - `col_blog_post` (ColumnPanel) ✓
  - `card_post_header` (Card) [elevated] — category badge, title, meta ✓
  - `img_featured` (Image) ✓
  - `card_content` (Card) [outlined] — post content ✓
  - `card_pullquote` — excluded (hidden on load, dashed) ✓
  - `card_video` — excluded (hidden on load, dashed) ✓
  - `flow_sharing` (FlowPanel) — 3 share buttons ✓
  - `card_sidebar` — excluded (hidden on load, dashed) ✓
  - `card_comments` (Card) [outlined] — heading, RP placeholder, comment form ✓
  - `footer_component` (FooterComponent) ✓
  - `col_empty_comments` — excluded (hidden on load, dashed) ✓
- [x] No extra components
- [x] Component hierarchy matches wireframe
- [x] Component types match wireframe

## B. Page chrome exclusion

- [x] No left sidebar, app bar, footer, or navigation outside wireframe boundary
- [x] The screen contains ONLY what the wireframe declares

## C. Repeating panels and row templates

- [x] RepeatingPanel row content is NOT rendered (rp_comments)
- [x] Host screen shows placeholder annotation

## D. Sample data (per Standard §8)

- [x] Sample data is South African consulting/services vertical
- [x] Author name: "Thandi Nkosi" (plausible SA name)
- [x] Date: "15 June 2026"
- [x] No "Lorem ipsum"
- [x] No production data

## E. MUI aesthetic overlay (all 9 variables)

- [x] 1. Surface distinction — body `var(--surface)`
- [x] 2. Card shadow — `var(--mui-shadow-card)` (hsla dual-layer)
- [x] 3. Card radius — 8px (verified: `.card-post-header`, `.card-content`, `.card-comments` all `border-radius: 8px`)
- [x] 4. Typography heading weight — 600 (verified: `.hdg-post-title` `font-weight: 600`)
- [x] 5. Dividers — outline-variant at 40% opacity (verified: `.flow-sharing` uses `var(--divider-color)`)
- [x] 6. State hover opacity — 0.08 via `--btn-outlined-hover: color-mix(in srgb, var(--primary) 8%, transparent)`
- [x] 7. State pressed/focused opacity — 0.12 via CSS custom property
- [x] 8. App bar — not applicable (no app bar in wireframe)
- [x] 9. Left nav — not applicable (no left nav in wireframe)

## F. Colour and token compliance

- [x] No hardcoded hex values — verified via grep: zero matches for `#[0-9a-fA-F]{6}`
- [x] No hardcoded rgb()/rgba() — verified via grep: zero matches for `rgba(\d`. Button hover uses `color-mix()` via CSS custom properties. MUI shadow uses `hsla()` (exempt).
- [x] All colors resolve through CSS custom properties from Professional Blue palette
- [x] Status colours — none used in this form

**Previously failing — now fixed:**
- ~~FAIL: `rgba(73,62,243,0.92)` on `.btn-filled:hover`~~ → now `var(--btn-filled-hover)` using `color-mix()`
- ~~FAIL: `rgba(73,62,243,0.08)` on `.btn-outlined:hover`~~ → now `var(--btn-outlined-hover)` using `color-mix()`
- ~~FAIL: `rgba(0,0,0,0.3)` on button box-shadow~~ → now `var(--btn-shadow-hover)` using hsla (MUI shadow format)

## G. Component compliance

- [x] No `Label` component — verified
- [x] No `HTMLTemplate` component — verified
- [x] No `SidesheetContent` component — verified
- [x] No `HtmlPanel` component — verified
- [x] FooterComponent is an approved custom component per Standard §7
- [x] All other components are from the approved M3 list

## H. Naming and structure

- [x] Component names follow `nomenclature.md` prefixes (hdg_, txt_, btn_, nav_, card_, col_, flow_, img_, rp_, footer_)
- [x] Material Icons imported (`<link href="https://fonts.googleapis.com/icon?family=Material+Icons">`)
- [x] `footer_component` uses correct naming for FooterComponent

## I. Output quality

- [x] Screen renders correctly in a browser
- [x] No broken layout or overflow issues
- [x] Typography matches the project scale (Roboto, 32px/600 title, 14px body)
- [x] Spacing follows the 4px grid — all values use `var(--space-*)` tokens
- [x] Button min-height 40px — verified via grep: `min-height: 40px`
- [x] Form input min-height 48px — verified
- [x] Max-width 900px — matches Public page type per Standard §11
- [x] Dark mode — `@media (prefers-color-scheme: dark)` block present with surface/outline token overrides
- [x] Reduced motion — `@media (prefers-reduced-motion: reduce)` present

**Previously failing — now fixed:**
- ~~FAIL: `.btn` had `min-height: 36px`~~ → corrected to `min-height: 40px`
- ~~MISSING: No dark mode block~~ → added `@media (prefers-color-scheme: dark)` with 7 token overrides

---

## Discrepancies found

**None remaining.** Three issues were found during audit and fixed:
1. Hardcoded `rgba()` on button hover states → replaced with `color-mix()` via CSS custom properties
2. Button `min-height: 36px` → corrected to `40px`
3. Missing dark mode block → added with surface/outline token overrides

---

*End of checklist — screen-blog-BlogPostForm (revised 2026-07-02)*
