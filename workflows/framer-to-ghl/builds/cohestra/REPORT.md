# Cohestra: Framer > GHL rebuild report

**Source:** https://cohestra.framer.media/ (homepage, Framer publish of Aug 23 2026)
**Build:** [`cohestra-ghl.html`](./cohestra-ghl.html), one file, one GHL Custom Code element
**Theme switch:** `const THEME = "framer" | "final"` (line 1 of the config block)
**Extraction method:** live computed styles from headless Chromium at 1440 / 810 / 390, Framer's appear-animation JSON, the page's compiled component modules (exact transition objects, spring configs, scroll math), and frame grabs of every video. Nothing was estimated from screenshots alone unless noted in the deviations table.

---

## 1. Section inventory

| # | Section | Layout | Background | Visual elements | Animations | Form |
|---|---|---|---|---|---|---|
| 0 | Nav | sticky top, flex row, 1160 container | transparent | star mark + wordmark, 2-line burger | burger > X morph, full-screen menu overlay | no |
| 0b | Menu overlay | fixed, grid 2 cols + switch | ink | 8 x 64px links, system/light/dark switch | fade in, links stagger .1/.2/.3s spring | no |
| 1 | Hero | stack, centred, pad 120/20/100 | ink + cursor-trail canvas | stats pill (4 avatars, blur 4), H1 96px, body, 2 buttons, 1160x660 video card r24, play button (80px glass) | trail (mouse + idle demo), line blur-in H1/body, fades on pill/buttons/video | no |
| 2 | Program | flex row 572 / 8 / 580, h540 | ink | 1:1 motion-loop tile (morphing white shape over red / meadow, portrait inside), text card surface 5% r24 | media slides from x-80, card from x+80, content rises y-60>0 in view | no |
| 3 | Testimonials | stack, gap 48, card 1160x580 r24 taupe | ink / taupe card | 3 quotes 44px, attribution, prev/next chevrons, cursor-reveal smoke image | title/body y20 spring (repeat), quote swap blur+x±10, reveal blob follows cursor, idle wander | no |
| 4 | Philosophy | stack, 4 cards 284x362 gap 8 | ink | 4 photo cards (image x1.06, veil 40%, 1px 20% border), index, title, hover description | y20 spring stagger 0/.1/.2/.3, 3D tilt spring on hover (8deg), description reveal | no |
| 5 | Founders stack | sticky stage, height (3+1.6) x 100vh | ink | title 64px + subtitle, 3 portrait cards 394x484 r16 with fade gradient | scroll-scrubbed: title fade/scale/lift, cards fly in on z/rotateX, stack, scale, veil, exit | no |
| 6 | Blog | stack, ticker row | ink | title, body, button, 5 article cards (346x352 image, 50% veil, tag pill, clock/calendar meta, 22px title) | parallax speed 105 (phone 104), ticker 50px/s, head y20 spring | no |
| 7 | Lead magnet | same split as Program | ink | 1:1 motion-loop tile (dusk meadow, shape morph), title 44px, body, **email form**, note | parallax speed 102, slide-ins, rise y-60>0 | **yes** |
| 8 | FAQ | stack, list max 840 gap 8 | ink | title, body, button, 6 rows (24px glyph, 22px question, chevron) | head y20 spring, rows flip in (rotateX -82deg, y72), accordion height .3s | no |
| 9 | CTA | box 1240x580 | ink + particle membrane canvas | title 64px, body, primary button | membrane 16s loop, title/body line rise y10 spring | no |
| 10 | Footer | grid 592/200/200/32, h450 | ink | contact block, 3 social tiles, 2 link columns, vertical theme switch, giant fading wordmark, legal row | link hover colour | no |

## 2. Element inventory

