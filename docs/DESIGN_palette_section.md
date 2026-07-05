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

Each palette carries the **full M3 token set** — accent roles, error roles, and all surface/neutral tokens.
The palette selector must swap all tokens listed below, not just accent roles.

---

### Preset Palettes

#### Professional Blue

- Source color: `#2563EB`
- Best for: consultants, clinics, therapists.
- Intended effect: trust, professionalism, stability.

```css
/* Professional Blue — Full M3 Token Set (Light Mode) */
--md-sys-color-primary:                   #2e519e;
--md-sys-color-on-primary:                #ffffff;
--md-sys-color-primary-container:         #d7e0f4;
--md-sys-color-on-primary-container:      #101623;
--md-sys-color-secondary:                 #5d636f;
--md-sys-color-on-secondary:              #ffffff;
--md-sys-color-secondary-container:       #e3e5e8;
--md-sys-color-on-secondary-container:    #18191b;
--md-sys-color-tertiary:                  #596173;
--md-sys-color-on-tertiary:               #ffffff;
--md-sys-color-tertiary-container:        #e2e4e9;
--md-sys-color-on-tertiary-container:     #17191c;
--md-sys-color-error:                     #9e5d2e;
--md-sys-color-on-error:                  #ffffff;
--md-sys-color-error-container:           #f4e3d7;
--md-sys-color-on-error-container:        #231810;
/* Surface & Neutral — 12 surface tokens */
--md-sys-color-background:                #fcfcfd;
--md-sys-color-on-background:             #18191b;
--md-sys-color-surface:                   #fcfcfd;
--md-sys-color-on-surface:                #18191b;
--md-sys-color-surface-variant:           #e5e5e6;
--md-sys-color-on-surface-variant:        #494b50;
--md-sys-color-surface-container-lowest:  #ffffff;
--md-sys-color-surface-container-low:     #f4f5f5;
--md-sys-color-surface-container:         #eff0f0;
--md-sys-color-surface-container-high:    #eaeaeb;
--md-sys-color-surface-container-highest: #e5e5e6;
--md-sys-color-outline:                   #797d86;
--md-sys-color-outline-variant:           #cacbce;
--md-sys-color-inverse-surface:           #313235;
--md-sys-color-inverse-on-surface:        #f2f2f3;
--md-sys-color-inverse-primary:           #b0c2e8;
```

```css
/* Professional Blue — Dark Mode */
--md-sys-color-primary:                   #b0c2e8;
--md-sys-color-on-primary:                #1c2a4a;
--md-sys-color-primary-container:         #223d77;
--md-sys-color-on-primary-container:      #d7e0f4;
--md-sys-color-secondary:                 #c8cad0;
--md-sys-color-on-secondary:              #2f3237;
--md-sys-color-secondary-container:       #464a53;
--md-sys-color-on-secondary-container:    #e3e5e8;
--md-sys-color-tertiary:                  #c5c9d3;
--md-sys-color-on-tertiary:               #2d3139;
--md-sys-color-tertiary-container:        #424957;
--md-sys-color-on-tertiary-container:     #e2e4e9;
--md-sys-color-error:                     #e8c7b0;
--md-sys-color-on-error:                  #4a2f1c;
--md-sys-color-error-container:           #774522;
--md-sys-color-on-error-container:        #f4e3d7;
/* Surface & Neutral — Dark Mode */
--md-sys-color-background:                #0c0d0d;
--md-sys-color-on-background:             #e5e5e6;
--md-sys-color-surface:                   #0c0d0d;
--md-sys-color-on-surface:                #e5e5e6;
--md-sys-color-surface-variant:           #494b50;
--md-sys-color-on-surface-variant:        #cacbce;
--md-sys-color-surface-container-lowest:  #070808;
--md-sys-color-surface-container-low:     #18191b;
--md-sys-color-surface-container:         #1d1e20;
--md-sys-color-surface-container-high:    #2a2b2d;
--md-sys-color-surface-container-highest: #36373a;
--md-sys-color-outline:                   #94979e;
--md-sys-color-outline-variant:           #494b50;
--md-sys-color-inverse-surface:           #e5e5e6;
--md-sys-color-inverse-on-surface:        #313235;
--md-sys-color-inverse-primary:           #2e519e;
```

