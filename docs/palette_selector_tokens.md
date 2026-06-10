# Palette Selector — Token Swap Blocks

These are the CSS custom property blocks your palette selector
must apply to `:root` (or the top-level app element) when
a palette is selected. Each block contains the complete token set.

---

## Usage Pattern

```python
# In your palette selector Python component
PALETTES = {
    'professional_blue': {...},
    'warm_coral':        {...},
    'calm_teal':         {...},
    'sophisticated_purple': {...},
}

def apply_palette(palette_key):
    tokens = PALETTES[palette_key]
    js_assignments = '; '.join(
        f"document.documentElement.style.setProperty('{k}', '{v}')"
        for k, v in tokens.items()
    )
    anvil.js.call_js('eval', js_assignments)
    # persist choice to business_profile.palette
```

---

## Token Blocks

### professional_blue

```python
'professional_blue': {
    '--md-sys-color-primary':                   '#493ef3',
    '--md-sys-color-on-primary':                '#ffffff',
    '--md-sys-color-primary-container':         '#e6dff3',
    '--md-sys-color-on-primary-container':      '#0f0e59',
    '--md-sys-color-secondary':                 '#625a74',
    '--md-sys-color-on-secondary':              '#ffffff',
    '--md-sys-color-secondary-container':       '#e8defc',
    '--md-sys-color-on-secondary-container':    '#1e182d',
    '--md-sys-color-tertiary':                  '#844f5d',
    '--md-sys-color-on-tertiary':               '#ffffff',
    '--md-sys-color-tertiary-container':        '#ffd9e1',
    '--md-sys-color-on-tertiary-container':     '#380a1a',
    '--md-sys-color-error':                     '#b42336',
    '--md-sys-color-on-error':                  '#ffffff',
    '--md-sys-color-error-container':           '#f4dddc',
    '--md-sys-color-on-error-container':        '#410004',
    '--md-sys-color-background':                '#fdfcff',
    '--md-sys-color-on-background':             '#1c1b20',
    '--md-sys-color-surface':                   '#fdfcff',
    '--md-sys-color-on-surface':                '#1c1b20',
    '--md-sys-color-surface-variant':           '#e5e0f0',
    '--md-sys-color-on-surface-variant':        '#494552',
    '--md-sys-color-surface-container-lowest':  '#ffffff',
    '--md-sys-color-surface-container-low':     '#f5f2fa',
    '--md-sys-color-surface-container':         '#efedf5',
    '--md-sys-color-surface-container-high':    '#eae7ef',
    '--md-sys-color-surface-container-highest': '#e4e1e9',
    '--md-sys-color-outline':                   '#7a7583',
    '--md-sys-color-outline-variant':           '#c9c4d3',
    '--md-sys-color-inverse-surface':           '#312f35',
    '--md-sys-color-inverse-on-surface':        '#f2eff7',
    '--md-sys-color-inverse-primary':           '#cfbdf8',
},
```

### warm_coral

```python
'warm_coral': {
    '--md-sys-color-primary':                   '#ae2f2f',
    '--md-sys-color-on-primary':                '#ffffff',
    '--md-sys-color-primary-container':         '#fbdbd7',
    '--md-sys-color-on-primary-container':      '#3d0600',
    '--md-sys-color-secondary':                 '#785651',
    '--md-sys-color-on-secondary':              '#ffffff',
    '--md-sys-color-secondary-container':       '#fbdbd7',
    '--md-sys-color-on-secondary-container':    '#2f1310',
    '--md-sys-color-tertiary':                  '#6a5d37',
    '--md-sys-color-on-tertiary':               '#ffffff',
    '--md-sys-color-tertiary-container':        '#f3e1b5',
    '--md-sys-color-on-tertiary-container':     '#221b01',
    '--md-sys-color-error':                     '#b42336',
    '--md-sys-color-on-error':                  '#ffffff',
    '--md-sys-color-error-container':           '#f4dddc',
    '--md-sys-color-on-error-container':        '#410004',
    '--md-sys-color-background':                '#fffbfb',
    '--md-sys-color-on-background':             '#1f1a1a',
    '--md-sys-color-surface':                   '#fffbfb',
    '--md-sys-color-on-surface':                '#1f1a1a',
    '--md-sys-color-surface-variant':           '#ede0de',
    '--md-sys-color-on-surface-variant':        '#4f4443',
    '--md-sys-color-surface-container-lowest':  '#ffffff',
    '--md-sys-color-surface-container-low':     '#f9f2f1',
    '--md-sys-color-surface-container':         '#f3eceb',
    '--md-sys-color-surface-container-high':    '#ede7e6',
    '--md-sys-color-surface-container-highest': '#e8e1e0',
    '--md-sys-color-outline':                   '#807473',
    '--md-sys-color-outline-variant':           '#d0c4c2',
    '--md-sys-color-inverse-surface':           '#342f2f',
    '--md-sys-color-inverse-on-surface':        '#f6efee',
    '--md-sys-color-inverse-primary':           '#feb4ab',
},
```