| Element | Where | Rebuilt as |
|---|---|---|
| Star logo mark | nav, menu | inline SVG `#i-star` |
| Burger / close | nav | 2 CSS bars, rotate to X |
| 4 graduate avatars (24px, -12 overlap) | stats pill | `ph-portrait` duotone discs, slots `grad-1..4` |
| Hero talking-head video 16:9 (poster + mp4) | hero | `ph-studio`: dark teal studio, warm subject, magenta rim, monitor panel; slot `hero-video` |
| Play / pause glyphs | hero | inline SVG |
| 9 abstract motion-study images | hero cursor trail | canvas-drawn gradient cards from palette tokens; slot `trail` (up to 16 URLs) |
| Program loop video (1200x1200, 7.3s) | program | `ph-loop`: SVG shape morph (blob > flower > squircle) over red/meadow, portrait clipped inside |
| Kit loop video (1200x1200, 4.62s) | lead magnet | `ph-loop` dusk-meadow variant |
| Testimonial reveal image | testimonial card | `ph-smoke` masked by SVG turbulence blob |
| Chevron arrows (24px) | testimonial, FAQ | inline SVG `#i-chev` |
| 4 meadow/floral motion-blur photos | philosophy | `ph-meadow` variants v1..v4 (streaked gradients, blur 6px) |
| 3 red studio portraits | founders | `ph-portrait` with Framer's fade gradient |
| 5 article photos | blog | `ph-meadow` variants v3..v6 |
| Clock / calendar icons (12px) | blog meta | inline SVG |
| 6 FAQ glyphs (24px, 40% ink) | FAQ | inline SVG q1..q6 (star, petal X, checker, burst, leaf, 4 dots) |
| Particle membrane (WebGL over video) | CTA | canvas 2D, 9,000 rim-lit particles, 16s loop |
| Threads / X / LinkedIn icons | footer | inline SVG |
| System / light / dark icons | footer, menu | inline SVG |
| Giant "COHESTRA" wordmark PNG (1322x183) | footer | SVG text, brand name from config, gradient rgb(212,185,174) 35% > 0 |
| "Get Cohestra" / "Made in Framer" badges | fixed bottom right | **excluded** (Framer marketplace chrome, see deviations) |

## 3. Design tokens

Extracted from Framer's own token CSS (dark + light sets) and computed styles.

```css
/* primitives (Framer) */
--p-ink: #0C0A09;  --p-paper: #FBFAF9;  --p-accent: #7D6D67;
--p-mist-1: #F3F1F1;  --p-mist-2: #E9E4E3;  --p-card: #1A1A1A;  --p-mark: rgb(212,185,174);

/* roles, dark mode */
--c-bg: ink;  --c-fg: paper;  --c-fg-70: paper 70%;  --c-fg-40: paper 40%;
--c-surface-1: paper 5%;  --c-surface-2: paper 10%;  --c-border: paper 20%;
--c-btn-1: paper on ink text;  --c-btn-2: paper 10% + blur(8px);
/* roles, light mode */
--c-bg: paper;  --c-fg: ink;  --c-fg-70: ink 70%;  --c-surface-1: #F3F1F1;  --c-surface-2: #E9E4E3;
--c-btn-1: ink on paper text;  --c-btn-2: #7D6D67 with paper text;

/* type: Outfit 400 / 600 */
--fs-h1: 96 / 68 / 44px;  line-height 1.02;  letter-spacing -0.04em
--fs-h2: 64 / 48 / 34px;  line-height 1.05;  letter-spacing -0.01em
--fs-h3: 44 / 36 / 28px;  line-height 1.10;  letter-spacing -0.01em
card title 22px/600/1.3;  blog title 22/20/18px 600;  FAQ question 22/20/18px 600
body 16 / 16 / 15px, line-height 1.34;  small 12px, +0.01em;  button 15px/18px, -0.02em
footer heading 20/18/17px;  address 14/14/13px;  menu 64px
text-wrap: balance on every Framer text preset

/* layout */
breakpoints: >=1200, 810-1199.98, <=809.98 (Framer's own)
container 1160 (content), 1240 (CTA/footer), gutter 20
section pad-y 120 desktop, 64 tablet/phone (program keeps 120; CTA 60 below 1200)
hero pad 120/20/100 desktop, 64/20/64 below 1200;  hero stack gaps 40
video card 1160x660 / 770x438 / 350x468

/* shape + depth */
radius: button 8, card 24, FAQ 16, founder card 16, blog image 16, pill 999, social 8
blur: stats pill 4px, secondary button 8px, play 6px
borders: philosophy 1px paper 20%, blog image 1px paper 10%
```

