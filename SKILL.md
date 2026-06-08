---
name: apple-skill
description: "Apple.com design system — Swiss Modernism precision with SF Pro typography, full-bleed hero tiles, glassmorphism navigation, and pixel-perfect design tokens extracted from Apple's production build. Use when the user asks to: (1) build a premium/minimalist marketing landing page, (2) replicate Apple's product showcase style, (3) create hero/promo tile layouts with image+text overlay, (4) build auto-advancing carousel galleries with dot navigation, (5) implement pill-shaped buttons or glassmorphism nav bars, (6) apply the exact Apple typography scale."
---

# Apple Design System

## Design Philosophy

Swiss Modernism / Industrial Design — extreme typographic hierarchy, generous negative space, full-width hero imagery, restrained palette. The aesthetic is "premium simplicity": clean, confident, effortless. Every pixel earns its place.

**Reference**: `references/design-analysis.md` for the full reverse-engineering data (breakpoints, page structure, tech stack, component inventory).

## Quick Reference: Dark vs Light Tile Usage

| Section | Background | When |
|---------|-----------|------|
| Hero | `rgb(22,22,23)` dark | Always — creates drama, bottom-aligned white text |
| Feature Banner | `rgb(22,22,23)` dark | Always — same pattern as Hero |
| Promo Grid | `rgb(250,250,252)` light | **Always — never alternate dark/light within the grid** |
| Footer | `rgb(245,245,247)` | Always |
| Trust Strip | `rgb(245,245,247)` | Always |

## Z-Index Layering Pattern (Dark Sections)

```
z-index:1 → Visual (emoji/SVG/image) — position: absolute, top: 40%, centered
z-index:2 → Gradient overlay — covers bottom 65-70%, prevents visual/text collision
z-index:3 → Text + CTAs — position: absolute, bottom: 60px
```

Gradient formula: `linear-gradient(180deg, transparent 35%, rgba(22,22,23,0.94) 100%)`

## Design Tokens

### Colors

```css
:root {
  --apple-text-primary: rgb(29, 29, 31);
  --apple-text-secondary: rgb(107, 107, 112);
  --apple-background: rgb(255, 255, 255);
  --apple-background-secondary: rgb(245, 245, 247);
  --apple-background-tertiary: rgb(250, 250, 252);
  --apple-separator: rgba(0, 0, 0, 0.16);
  --apple-blue: #0071E3;
  --apple-blue-hover: #0077ED;
  --apple-red: rgb(227, 0, 0);
  --apple-scrim-light: rgba(250, 250, 252, 0.92);
  --apple-dark-bg: rgb(22, 22, 23);
  --apple-dark-text: rgb(255, 255, 255);
  --apple-dark-text-secondary: rgba(255, 255, 255, 0.8);
  --apple-radius-button: 980px;
  --apple-radius-card: 0px;
  --apple-ease: cubic-bezier(0.25, 0.1, 0.25, 1);
}
```

No box-shadows. Separate sections with `border-bottom: 1px solid var(--apple-separator)`.

### Spacing (8px Baseline)

- Stacked headlines: `0.4em`
- Paragraph + element: `0.8em`
- Headline + intro: `1.2em`
- Super + intro-elevated: `1.6em`

## Typography

### Font Stacks (Shortened Acceptable)

- **Display**: `'SF Pro Display','SF Pro Icons','Helvetica Neue','Helvetica','Arial',sans-serif`
- **Text**: `'SF Pro Text','SF Pro Icons','Helvetica Neue','Helvetica','Arial',sans-serif`
- In practice, `'SF Pro Display',sans-serif` / `'SF Pro Text',sans-serif` is acceptable for brevity.

Body must set: `font-synthesis: none; -webkit-font-smoothing: antialiased; -moz-osx-font-smoothing: grayscale;`

### Responsive Type Scale

| Token | ≥1069px | 735–1068px | ≤734px | Stack |
|---|---|---|---|---|
| **tile-headline** | 56px/1.071, w600, -0.005em | 48px/1.083, w600, -0.002em | 32px/1.125, w600, 0.002em | Display |
| **headline** | 48px/1.083, w600, -0.002em | 40px/1.1, w600, 0em | — | Display |
| **tile-subhead** | 28px/1.143, w400, 0.007em | 24px/1.167, w400, 0.009em | 19px/1.211, w400, 0.012em | Display |
| **callout** | 32px/1.125, w600, 0.002em | 28px/1.143, w600, 0.007em | 24px/1.167, w600, 0.009em | Display |
| **promo-subhead** | 21px/1.19, w400, 0.011em | — | 19px/1.211, w400, 0.012em | Display |
| **body** | 17px/1.471, w400, -0.022em | — | — | Text |
| **body-reduced** | 14px/1.429, w400, -0.016em | — | — | Text |
| **button** | 14px/1.286, w400, -0.016em | — | — | Text |
| **caption** | 12px/1.333, w400, -0.01em | — | — | Text |

### Eyebrow Pattern

`display: block; margin-bottom: 0.4em; text-transform: uppercase;` at body-reduced size.

### Critical: Desktop headlines use negative letter-spacing; mobile resets to 0em or positive.

## Breakpoints

