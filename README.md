# MRS Dental Shop — coming soon page

A single static page. No build step, no dependencies, no JavaScript.

```
index.html          the page
styles.css          all styling and animation
assets/mrs-logo.jpg the logo (also used as the favicon)
robots.txt
```

## Before going live

The contact links are live:

| What | Value |
|---|---|
| Facebook page | `https://www.facebook.com/share/1HQRiVQHAm/` |
| WhatsApp | `+880 1822-392916` → `https://wa.me/8801822392916` |

The WhatsApp link takes the number in international format with no `+`, spaces or
dashes, which is why `+880 1822-392916` is written `8801822392916`.

One placeholder is left — the domain, in `canonical`, `og:url`, `og:image` and
`robots.txt`, currently `https://www.mrsdentalshop.com/`. It only affects how
the page looks when shared and how search engines index it, not the page itself.

## Viewing it locally

Open `index.html` directly in a browser, or serve the folder:

```bash
python -m http.server 8000
```

## Deploying

Upload the four files, keeping the `assets/` folder next to `index.html`. Any
static host works — Netlify, Vercel, GitHub Pages, or plain cPanel hosting.

## Notes on the design

This is a direct port of the two Design artboards, not a reinterpretation:

| | Base styles | `@media (min-width: 760px)` |
|---|---|---|
| Source artboard | `Mobile.dc.html` (390x844) | `Main.dc.html` (desktop) |
| Headline | two lines, "Website / Coming Soon", 40px | one line, "Website is coming soon" |
| Emblem | fixed 300x234, anchored at 45% | `clamp(260px, 43vh, 580px)`, 23:9 ratio, 50% |
| Buttons | stacked, 54px tall | side by side, 46px tall |
| Sparks | 5 | 10, different positions |
| Blobs | 2 | 3 |

Values that differ between the boards (drift distances, blur radii, animation
delays, the emblem anchor) are carried as CSS custom properties, so one set of
`@keyframes` serves both and the desktop block only redefines the variables.

- Colours come from the logo: orange `#F7941D`/`#F26322`, blue `#1570CD`/`#0E5AA8`.
  They are CSS custom properties at the top of `styles.css`, so the whole page
  re-themes from four lines.
- The page locks to one viewport height at 760px wide and up; below that it
  scrolls, using `100svh` so a mobile URL bar does not clip the footer.
- `prefers-reduced-motion` stops every animation and transition.
- The logo card sits right of the emblem's centre, which uncovers the drawn
  tooth outline behind it. `--logo-shift-x` controls it, as a percentage of
  the emblem width; `0%` returns it to dead centre. It is set three times:
  `35%` on desktop, `25%` on phones (the emblem is a fixed 300px there, so
  more would reach the page gutter) and `12%` under 360px wide, where the
  card would otherwise be clipped at the screen edge.

### One deliberate difference from the artboards

The artboards' `pop` and `float` keyframes set `transform: translateY(...)`
without re-stating the `translate(-50%, -50%)` that centres the logo card. CSS
replaces the whole `transform`, so the card leaves its anchor as soon as either
animation takes over — permanently, once the infinite float starts. Here the
centring offset is repeated inside every keyframe. If you ever edit those
keyframes, keep it.
- The logo is a JPG with a white background, which is why it sits on a white
  card. A transparent PNG or SVG would let it sit directly on the tinted
  background — drop it in as `assets/mrs-logo.png` and update the `<img src>`.

## Replacing the logo

The favicon points at the same file. For sharper browser-tab and home-screen
icons, export a 512x512 PNG and swap both `<link rel="icon">` and
`<link rel="apple-touch-icon">`.