## 4. Motion map

Springs are Framer "duration + bounce" springs, converted with Framer Motion's own solver (damping ratio = 1 - bounce, natural frequency solved so the envelope settles within `duration`), integrated to rest and emitted as CSS `linear()`. Fallback where `linear()` is unsupported: `cubic-bezier(.22,1,.36,1)`.

| Element | Trigger | Properties | From > To | Duration | Delay | Stagger | Easing | Repeat |
|---|---|---|---|---|---|---|---|---|
| Stats pill | load | opacity | .001 > 1 | 1s | 0 | - | cubic-bezier(.44,0,.56,1) | no |
| Hero H1 | load | opacity, blur | .001 / 8px > 1 / 0, per line | .6s | .05 per line | .05s | cubic-bezier(.44,0,.56,1) | no |
| Hero body | load | opacity, blur | same, per line | .6s | .1 + .05/line | .05s | cubic-bezier(.44,0,.56,1) | no |
| Primary / secondary CTA | load | opacity | .001 > 1 | .6s | .3 / .4 | - | cubic-bezier(.44,0,.56,1) | no |
| Hero video card | load | opacity | .001 > 1 | .6s | 0 | - | cubic-bezier(.44,0,.56,1) | no |
| Cursor trail | pointer move / idle 1.8s | card spawn every 58px, scale 1.5>1, clip reveal, alpha | 88px cards, r10, life 1120ms | 1120ms | - | - | expo-out / cubic | loop (idle demo 17.5s bowed diagonal) |
| Play button | tap | icon swap | play > pause | instant | - | - | - | toggle |
| Program / kit media | in view | opacity, x | 0 / -80 > 1 / 0 | spring .4s bounce .2 | 0 | - | linear() spring | no |
| Program / kit card | in view | opacity, x | 0 / +80 > 1 / 0 | spring .4s bounce .2 | 0 | - | linear() spring | no |
| Program / kit content | scroll-linked | y | -60 > 0 | viewport bottom > centre | - | - | linear | scrubbed |
| Loop tiles | in view | SVG path morph, bg swap | blob > flower > squircle | 7.3s / 4.62s | - | - | cubic in-out | loop |
| Section titles + bodies + buttons | in view (threshold .5 / 0) | opacity, y | 0 / 20 > 1 / 0 | spring .4s bounce .2 | 0 / .1 / .2 | - | linear() spring | yes (re-animates on exit) |
| Quote block | scroll-linked | y | -30 > -1 | viewport bottom > centre | - | - | linear | scrubbed |
| Quote swap | tap prev/next | opacity, blur, x | .001 / 8px / ±10 > 1 / 0 / 0 | spring 1s bounce 0 | .075 (quote), .175 (attribution) | - | linear() spring | on each swap |
| Testimonial reveal | pointer / idle | radius, position, opacity | idle r .20 op .74 wander .3 range; hover r .42 op 1 | smoothing .1, blend .05, grow 1.3 | - | - | per-frame lerp | loop |
| Philosophy cards | in view | opacity, y | 0 / 20 > 1 / 0 | spring .4s bounce .2 | 0 / .1 / .2 / .3 | .1s | linear() spring | yes |
| Philosophy tilt | hover | rotateX, rotateY | 0 > ±8deg | spring k220 c22 m.6 | - | - | physics | follows pointer |
| Philosophy description | hover | opacity, height, y | 0 / 0 / 8 > 1 / auto / 0 | .4s | .1s | - | cubic-bezier(.22,1,.36,1) | toggle |
| Founders title | scroll-scrubbed | opacity, y, scale | enter fade over .95vh, lift -300 > 0; shrink to .85; exit -600 | section scroll | - | - | linear | scrubbed |
| Founder cards | scroll-scrubbed | opacity, y, z, rotateX, scale, veil | y 912 z 750 rotX 90 > 0; stack -50.25/card, scale -.125/card, veil +.1/card; exit -450 | 1/(3+1.6) of section each | per card | card index | linear | scrubbed |
| Blog section | scroll-linked | y | -(speed-100)% of scrollY, speed 105 (phone 104) | continuous | - | - | linear | scrubbed |
| Blog ticker | loop | x | 0 > -track | 50 px/s | - | - | linear | infinite |
| Lead magnet section | scroll-linked | y | speed 102 | continuous | - | - | linear | scrubbed |
| FAQ rows | in view | opacity, y, rotateX | 0 / 72 / -82deg > 1 / 0 / 0, perspective 900 | .3s | - | - | cubic-bezier(.2,.8,.2,1) | no |
| FAQ open / close | tap | height, chevron rotate | 0 > auto, 0 > 180deg | .3s | - | - | cubic-bezier(.2,.8,.2,1) | toggle |
| CTA title / body | in view (.5) | opacity, y, per line | .001 / 10 > 1 / 0 | spring .4s bounce 0 | 0 / .1 + .075 per line | .075s | linear() spring | no |
| Membrane | loop | particle rim deformation | 16s cycle | 16s | - | - | sine | infinite |
| Menu links | open | opacity, y | 0 / 20 > 1 / 0 | spring .4s bounce .2 | .1 / .2 / .3 | .1s | linear() spring | per open |
| Buttons | hover | opacity | 1 > .85 | .3s | - | - | cubic-bezier(.44,0,.56,1) | - |
| Arrow buttons | hover | bg, icon colour | transparent > paper 10%, 70% > 100% | spring .4s bounce .2 | - | - | - | - |
| Smooth scroll | wheel / keys | window scroll | exponential glide, tau = max(45, .8x150) = 120ms | - | - | - | exp | - |
| Colour mode | switch | all role tokens | dark <> light | .3s | - | - | cubic-bezier(.44,0,.56,1) | - |