### calm_teal

```python
'calm_teal': {
    '--md-sys-color-primary':                   '#206868',
    '--md-sys-color-on-primary':                '#ffffff',
    '--md-sys-color-primary-container':         '#91f3f2',
    '--md-sys-color-on-primary-container':      '#002020',
    '--md-sys-color-secondary':                 '#486363',
    '--md-sys-color-on-secondary':              '#ffffff',
    '--md-sys-color-secondary-container':       '#cae8e8',
    '--md-sys-color-on-secondary-container':    '#051f1f',
    '--md-sys-color-tertiary':                  '#486176',
    '--md-sys-color-on-tertiary':               '#ffffff',
    '--md-sys-color-tertiary-container':        '#cce6fe',
    '--md-sys-color-on-tertiary-container':     '#001e2f',
    '--md-sys-color-error':                     '#b42336',
    '--md-sys-color-on-error':                  '#ffffff',
    '--md-sys-color-error-container':           '#f4dddc',
    '--md-sys-color-on-error-container':        '#410004',
    '--md-sys-color-background':                '#f8fdfd',
    '--md-sys-color-on-background':             '#181c1c',
    '--md-sys-color-surface':                   '#f8fdfd',
    '--md-sys-color-on-surface':                '#181c1c',
    '--md-sys-color-surface-variant':           '#d9e5e4',
    '--md-sys-color-on-surface-variant':        '#3f4948',
    '--md-sys-color-surface-container-lowest':  '#ffffff',
    '--md-sys-color-surface-container-low':     '#eff5f4',
    '--md-sys-color-surface-container':         '#e9efef',
    '--md-sys-color-surface-container-high':    '#e4e9e9',
    '--md-sys-color-surface-container-highest': '#dee4e3',
    '--md-sys-color-outline':                   '#6f7979',
    '--md-sys-color-outline-variant':           '#bec9c8',
    '--md-sys-color-inverse-surface':           '#2d3131',
    '--md-sys-color-inverse-on-surface':        '#ecf2f2',
    '--md-sys-color-inverse-primary':           '#75d6d5',
},
```

### sophisticated_purple

```python
'sophisticated_purple': {
    '--md-sys-color-primary':                   '#971dbf',
    '--md-sys-color-on-primary':                '#ffffff',
    '--md-sys-color-primary-container':         '#f5dafb',
    '--md-sys-color-on-primary-container':      '#31063f',
    '--md-sys-color-secondary':                 '#6b586f',
    '--md-sys-color-on-secondary':              '#ffffff',
    '--md-sys-color-secondary-container':       '#f1dbf6',
    '--md-sys-color-on-secondary-container':    '#25162a',
    '--md-sys-color-tertiary':                  '#855052',
    '--md-sys-color-on-tertiary':               '#ffffff',
    '--md-sys-color-tertiary-container':        '#fbdbdb',
    '--md-sys-color-on-tertiary-container':     '#380b11',
    '--md-sys-color-error':                     '#b42336',
    '--md-sys-color-on-error':                  '#ffffff',
    '--md-sys-color-error-container':           '#f4dddc',
    '--md-sys-color-on-error-container':        '#410004',
    '--md-sys-color-background':                '#fefbff',
    '--md-sys-color-on-background':             '#1e1a1f',
    '--md-sys-color-surface':                   '#fefbff',
    '--md-sys-color-on-surface':                '#1e1a1f',
    '--md-sys-color-surface-variant':           '#eadfec',
    '--md-sys-color-on-surface-variant':        '#4d444f',
    '--md-sys-color-surface-container-lowest':  '#ffffff',
    '--md-sys-color-surface-container-low':     '#f7f2f9',
    '--md-sys-color-surface-container':         '#f2ecf3',
    '--md-sys-color-surface-container-high':    '#ece6ed',
    '--md-sys-color-surface-container-highest': '#e6e1e7',
    '--md-sys-color-outline':                   '#7e7480',
    '--md-sys-color-outline-variant':           '#cec3d0',
    '--md-sys-color-inverse-surface':           '#332f34',
    '--md-sys-color-inverse-on-surface':        '#f4eff6',
    '--md-sys-color-inverse-primary':           '#e9b4f7',
},
```
