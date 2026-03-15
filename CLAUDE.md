# Jake in Chicago — Portfolio Site

## Overview
One-page portfolio/resume site for a frontend developer. NYC subway / CTA transit signage design system over a live CTA train map background. Built with Astro, deployed on Vercel.

## Reference
`reference/portfolio-v3.html` is the single source of truth for all design decisions. When in doubt, match it exactly. This file contains every CSS rule, HTML structure, and JS behavior that defines the site.

## Stack
- **Astro** (minimal template) with vanilla JS — no React, Vue, or other frameworks
- **Vercel** for hosting with automatic GitHub deploys
- **Inter** from Google Fonts as primary typeface, with Helvetica/Arial fallback
- Font stack: `Inter, Helvetica, 'Helvetica Neue', Arial, sans-serif`

## Architecture
- `src/pages/index.astro` — the main single-page site (can be the prototype HTML directly)
- Plan for future project detail pages (e.g. `/projects/cta-train-map`) using Astro content collections
- Plan for image galleries on project pages using `astro:assets`
- Keep JS minimal and vanilla — no framework needed for interactivity
- Keep it simple. Don't over-engineer. One page is fine for now.

## CTA Map Background
- Live CTA train map embedded via iframe from `gojackrabbitgo.pythonanywhere.com`
- URL params: `?lat=41.8827&lng=-87.6317&zoom=8&embed=true`
- Iframe is 140% width with -20% margin-left for coverage
- JS dynamically positions the iframe so the Loop (map center) aligns over the center of the right content column, recalculates on resize
- Vignette radial gradient overlay fades map edges to black, center point tracked via JS to match iframe position
- Map starts at full opacity, fades to 0 as user scrolls through first viewport
- Parallax drift up at 0.2x scroll speed
- On mobile (≤900px), iframe resets to 100% centered, no offset

## Layout
- Two-column sticky sidebar layout inspired by brittanychiang.com
- Left sidebar: 48% width, sticky, contains name/title/tagline, nav, social links
- Right column: scrollable content with full-viewport empty spacer before sections (map is the splash)
- Max-width: 1200px container, 4rem horizontal padding (1.5rem on mobile)
- On mobile: columns stack, sidebar becomes relative, nav hidden

## Typography
- All text regular case — NO uppercase transforms anywhere
- Tight negative letter-spacing throughout: -0.03em on body, -0.04em to -0.05em on headings
- H1 (name): 64px, weight 600, letter-spacing -4px, word-spacing 2px
- Hierarchy via font-weight, not opacity/transparency: 700 → 600 → 400 → 300
- Text colors: #fff for primary, #999 for secondary, #666 for dim

## Colors
- Background: pure #000 black
- Text: white hierarchy via weight, not color opacity
- CTA line colors used ONLY for accents (badges, nav dots, route tags, timeline):
  - Red: #c60c30
  - Blue: #00a1de
  - Green: #009b3a
  - Orange: #f9461c
  - Purple: #522398
  - Brown: #62361b
  - Pink: #e27ea6
- Accent/hover: #fff primary, #ccc hover
- No blue tints anywhere except CTA Blue Line references

## Section Headers — NYC Subway Sign Style
- Black (#000) background, no outline/border
- Inset horizontal rule near top: #bbb light gray, 2px, full width, positioned 0.35rem from top
- Section name on left (1.8rem, weight 700, tight tracking)
- SVG circle badge with section number on right (46px, CTA line colors)
- Numbers optically centered using SVG `text-anchor="middle"` and `dominant-baseline="central"`
- Number "1" has dx="-1.5" nudge for optical centering
- Padding: 1.2rem top, 0.75rem horizontal, 0.7rem bottom
- 2rem margin-bottom to content

## Navigation
- SVG colored circle dots that start at scale 0.32 (tiny dot), expand to full size on hover/active
- Numbers hidden by default (SVG text opacity 0), revealed on hover/active
- Colors match section badges: blue=About, red=Experience, green=Projects, orange=Contact
- Active section tracked via scroll position in JS

## Experience Section — Subway Line Timeline
- Vertical 3px red line on left (var(--red), full opacity)
- Station dots: 16px circles, hollow (black fill, 3px red border) for past jobs
- Current job: filled red dot (class="current")
- Dashed extension below last station: 60px, repeating-linear-gradient (10px on, 5px off)
- Left padding: 2.5rem to make room for line
- Company name bold (#fff, 1.15rem, 600 weight) with date right-aligned (0.85rem, 400 weight, #fff)
- Role directly below company (1.05rem, 400 weight, #fff), no arrow
- Description below (0.95rem, #666)
- 2.5rem spacing between cards

## Project Cards
- Grid layout: 120px thumbnail left, content right
- Thumbnail: 120x80px, 1px border rgba(255,255,255,0.1), #111 background
- Title as `<a>` link with MTA-style SVG arrow (20px, flex-end aligned, top: 3px)
- Arrow hover: translate(2px, -2px)
- Tech tags as SVG route circles (32px colored circles with 2-letter abbreviations)
- Tags expand on hover to black pill with colored border showing full name
- 2rem spacing between cards with 1px divider lines

## SVG Route Tags (Tech Labels)
- 32px SVG circles with abbreviation text, colored per CTA lines
- On hover: wrapper expands to pill shape (32px tall, border-radius 16px)
- Pill has black background + 1.5px colored border matching the circle
- Circle overlaps pill border (z-index: 1, negative margins)
- Label fades in with 0.5rem left padding, 0.75rem right padding
- Abbreviation map: JS→red, D3→blue, No(de)→green, AP(I)→orange, HC(SS)→purple, Gi(t)→brown, CI→pink, Py→purple, Pi→green, etc.

## Arrows
- Custom SVG MTA-style arrows: chunky, thick stem, solid angular arrowhead
- Path: `M3 13L11.5 4.5H5V2.5H14.5V12H12.5V5.5L4 14L3 13Z`
- 20px size, white, only on links
- Aligned flex-end with `position: relative; top: 3px`
- Hover: `transform: translate(2px, -2px)`
- Only appear next to actual `<a>` links, never on plain text

## Splash Screen
- Content spacer: 100vh (50vh on mobile) of empty space so map is the hero
- "Live" sign: bottom-right of spacer, black background, bold white text on two lines
- Upper-left MTA arrow + "Live CTA Trains / The Loop, Chicago"
- Fades out on scroll

## JS Behaviors
- Map parallax + fade on scroll (starts fading immediately, reaches 0 by 70% of first viewport)
- Section reveal animations: opacity 0 → 1, translateY 20px → 0, triggered by scroll position
- Active nav tracking: updates based on which section has scrolled past 40% of viewport
- Iframe positioning: calculates content column center, applies CSS variable offset, recalculates on resize
- Vignette center tracks iframe position via CSS variable
- Live badge fade: tied to scroll position
- All scroll listeners use `{ passive: true }`

## Mobile (≤900px)
- Single column layout, sidebar not sticky
- Nav hidden
- Social icons: margin-top 2rem
- Name: 48px, letter-spacing -3px
- Title icon: 28px
- Content spacer: 50vh
- Project cards: single column grid, thumbnail full-width 140px tall
- Section badges: 38px
- Iframe: 100% centered, no JS offset