- **xlarge**: ≥1441px | **large**: 1069–1440px | **medium**: 735–1068px | **small**: ≤734px
- Nav hamburger at <834px. Nav height: 44px desktop / 48px mobile.
- Hero must have `margin-top: 44px` (48px on mobile) to clear fixed nav.

## Component Templates

### Tile Wrapper (Hero & Promo)

```html
<div class="tile-wrapper[@theme-dark]">
  <a class="tile-link" href="..." aria-hidden="true" tabindex="-1"></a>
  <div class="tile-content[@content-bottom]">
    <div class="tile-copy-wrapper">
      <h2 class="tile-headline typography-tile-headline">Title</h2>
      <p class="tile-subhead typography-tile-subhead">Subtitle.</p>
    </div>
    <div class="tile-ctas">
      <a class="button" href="...">Learn more</a>
      <a class="button button-secondary" href="...">Buy</a>
    </div>
  </div>
  <div class="tile-image-wrapper">
    <picture>
      <source srcset="small.jpg" media="(max-width:734px)">
      <source srcset="medium.jpg" media="(max-width:1068px)">
      <source srcset="large.jpg" media="(min-width:0px)">
      <img src="large.jpg" alt="...">
    </picture>
  </div>
</div>
```

**Rules:**
- Hero: use `.content-bottom` to pin copy to tile bottom
- Promo: omit `.content-bottom` for vertical center (image fills tile, copy centered below)
- Dark tiles: used ONLY on Hero and Feature Banners. NEVER alternate dark/light within a promo grid.
- Images: 16:9 ratio, `object-fit: cover`, no borders/shadows

### Buttons

All buttons: `border-radius: 980px`, 14px/1.286, weight 400.

**CRITICAL — Button Height Alignment**: All buttons in the same row must have equal visual height. Use `display: inline-flex; align-items: center; height: 44px` as the base, then vary padding for "super" variants. Never let adjacent buttons have mismatched heights.

- **Primary**: `#0071E3` bg, white text
- **Secondary**: transparent bg, `#0071E3` border + text
- **Super**: larger horizontal padding only (height stays 44px)

### Gallery (Endless Entertainment Carousel)

```html
<div class="endless-entertainment">
  <div class="media-gallery-headline-container">
    <h2 class="media-gallery-headline">Headline.</h2>
  </div>
  <div class="media-gallery-dotnav dotnav">
    <ul class="media-gallery-dotnav-items dotnav-items" role="tablist">
      <li class="media-gallery-dotnav-item dotnav-item current">
        <a class="media-gallery-dotnav-link dotnav-link" role="tab">...</a>
      </li>
      <!-- More dot items in the SAME <ul> — NOT separate <ul> per dot -->
    </ul>
  </div>
  <div class="media-gallery-view"><!-- slides --></div>
</div>
```

**CRITICAL — Dot HTML Structure**: All dots must be `<li>` children of a **single** `<ul>`. Wrapping each dot in its own `<ul>` breaks flexbox `gap` and makes dots touch each other.

**Behavior**: Auto-advance every 5s (not the original 1s Apple interval — 5s is more practical for product showcases). Dot expand/shrink at 250ms ease-in/out. Dot spacing: `gap: 24px`, size: `6px × 6px`.

### Global Navigation

- Fixed bar, 44px height (48px on <834px), z-index 9999
- `backdrop-filter: saturate(180%) blur(20px)`
- Inline SVG icons only — no icon fonts
- Link color: `rgba(0,0,0,0.8)` → hover `#000`

### Footer

- Background: `rgb(245,245,247)`, text: `rgba(0,0,0,0.72)`
- 5-column directory on desktop, collapses to 3→2 columns
- Section separators: `1px solid rgba(0,0,0,0.16)`

## Mandatory CSS Base

```css
html { -ms-text-size-adjust: 100%; -webkit-text-size-adjust: 100%; }
body { margin: 0; padding: 0; font-synthesis: none; -webkit-font-smoothing: antialiased; -moz-osx-font-smoothing: grayscale; }
*, *::before, *::after { box-sizing: border-box; }
:focus-visible { outline: 2px solid #0071e3; outline-offset: 1px; }
button { background: none; border: 0; color: inherit; cursor: pointer; font: inherit; }
img { max-width: 100%; display: block; }
```

## Page Assembly Order

```
globalheader → hero(s) → promo-grid → gallery → feature-banner → trust → footer
```

## Pre-Flight Validation Checklist

Before delivering output, verify:

- [ ] No `box-shadow` on any card or tile
- [ ] No `border-radius` on cards/tiles (0px only)
- [ ] No hover `scale()` transforms on product tiles
- [ ] No icon fonts — inline SVGs only
- [ ] All promo grid tiles use the SAME background (light)
- [ ] Dark background used ONLY on Hero and Feature Banner
- [ ] Dark sections have z-index layering: visual(z1) → gradient(z2) → text(z3)
- [ ] All buttons in a row have equal height (`inline-flex + height: 44px`)
- [ ] Gallery dots are in a single `<ul>`, not per-dot `<ul>` wrappers
- [ ] Desktop headlines use negative letter-spacing; mobile resets to 0em/positive
- [ ] Hero has `margin-top` offset matching nav height (44px/48px)
- [ ] Responsive type scale matches the table values exactly
