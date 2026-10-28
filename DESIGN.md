# Interactive card — design spec

Source of truth: `website-portfolio` (`src/app/globals.css`, `src/app/page.module.css`,
`src/app/layout.tsx`, `public/card-*.svg`, `src/components/SplitFlapText.tsx`).
The reference card supplies composition and the blue; everything else comes from the site.

## Color

| Role | Value | From |
|---|---|---|
| Background | `#060607` | `globals.css --background` |
| Surface (dialog) | `#0b0b0e` | `card-front.svg` gradient stop |
| Text | `#f5f5f7` | `globals.css --foreground` |
| Muted | `#f5f5f7` @ 62% / 50% / 38% | `page.module.css --ink-quiet`, `--ink-faint`, card SVG footer |
| Hairline | `#fff` @ 10% (32% on hover/active) | `card-*.svg` frame |
| Glow | `rgb(120 130 160)` @ 16% | `page.module.css .page::before` |
| Accent | `#2b47ff` (signal) | Reference card; hue 228° matches the site's glow |
| Gradient | `#2b47ff → #1124b8 → #0a1150 → #060607`, radial from top-right | Front only |

The repo has no brand colour. Blue is confined to the card front, so the page itself stays the site's black.

## Typography

- **Font:** Inter 400/500/600 (sans) and Geist Mono 500 (labels), as in `layout.tsx`
- **Name:** 10.5pt, 600, −1.5% tracking
- **Role:** 5.5pt, 500, uppercase, +16% tracking (the site's `.meta`)
- **Body / tagline:** 6pt, 400, 1.4 line height
- **Contact values:** 6pt, 400 (same as the tagline)
- **Labels:** Geist Mono 5pt, 500, uppercase, +16% (role line). Phone screens get an 8px minimum.
- **Contact icons:** 6.5pt monochrome glyphs in muted 38% (62% while the row's link is hovered), with a 10px minimum on phones. The words stay as screen-reader text.
- **UI chrome:** 0.6875rem uppercase meta (`page.module.css .meta`, `.hint`)

## Spacing

- **Base unit:** 0.125in on the card (bleed) and 0.5rem in the UI
- **Card:** trim 3.5 × 2in, bleed 0.125in, safe margin 0.25in. Content sits on the four corners only.
- **Controls:** 0.5rem gaps, 44px minimum touch height
- **Page padding:** `clamp(1rem, 4.5vw, 3.5rem)`, as on the site's `.bar`

## Motion

| What | Duration | Easing |
|---|---|---|
| Entrance (`rise`: 10px, blur 8px → 0) | 800–900ms, staggered | `--enter` `cubic-bezier(.16,1,.3,1)` (site) |
| Flip | 560ms, card presses to 0.985 first (160ms) | `--turn` `cubic-bezier(.6,0,.2,1)` |
| Tilt | ±3° toward the pointer, mouse only; artwork stays still | Spring (stiffness 140, damping 22), time-based |
| Gradient drift | 22s alternate, 4% travel | ease-in-out |
| Hover / press | 200ms color; 200ms underline (scaleX); 160ms press to 0.97 | ease / `--ease-out` `cubic-bezier(.23,1,.32,1)` |

`prefers-reduced-motion` keeps fades and removes movement: no tilt or drift, entrances fade, and the flip becomes a 200ms crossfade.

## Components

The portfolio has no shared button, link, dialog or tooltip components yet: `page.tsx` is a single
page, and `Lanyard` and `SplitFlapText` are bespoke. So this file reuses the site's **patterns**
rather than imports:

- **Buttons:** the `.meta` type plus a hairline border and 2px radius (the site's focus radius)
- **Links:** the `.link` colour transition (quiet → full ink)
- **Focus:** a 1px `--foreground` outline offset 4px, from `.link:focus-visible`
- **Dialog:** a native `<dialog>`, so there's no new primitive
- **Icons:** used only to key the contact rows. Phone and globe come from Material Icons (Apache-2.0); Gmail, LinkedIn and Facebook come from Simple Icons (CC0). All are inlined as `<symbol>`s with `currentColor` fills.