---

#### Warm Coral

- Source color: `#FF6F61`
- Best for: salons, spas, lifestyle, beauty.
- Intended effect: warmth, creativity, approachability.

```css
/* Warm Coral — Full M3 Token Set (Light Mode) */
--md-sys-color-primary:                   #9e382e;
--md-sys-color-on-primary:                #ffffff;
--md-sys-color-primary-container:         #f4dad7;
--md-sys-color-on-primary-container:      #231210;
--md-sys-color-secondary:                 #6f5f5d;
--md-sys-color-on-secondary:              #ffffff;
--md-sys-color-secondary-container:       #e8e4e3;
--md-sys-color-on-secondary-container:    #1b1818;
--md-sys-color-tertiary:                  #735b59;
--md-sys-color-on-tertiary:               #ffffff;
--md-sys-color-tertiary-container:        #e9e3e2;
--md-sys-color-on-tertiary-container:     #1c1817;
--md-sys-color-error:                     #9e5d2e;
--md-sys-color-on-error:                  #ffffff;
--md-sys-color-error-container:           #f4e3d7;
--md-sys-color-on-error-container:        #231810;
/* Surface & Neutral — 12 surface tokens */
--md-sys-color-background:                #fdfcfc;
--md-sys-color-on-background:             #1b1918;
--md-sys-color-surface:                   #fdfcfc;
--md-sys-color-on-surface:                #1b1918;
--md-sys-color-surface-variant:           #e6e5e5;
--md-sys-color-on-surface-variant:        #504a49;
--md-sys-color-surface-container-lowest:  #ffffff;
--md-sys-color-surface-container-low:     #f5f5f4;
--md-sys-color-surface-container:         #f0efef;
--md-sys-color-surface-container-high:    #ebeaea;
--md-sys-color-surface-container-highest: #e6e5e5;
--md-sys-color-outline:                   #867a79;
--md-sys-color-outline-variant:           #cecbca;
--md-sys-color-inverse-surface:           #353131;
--md-sys-color-inverse-on-surface:        #f3f2f2;
--md-sys-color-inverse-primary:           #e8b5b0;
```

```css
/* Warm Coral — Dark Mode */
--md-sys-color-primary:                   #e8b5b0;
--md-sys-color-on-primary:                #4a201c;
--md-sys-color-primary-container:         #772a22;
--md-sys-color-on-primary-container:      #f4dad7;
--md-sys-color-secondary:                 #d0c8c8;
--md-sys-color-on-secondary:              #37302f;
--md-sys-color-secondary-container:       #534746;
--md-sys-color-on-secondary-container:    #e8e4e3;
--md-sys-color-tertiary:                  #d3c6c5;
--md-sys-color-on-tertiary:               #392e2d;
--md-sys-color-tertiary-container:        #574442;
--md-sys-color-on-tertiary-container:     #e9e3e2;
--md-sys-color-error:                     #e8c7b0;
--md-sys-color-on-error:                  #4a2f1c;
--md-sys-color-error-container:           #774522;
--md-sys-color-on-error-container:        #f4e3d7;
/* Surface & Neutral — Dark Mode */
--md-sys-color-background:                #0d0c0c;
--md-sys-color-on-background:             #e6e5e5;
--md-sys-color-surface:                   #0d0c0c;
--md-sys-color-on-surface:                #e6e5e5;
--md-sys-color-surface-variant:           #504a49;
--md-sys-color-on-surface-variant:        #cecbca;
--md-sys-color-surface-container-lowest:  #080707;
--md-sys-color-surface-container-low:     #1b1918;
--md-sys-color-surface-container:         #201e1d;
--md-sys-color-surface-container-high:    #2d2a2a;
--md-sys-color-surface-container-highest: #3a3636;
--md-sys-color-outline:                   #9e9594;
--md-sys-color-outline-variant:           #504a49;
--md-sys-color-inverse-surface:           #e6e5e5;
--md-sys-color-inverse-on-surface:        #353131;
--md-sys-color-inverse-primary:           #9e382e;
```

---

#### Calm Teal

