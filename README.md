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
| Facebook page | `https://www.facebook.com/mrsdentalshop` |
| WhatsApp | `+880 1822-392916` → `https://wa.me/8801822392916` |

The WhatsApp link takes the number in international format with no `+`, spaces or
dashes, which is why `+880 1822-392916` is written `8801822392916`.

The domain is `https://www.mrsdentalshop.com/`, already set in `canonical`,
`og:url`, `og:image` and `robots.txt`. The one thing left is `og:image` — the
logo is nearly square and gets cropped in link previews, so a 1200x630 share
image would look better.

## Hosting on GitHub Pages

`CNAME` in the repo root holds the custom domain. GitHub Pages reads it
automatically — do not delete or rename it.

Two steps happen outside this repo:

**1. Enable Pages** — repo Settings → Pages → Source: *Deploy from a branch*,
branch `main`, folder `/ (root)`. The custom domain fills itself in from
`CNAME`. Tick **Enforce HTTPS** once the certificate is issued.

**2. DNS at Namecheap** (where the domain is currently parked) — Domain List →
Manage → Advanced DNS. Remove the parking records first, then add:

| Type | Host | Value |
|---|---|---|
| CNAME | `www` | `mostakim3157.github.io.` |
| A | `@` | `185.199.108.153` |
| A | `@` | `185.199.109.153` |
| A | `@` | `185.199.110.153` |
| A | `@` | `185.199.111.153` |

The four A records point the bare domain at GitHub, which then redirects to
`www`. IPv6 is optional: AAAA `@` to `2606:50c0:8000::153` through
`2606:50c0:8003::153`.

DNS usually propagates in minutes but can take up to 24 hours, and the HTTPS
certificate is only issued after it resolves.

## Director's card — `/director/`

`director/` is the digital visiting card the QR code on the director's printed
card points to. Encode `https://www.mrsdentalshop.com/director/` in the QR code
(the printed card uses `/director` without the slash; GitHub Pages redirects it).

```
director/index.html   the card page
director/director.css its styling (same colours and fonts as the main page)
director/contact.vcf  the "Save contact" file
```

The director's details appear in both `index.html` and `contact.vcf`, so when
one changes, change the other too. The page is marked `noindex` so the phone
number stays out of search results.

## Updating the styles

Both pages link their stylesheet with a version number, e.g.
`styles.css?v=20261019`. GitHub Pages lets browsers keep files for 10 minutes,
so whenever you change `styles.css` or `director/director.css`, change the
`?v=` number in the matching `index.html` to today's date. Visitors then get
the new styles together with the new page instead of a stale cached copy.

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