`prefers-reduced-motion`: every animation and transition is dropped, final states stay visible, loops stop, parallax is off.

## 5. Colour role map (Framer > role > final)

No palette was supplied, so THEME "final" currently carries the Framer primitives. Every role flows from the 7 primitives, so pasting your palette into `[data-theme="final"]` rebrands the whole page, mockups included.

| Framer colour | Role | Final (current) | Notes |
|---|---|---|---|
| #0C0A09 | `--p-ink`: dark bg, light-mode text, primary button (light) | #0C0A09 | PASTE your dark |
| #FBFAF9 | `--p-paper`: light bg, dark-mode text, primary button (dark) | #FBFAF9 | PASTE your light |
| #7D6D67 | `--p-accent`: testimonial card, light-mode secondary button | #7D6D67 | PASTE your accent |
| #F3F1F1 | `--p-mist-1`: light-mode surface 1 (cards, FAQ, input) | #F3F1F1 | derive: paper 95% + ink |
| #E9E4E3 | `--p-mist-2`: light-mode surface 2 (pill, social, active switch) | #E9E4E3 | derive: paper 90% + accent |
| #1A1A1A | `--p-card`: founder card base | #1A1A1A | |
| rgb(212,185,174) | `--p-mark`: footer wordmark | same | |
| paper 5 / 10 / 20 / 40 / 70% | surfaces, border, faint text, body text | colour-mix of paper | |
| duotones (studio, portrait, meadow, smoke, dusk) | photo placeholders | colour-mix of accent / ink / paper | placeholders recolour with the palette |

**Contrast checks (final theme):**