- Source color: `#008080`
- Best for: fitness, wellness, physiotherapy, health services.
- Intended effect: health, balance, growth.

```css
/* Calm Teal — Full M3 Token Set (Light Mode) */
--md-sys-color-primary:                   #2e9e9e;
--md-sys-color-on-primary:                #ffffff;
--md-sys-color-primary-container:         #d7f4f4;
--md-sys-color-on-primary-container:      #102323;
--md-sys-color-secondary:                 #5d6f6f;
--md-sys-color-on-secondary:              #ffffff;
--md-sys-color-secondary-container:       #e3e8e8;
--md-sys-color-on-secondary-container:    #181b1b;
--md-sys-color-tertiary:                  #597373;
--md-sys-color-on-tertiary:               #ffffff;
--md-sys-color-tertiary-container:        #e2e9e9;
--md-sys-color-on-tertiary-container:     #171c1c;
--md-sys-color-error:                     #9e5d2e;
--md-sys-color-on-error:                  #ffffff;
--md-sys-color-error-container:           #f4e3d7;
--md-sys-color-on-error-container:        #231810;
/* Surface & Neutral — 12 surface tokens */
--md-sys-color-background:                #fcfdfd;
--md-sys-color-on-background:             #181b1b;
--md-sys-color-surface:                   #fcfdfd;
--md-sys-color-on-surface:                #181b1b;
--md-sys-color-surface-variant:           #e5e6e6;
--md-sys-color-on-surface-variant:        #495050;
--md-sys-color-surface-container-lowest:  #ffffff;
--md-sys-color-surface-container-low:     #f4f5f5;
--md-sys-color-surface-container:         #eff0f0;
--md-sys-color-surface-container-high:    #eaebeb;
--md-sys-color-surface-container-highest: #e5e6e6;
--md-sys-color-outline:                   #798686;
--md-sys-color-outline-variant:           #cacece;
--md-sys-color-inverse-surface:           #313535;
--md-sys-color-inverse-on-surface:        #f2f3f3;
--md-sys-color-inverse-primary:           #b0e8e8;
```

```css
/* Calm Teal — Dark Mode */
--md-sys-color-primary:                   #b0e8e8;
--md-sys-color-on-primary:                #1c4a4a;
--md-sys-color-primary-container:         #227777;
--md-sys-color-on-primary-container:      #d7f4f4;
--md-sys-color-secondary:                 #c8d0d0;
--md-sys-color-on-secondary:              #2f3737;
--md-sys-color-secondary-container:       #465353;
--md-sys-color-on-secondary-container:    #e3e8e8;
--md-sys-color-tertiary:                  #c5d3d3;
--md-sys-color-on-tertiary:               #2d3939;
--md-sys-color-tertiary-container:        #425757;
--md-sys-color-on-tertiary-container:     #e2e9e9;
--md-sys-color-error:                     #e8c7b0;
--md-sys-color-on-error:                  #4a2f1c;
--md-sys-color-error-container:           #774522;
--md-sys-color-on-error-container:        #f4e3d7;
/* Surface & Neutral — Dark Mode */
--md-sys-color-background:                #0c0d0d;
--md-sys-color-on-background:             #e5e6e6;
--md-sys-color-surface:                   #0c0d0d;
--md-sys-color-on-surface:                #e5e6e6;
--md-sys-color-surface-variant:           #495050;
--md-sys-color-on-surface-variant:        #cacece;
--md-sys-color-surface-container-lowest:  #070808;
--md-sys-color-surface-container-low:     #181b1b;
--md-sys-color-surface-container:         #1d2020;
--md-sys-color-surface-container-high:    #2a2d2d;
--md-sys-color-surface-container-highest: #363a3a;
--md-sys-color-outline:                   #949e9e;
--md-sys-color-outline-variant:           #495050;
--md-sys-color-inverse-surface:           #e5e6e6;
--md-sys-color-inverse-on-surface:        #313535;
--md-sys-color-inverse-primary:           #2e9e9e;
```

---

#### Sophisticated Purple

- Source color: `#6B46C1`
- Best for: executive coaching, premium consulting, high-end services.
- Intended effect: premium, refined, exclusive.

