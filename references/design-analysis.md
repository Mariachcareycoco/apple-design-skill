# Apple.com Design Analysis — Reverse-Engineering Supplement

When deeper context is needed on a specific subsystem, grep this file for the section name.

## Layout Architecture & Grid

- Container: full-bleed sections, content capped at ~980–1068px
- Page structure: nav(44px) → hero tiles(stacked) → promo grid(2-col) → gallery(full-width) → footer(5-col)
- Promo grid: Flexbox 2-column, collapses at 734px. Tiles exactly 50% on desktop, zero gap between them
- Nav: centered flexbox row, max-width ~980px

## Color Reference (Cross-Validated from main.built.css)

| Role | Value |
|------|-------|
| Primary text | rgb(29, 29, 31) |
| Body bg | rgb(255, 255, 255) |
| Secondary bg (footer) | rgb(245, 245, 247) |
| Tertiary bg (nav scrim) | rgb(250, 250, 252) |
| Link blue | rgb(0, 102, 204) |
| Link hover | rgb(0, 113, 227) |
| Button primary | #0071E3 |
| Separator light | rgba(0, 0, 0, 0.16) |
| Separator dark | rgba(255, 255, 255, 0.24) |
| Secondary text | rgb(107, 107, 112) |
| Footer text | rgba(0, 0, 0, 0.56) |

## Typography Calibration Notes

- Body base: 17px / line-height 1.4705882353 / letter-spacing -0.022em
- All values calibrated from main.built.css, visually confirmed against ctrls render
- `--sk-*` tokens: `--sk-default-stacked-margin: .4em`, `--sk-paragraph-plus-element-margin: .8em`, `--sk-headline-plus-first-element-margin: .8em`, `--sk-headline-plus-headline-margin: .4em`, `--sk-paragraph-plus-headline-margin: 1.6em`

## Motion & Interaction

| Element | Duration | Easing | Trigger |
|---------|----------|--------|---------|
| Nav flyout reveal | 500ms | opacity | hover |
| Nav flyout close delay | 120ms | — | mouseleave |
| Gallery auto-advance | 5000ms (practical) | — | setInterval |
| Dot expand/shrink | 250ms | ease-in/out | click/auto |
| Footer accordion | 300ms | ease | click (mobile) |
| Scroll-triggered video | — | — | IntersectionObserver, t-75vh |

## Component Deep-Dive

### Navigation
- Fixed bar, 44px (48px <834px), z-index 9999
- Inline SVG icons: regular 44px / compact 48px variants
- Scrim backdrop: `saturate(180%) blur(20px)`
- Flyout submenus with opacity transition

### Hero Section
- Full-width tiles, bottom-aligned text overlay
- Supports `<video>` with scroll-triggered play/pause, static `<picture>` fallback
- Dark imagery only — light text overlay

### Promo Grid
- 2-column center-aligned tiles
- Images fill tile area with `object-fit: cover`
- No hover scale effects

### Gallery (Endless Entertainment)
- Auto-advancing dot-nav
- Timed transitions between slides
- Dotnav uses `<ul>` with `<li>` children, `role="tablist"`

### Buttons
- Primary: `#0071E3` bg, white text
- Secondary: transparent, blue border
- Super: larger padding
- All: `border-radius: 980px`, 14px font

### Footer
- 5-column directory on desktop
- Accordion collapse on mobile (<834px)
- Section dividers: `1px solid rgba(0,0,0,0.16)`

## Imagery Specs

- Hero images: 16:9 ratio, ~1250×668px (large variant)
- 5 responsive variants per image: small, medium, mediumtall, large, largetall
- All variants have 2× retina counterparts
- `object-fit: cover`, no borders or shadows

## Tech Stack (Apple's Actual)

- No frontend framework — vanilla server-rendered HTML + PostCSS + ES6
- CSS: single-file build output (`main.built.css`), design tokens via `--sk-*` custom properties
- JS: per-component built files (`main.built.js`, `inline-media.built.js`, `home-gallery.built.js`)
- Fonts: proprietary SF Pro from Apple CDN
- Icons: inline SVGs, not icon fonts or React components