| Pair | Framer ratio | Fix in final | Result |
|---|---|---|---|
| paper 40% on ink (kit note, footer heading, copyright) | 3.7:1 | `--fg-40-dark: 50%` | 5.2:1 |
| ink 40% on paper (same, light mode) | 2.7:1 | `--fg-40-light: 62%` | 5.6:1 |
| paper 70% on taupe (testimonial attribution) | 3.2:1 | `--on-accent-muted: 100%` | 4.7:1 |
| all other text pairs | >= 4.5:1 | none | pass |

THEME "framer" keeps Framer's exact values.

## 6. Image slot map

| Slot | Framer element | Placeholder | Your image | object-fit | object-position |
|---|---|---|---|---|---|
| `grad-1..4` | graduate avatars 24px | `ph-portrait` disc | PASTE | cover | 50% 50% |
| `hero-video` | 16:9 talking-head video + poster | `ph-studio` + play button | `VIDEOS.final["hero-video"]` | cover | 50% 50% |
| `trail` | 9 abstract motion studies (trail cards) | palette gradient cards on canvas | `IMAGES.final.trail` (array) | cover (aspect clamped .72-1.7) | centre |
| `program-loop` | 1:1 motion loop video | `ph-loop` morph + portrait | `VIDEOS.final["program-loop"]` | cover | 50% 50% |
| `testimonial-reveal` | smoke portrait revealed by cursor | `ph-smoke` | PASTE | cover | 50% 50% |
| `phil-1..4` | motion-blur meadow / floral photos 284x362 (x1.06) | `ph-meadow` v1-v4 | PASTE | cover | 50% 50% |
| `founder-1..3` | red studio portraits 394x484 | `ph-portrait` + fade gradient | PASTE | cover | 50% 30% |
| `blog-1..5` | article photos 346x352 (overscan 56px) | `ph-meadow` v3-v6 | PASTE | cover | 50% 50% |
| `kit-loop` | 1:1 motion loop video | `ph-loop` dusk variant | `VIDEOS.final["kit-loop"]` | cover | 50% 50% |
| logo | star mark + "Cohestra" | SVG star + brand text | `BRAND.final.logoSrc` | - | - |

If your image's aspect ratio differs, the Framer frame is kept and yours is cropped to it with `object-position`.

## 7. HTML file

[`cohestra-ghl.html`](./cohestra-ghl.html): 101 KB, THEME set to `"final"`. Paste the whole file into one Custom Code element.

## 8. GHL Form Custom CSS (final theme)

[`ghl-form-custom.css`](./ghl-form-custom.css). The light-mode variant and the exact THEME "framer" values are at the bottom of the file.

## 9. Form Builder settings

| Setting | Value |
|---|---|
| Form name | Starter Kit |
| Fields | Email (required), hidden: utm_source, utm_medium, utm_campaign, utm_content, utm_term, page_source |
| Layout | Single column, "Label" above field |
| Styles > Background | transparent (0% opacity) |
| Styles > Width | 100% |
| Styles > Padding / margin | 0 |
| Styles > Border | none, radius 0, shadow none |
| Font | Outfit (or set by Custom CSS) |
| Label | 12px, rgba(251,250,249,.7) |
| Field background | rgba(251,250,249,.05), radius 8, height 55 |
| Placeholder | `example@cohestra.com` |
| Button text | `Send Me the Kit` |
| Button | bg #FBFAF9, text #0C0A09, radius 8, height 55, auto width |
| On submit | Open URL = `FX_CONFIG.REDIRECT_URL` (or inline message) |
| Custom CSS | paste `ghl-form-custom.css` |
| Embed | handled by the page (inline iframe + form_embed.js); just copy the Form ID |

## 10. Field mapping sheet