```css
/* Sophisticated Purple — Full M3 Token Set (Light Mode) */
--md-sys-color-primary:                   #523399;
--md-sys-color-on-primary:                #ffffff;
--md-sys-color-primary-container:         #e0d9f2;
--md-sys-color-on-primary-container:      #161122;
--md-sys-color-secondary:                 #635e6e;
--md-sys-color-on-secondary:              #ffffff;
--md-sys-color-secondary-container:       #e5e3e8;
--md-sys-color-on-secondary-container:    #19181b;
--md-sys-color-tertiary:                  #615a72;
--md-sys-color-on-tertiary:               #ffffff;
--md-sys-color-tertiary-container:        #e4e2e9;
--md-sys-color-on-tertiary-container:     #19171c;
--md-sys-color-error:                     #9e5d2e;
--md-sys-color-on-error:                  #ffffff;
--md-sys-color-error-container:           #f4e3d7;
--md-sys-color-on-error-container:        #231810;
/* Surface & Neutral — 12 surface tokens */
--md-sys-color-background:                #fcfcfd;
--md-sys-color-on-background:             #19181b;
--md-sys-color-surface:                   #fcfcfd;
--md-sys-color-on-surface:                #19181b;
--md-sys-color-surface-variant:           #e5e5e6;
--md-sys-color-on-surface-variant:        #4b4950;
--md-sys-color-surface-container-lowest:  #ffffff;
--md-sys-color-surface-container-low:     #f5f4f5;
--md-sys-color-surface-container:         #f0eff0;
--md-sys-color-surface-container-high:    #eaeaeb;
--md-sys-color-surface-container-highest: #e5e5e6;
--md-sys-color-outline:                   #7d7986;
--md-sys-color-outline-variant:           #cbcace;
--md-sys-color-inverse-surface:           #323135;
--md-sys-color-inverse-on-surface:        #f2f2f3;
--md-sys-color-inverse-primary:           #c2b3e5;
```

```css
/* Sophisticated Purple — Dark Mode */
--md-sys-color-primary:                   #c2b3e5;
--md-sys-color-on-primary:                #2b1e48;
--md-sys-color-primary-container:         #3d2673;
--md-sys-color-on-primary-container:      #e0d9f2;
--md-sys-color-secondary:                 #cac8d0;
--md-sys-color-on-secondary:              #323036;
--md-sys-color-secondary-container:       #4a4653;
--md-sys-color-on-secondary-container:    #e5e3e8;
--md-sys-color-tertiary:                  #cac6d2;
--md-sys-color-on-tertiary:               #312e38;
--md-sys-color-tertiary-container:        #494356;
--md-sys-color-on-tertiary-container:     #e4e2e9;
--md-sys-color-error:                     #e8c7b0;
--md-sys-color-on-error:                  #4a2f1c;
--md-sys-color-error-container:           #774522;
--md-sys-color-on-error-container:        #f4e3d7;
/* Surface & Neutral — Dark Mode */
--md-sys-color-background:                #0d0c0d;
--md-sys-color-on-background:             #e5e5e6;
--md-sys-color-surface:                   #0d0c0d;
--md-sys-color-on-surface:                #e5e5e6;
--md-sys-color-surface-variant:           #4b4950;
--md-sys-color-on-surface-variant:        #cbcace;
--md-sys-color-surface-container-lowest:  #080708;
--md-sys-color-surface-container-low:     #19181b;
--md-sys-color-surface-container:         #1e1d20;
--md-sys-color-surface-container-high:    #2b2a2d;
--md-sys-color-surface-container-highest: #37363a;
--md-sys-color-outline:                   #97949e;
--md-sys-color-outline-variant:           #4b4950;
--md-sys-color-inverse-surface:           #e5e5e6;
--md-sys-color-inverse-on-surface:        #323135;
--md-sys-color-inverse-primary:           #523399;
```

---

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
The palette selector must swap **all tokens in the full token set above** — not only the accent roles.
Surface and neutral tokens are palette-specific and must be included in every palette swap.

### Dark Mode

Per ADR-46, dark mode is incorporated into V1. Each palette includes dark mode token overrides.
Screens must include `@media (prefers-color-scheme: dark)` blocks with the dark mode token values above.