| Framer label | GHL field | Merge key | Type | Required | Placeholder | Create? |
|---|---|---|---|---|---|---|
| Email | Email (standard) | `{{contact.email}}` | Email | yes | example@cohestra.com | no |
| (hidden) | utm_source | `{{contact.utm_source}}` | Single line text, Query Key `utm_source` | no | - | **yes**, folder "Attribution" |
| (hidden) | utm_medium | `{{contact.utm_medium}}` | Single line text, Query Key `utm_medium` | no | - | **yes**, folder "Attribution" |
| (hidden) | utm_campaign | `{{contact.utm_campaign}}` | Single line text, Query Key `utm_campaign` | no | - | **yes**, folder "Attribution" |
| (hidden) | utm_content | `{{contact.utm_content}}` | Single line text, Query Key `utm_content` | no | - | **yes**, folder "Attribution" |
| (hidden) | utm_term | `{{contact.utm_term}}` | Single line text, Query Key `utm_term` | no | - | **yes**, folder "Attribution" |
| (hidden) | page_source | `{{contact.page_source}}` | Single line text, Query Key `page_source` | no | - | **yes**, folder "Attribution" |

The page forwards `utm_*`, `gclid` and `fbclid` from its own URL into the iframe and appends `page_source=cohestra-home`. GHL fills each hidden field whose Query Key matches. Verified: `https://api.leadconnectorhq.com/widget/form/<ID>?utm_source=fb&utm_campaign=kit-launch&gclid=abc&page_source=cohestra-home`.

## 11. Asset upload checklist

| Asset | Source | Slot variable | Uploaded |
|---|---|---|---|
| Logo (SVG/PNG, ~88x18) | you | `BRAND.final.logoSrc` | N |
| Brand name | you | `BRAND.final.name` | N |
| 4 graduate avatars (96x96) | you | `IMAGES.final["grad-1..4"]` | N |
| Hero video (16:9 mp4) + poster | you | `VIDEOS.final["hero-video"]` | N |
| Program loop (1:1 mp4, muted) | you | `VIDEOS.final["program-loop"]` | N |
| Kit loop (1:1 mp4, muted) | you | `VIDEOS.final["kit-loop"]` | N |
| Testimonial reveal image | you | `IMAGES.final["testimonial-reveal"]` | N |
| 4 philosophy images (portrait) | you | `IMAGES.final["phil-1..4"]` | N |
| 3 founder portraits (394:484) | you | `IMAGES.final["founder-1..3"]` | N |
| 5 article images | you | `IMAGES.final["blog-1..5"]` | N |
| Trail images (up to 16) | you | `IMAGES.final.trail` | N |
| Palette (7 primitives) | you | `[data-theme="final"]` | N |
| GHL Form ID | GHL | `FX_CONFIG.FORMS.kit.id` | N |
| Redirect URL | you | `FX_CONFIG.REDIRECT_URL` | N |
| All page links | you | `FX_CONFIG.LINKS` | N |

Upload images to GHL Media Library (Sites > Media) and paste the public URLs.

## 12. GHL setup

See [`GHL-SETUP.md`](./GHL-SETUP.md).

## 13. QA

Measured in headless Chromium inside a mock GHL shell (1170px container, 15px gutters), Outfit loaded.

| Criterion | Result | Notes |
|---|---|---|
| 1. Framer theme vs Framer at 1440 / 810 / 390, spacing within 2px | **pass** | Every section top and height within 1px at all three widths. Page height 11457 / 10707 / 12869 vs Framer 11457 / 10707 / 12870. H1 glyph runs match to 1px. Line breaks match (text-wrap: balance + per-character spans, like Framer). |
| 2. Motion map implemented | **pass** | All rows above. Springs converted with Framer Motion's solver. Two WebGL effects rebuilt without WebGL (see deviations). |
| 3. Final = framer element for element | **pass** | Same DOM. Only primitives, images, videos, brand and 3 contrast lifts differ. |
| 4. Mockups recolour, no leftover Framer colours | **pass** | Placeholders read only `--duo-*` / `--p-*` tokens. Final duotones derive from the palette via color-mix. |
| 5. Full-bleed, zero horizontal scroll | **pass** | scrollWidth - clientWidth = 0 at 1440 / 810 / 390, page left = 0, width = viewport. Also tested with an `overflow:hidden` GHL row (wrappers un-clipped automatically) and with `body{overflow-x:hidden}` (sticky JS fallback): edge to edge, no errors. |
| 6. GHL form | **partial** | Embed, UTM forwarding, page_source, auto-resize script verified. Submission, contact creation and field fill need your live Form ID (can't be tested without your account). The side-by-side input + button uses GHL class names that should be checked once in your form. |
| 7. Lighthouse mobile 85+, CLS < 0.1, no console errors | **pass** | Performance 94-96 across 4 runs, Accessibility 95, Best Practices 100, LCP 1.3-1.5s, TBT 210-300ms, CLS 0.001, 0 errors. (The Framer original scores 33 in the same setup.) |
| 8. No Framer code or class names | **pass** | 0 matches for `framer-*`, `data-framer*`, `__framer`, framerusercontent. |
| 9. Config-only rebrand | **pass** | Palette, fonts, brand, images, videos, form, links, sections, motion speed, mode, smooth scroll all in the two config blocks. |

## 14. Deviations

| Element | Framer value | Build value | Why |
|---|---|---|---|
| CTA "Particle Membrane" | WebGL shader, 144k particles sampling an embedded 10s video | Canvas 2D, 9k rim-lit particles on a noise-deformed contour, 16s loop | No libraries or WebGL runtime required. Same look: wispy glowing blob, ink/paper aware. Swap in a video later if you want the exact source. |
| Testimonial cursor reveal | WebGL fbm domain-warped mask | CSS radial mask + SVG feTurbulence displacement, same radius / idle / grow / smoothing parameters | Same reason. The edge is organic but not identical noise. |
| Loop tiles (program, kit) | 1200x1200 mp4 motion graphics | SVG shape morph over palette backgrounds (placeholder) | Placeholder rule. Your own mp4 drops in via `VIDEOS`. |
| Photo / video content | real photography | tokenised duotone placeholders with slot labels | Placeholder rule. Labels hide via `SHOW_SLOT_LABELS: false` or when an asset is set. |
| Media clipping in split sections | video rendered 664x626 inside a 572x540 box (container clip not readable) | clipped to the box with radius 24 | Clip/radius on the media box couldn't be read from computed styles. Matched the card radius. |
| Colour mode persistence | Framer stores the choice in localStorage | in-memory only | localStorage is forbidden by the brief. |
| GHL form in light mode | Framer input follows the mode | GHL iframe keeps the colours from its Custom CSS | Page CSS can't reach inside the iframe. Pick the matching CSS variant for your default mode. |
| Form row layout | email + button inline | same via Custom CSS on GHL class names | GHL form DOM class names vary by builder version. Falls back to stacked (the Framer mobile layout). |
| "Get Cohestra" / "Made in Framer" badges | fixed bottom-right | removed | Framer marketplace chrome, not part of the page design, and would be Framer branding. |
| Menu open transition | overlay transition not exposed | .5s opacity on the bounce-.2 spring, links stagger .1/.2/.3 | Only the link springs were readable. |
| Mobile menu link size | not captured | 32px | 64px overflows two columns at 390px. |
| Hero trail on touch devices | starts immediately | starts after page load + the 1.8s idle delay | Keeps it off the critical path (TBT). Same motion once running. |
| Smooth scroll | wheel hijack on all pages | same behaviour, toggle `SMOOTH_SCROLL.enabled` | Kept. Turn off if GHL chat widgets or popups need native wheel. |
| CTA body line break at 1440 / 810 | Framer renders "leading i / nternational" (a mid-word glitch from its character tokens; the copy itself is clean) | "leading / international", every other break identical; 390 matches exactly | Reproducing the glitch would make the mobile breaks wrong and ship a visible typo-like break into your final theme. Add `data-fx-chars` to the `<p>` if you want the glitch back. |
| Footer "Apollo Studio" link | colour-only distinction | same | Framer design. Lighthouse flags `link-in-text-block`. Add an underline if you want the point. |
| Final-theme faint text | 40% | 50% dark / 62% light / 100% on taupe | Contrast rule in the brief. THEME "framer" keeps 40%. |
