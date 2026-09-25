# CourseKit: Framer > GHL rebuild report

**Source:** https://coursekit.framer.ai/ (homepage, Framer publish of Jun 3 2026, delivered as `course_2.zip` + live page)
**Build:** [`coursekit-ghl.html`](./coursekit-ghl.html), one file, one GHL Custom Code element (117 KB)
**Theme switch:** `const THEME = "framer" | "final"` (first line of the config block)
**Extraction method:** live computed styles and offset geometry of every `data-framer-name` layer in headless Chromium at 1440 / 810 / 390, Framer's appear-animation JSON, the compiled page modules (transition objects, ticker props, counter props, slider springs, component variants), and the original decorative PNGs traced into SVG geometry. Nothing was estimated from screenshots alone unless the deviations table says so.

---

## 1. Section inventory

| # | Section | Layout | Background | Visual elements | Animations | Form |
|---|---|---|---|---|---|---|
| 0 | Nav | fixed pill, 725 wide, top 15 (10 on phone) | panel #141414 r15 | logo mark + wordmark, 4 links, white "Join Community" button; burger < 1200 | drop-in y-50 spring, burger > X, dropdown (4 links + button) | no |
| 1 | Hero | stack, centred, pad 140/60/100 | ink + 2 gold light beams | Trustpilot row (4.8/5, 5 stars), H1 60px Roboto 700, body 18px, button, 900 wide video frame r28 | beams rise 125px (4s spring), H1 word blur-in, body/button rise, video tilt rotateX 20 / scale .8 / y-80 flattening on scroll | no |
| 2 | Logo rail 1 | row, 600 wide masked | ink | caption + 8 logos | marquee 50px/s | no |
| 3 | Roadmap | frame 1320 r25, timeline 1000 | panel | badge, H2, sub, 4 rows (400x225 media + UI card, text), centre rail + numbered dots | rows rise 25px, UI cards slide x100 (2s spring) | no |
| 4 | Features bento | 6-col grid, 3 + 2 cards | ink | search, calendar, levels, chats, classroom mockups (glow pop, search slide) | rise, glow pop scale .25 > 1 (bounce .5), search slide | no |
| 5 | Stats | 3 columns + dividers (lines on phone) | ink | $120M+, 1000+, 565% | counters: +1 per 10 / 25 / 24 ms from 0 / 950 / 500 | no |
| 6 | Logo rail 2 | as 2 | ink | as 2 | as 2 | no |
| 7 | Wins | slider 725x408 (462x300 tablet, 238x300 phone), gap 80 | ink + 2 gold beams | 3 photo slides with title, quote, name, role; prev/next | auto-advance 5s, spring k500 c60, beams rise/fall 100px (3s) | no |
| 8 | Course modules | frame r25, list 610 + circle card 350 | panel | 6 module rows (videos / documents / hours chips), about block, "Jordan's Inner Circle" card | rows rise | no |
| 9 | Pricing | stack, card 350 (315 phone) | ink + 2 gold arcs | badge, H2, sub, price panel (LIMITED TIME, $49 gradient, per month), 4 benefits, button, "Cancel anytime" | rise, arcs rise/fall 100px (3s spring) | no |
| 10 | Testimonials | masonry 3 / 2 / 1 columns | ink | 9 cards (desktop), 6 (tablet), 4 (phone), redistributed per breakpoint like Framer | rise | no |
| 11 | Team | grid 3 / 2 / 1, cards 1f | panel frame | 5 cards: name, role, signature, portrait | rise | no |
| 12 | FAQ | intro + list 5 | ink | badge, H2, sub, 5 accordion rows (gold inset glow open) | accordion height, chevron rotate | no |
| 13 | CTA | box 1200x498 r25 | ink > gold 3% + noise + glow | mark, H2, sub, button, vertical testimonial ticker (desktop) | ticker 25px/s reverse, rise | no |
| 14 | Footer | inside rounded wrapper | ink > black | logo, 4 link columns, divider, copyright, 3 social icons | link hover | no |

The homepage has **no form**. Every "Join our community" / "Join Community" CTA goes to `LINKS.join` (your GHL order form, checkout or funnel step) with UTM passthrough.

## 2. Element inventory

| Element | Where | Rebuilt as |
|---|---|---|
| Logo mark (gold "C" tile) | nav, CTA, footer, circle card | CSS tile, `data-brand-mark` (swap with `BRAND.faviconSrc`) |
| Wordmark | nav, footer | text `data-brand-name` (swap with `BRAND.logoSrc`) |
| Hero light beams (PNG 1496x2276, x2) | hero | SVG `#beam`: traced edge line, gold band, warm body, glow, blur 6px, `slice` scaling like `object-fit: cover` |
| Wins beams (same PNG, 280x382) | wins | same `#beam` symbol |
| Pricing arcs (PNG 2064x2493, x2) | pricing | SVG `#arc`: two circle fits of the traced band (r302 / r253), gold > deep gradient |
| Hero YouTube embed | hero | `ph-studio` poster (warm studio, subject, lamp) + play button; real embed when `VIDEO` is set |
| 8 client logos | rails | wordmark placeholders; `LOGOS` takes image URLs |
| Roadmap photos (4) | roadmap | `ph-boxes` variants (cardboard, grid shelves, product, warehouse) |
| Roadmap UI cards (Store Visitors, A/B Testing, Daniel Messaged, Shipped To USA) | roadmap | HTML/CSS mockups, exact type and bars |
| Feature mockups (search, calendar, levels, chats, classroom) | features | HTML/CSS mockups |
| Win portraits (3) | wins | `ph-grey` portraits |
| Circle card photo | modules | `ph-studio` |
| Testimonial avatars (9) | testimonials, CTA ticker | `ph-face` duotone discs |
| Team portraits (5) + signatures | team | `ph-mono` portraits + inline SVG signatures |
| Icons (video, doc, clock, calendar, lock, search, gear, reply, users, phone, chevron, socials) | throughout | inline SVG symbols |
| Trustpilot stars | hero | CSS stars (green tiles, clip-path star) |
| "Made in Framer" badge, "Funnel Pages" switcher | Framer chrome | not rebuilt (see deviations) |

## 3. Design tokens

**Primitives (config block 2):**

| Token | framer | Role |
|---|---|---|
| `--p-ink` | #0B0B0B | page |
| `--p-panel` | #141414 | frames, nav, cards |
| `--p-panel-2` | #1C1C1C | rows, team cards, feature tops |
| `--p-paper` | #FFFFFF | text, buttons |
| `--p-gold` | #FFD980 | accent text, bars, icons |
| `--p-gold-1 / -2 / -3` | #F8DF42 / #FFF599 / #E1A427 | badge gradient, beams |
| `--p-gold-deep` | #B0882C | price gradient end, arc tail |
| `--p-mute` | #908D8D | mockup secondary text |
| `--p-mock` | #47443B | mockup card highlight |

**Derived roles:** text at 80 / 70 / 60 / 50 / 40 / 10 / 4% of paper, `--g-badge` (168deg, 16 / 40.09 / 100%), `--g-price` (160deg), `--g-mock` (radial 60% 88% at 10.6% 0%), shadows `--sh-frame` 0 3 6 .3, `--sh-card` 0 2 4 .25, `--sh-badge` (4-layer Framer stack).

**Type:** Inter Display (Inter with `opsz 32`) for body and UI, Roboto 700 for H1, logo and mockups.

| Token | 1440 | 810 | 390 |
|---|---|---|---|
| H1 | 60 / 1.1 / -0.03em | 40 | 32 |
| H2 | 40 / 1.2 / -0.02em | 35 | 26 |
| Sub | 18 / 1.5 | 18 | 16 |
| Win title | 30 | 22 | 20 |
| Stat | 40 | 27 | 27 |
| Price | 60 | 60 | 60 |

**Breakpoints:** Framer's own, 1200 and 680 (`max-width: 1199.98px`, `679.98px`).
**Radii:** nav 15, frames 25, cards 15 / 17 / 10, buttons 10, video 28 / 22 (18 / 14 phone).
**Fonts:** Google Fonts only. The fallback faces are metric-matched (Arial at 98% for regular, Arial Bold at 97% for 500+), so text widths match the web fonts to within 1% and nothing shifts when they arrive.

## 4. Motion map

Framer springs (duration + bounce) are converted at runtime with Framer Motion's own solver into CSS `linear()` curves, with a cubic-bezier fallback. `MOTION_SPEED` scales every duration.

| # | Target | Trigger | From > to | Timing |
|---|---|---|---|---|
| 1 | Nav | load | y-50, opacity 0 > 1 | spring 1.5s, bounce .2 |
| 2 | Hero beams | load | y+125 (left rotated 180) > 0 | spring 4s, bounce .2 |
| 3 | H1 words | load | blur 10, y15, opacity 0 > clear | spring 1.5s, word stagger |
| 4 | Hero body, button | load | y25 > 0 | spring 1.5s, delay .2 / .3 |
| 5 | Video frame | load | opacity 0 > 1 | spring 2s, bounce .2, delay .4 |
| 6 | Video tilt | scroll | rotateX 20, scale .8, y-80 > flat | scrubbed over the hero |
| 7 | Logo rails | always | marquee | 50px/s, masked edges |
| 8 | Section heads, rows, cards | in view | y25 > 0 | spring 1s |
| 9 | Roadmap UI cards | in view | x100 > 0 | spring 2s, bounce .3 |
| 10 | Feature glow | in view | scale .25 > 1 | spring 2s, bounce .5 |
| 11 | Stats counters | in view | 0 / 950 / 500 > 120 / 1000 / 565 | +1 per 10 / 25 / 24 ms |
| 12 | Wins slider | auto 5s + arrows | x ± (width + 80) | spring stiffness 500, damping 60 |
| 13 | Wins / pricing beams and arcs | in view | y±100 > 0 | spring 3s, bounce .2 |
| 14 | Pricing card | in view | y40 > 0 | spring 2s |
| 15 | FAQ | click | height 0 > auto, chevron 180deg, gold inset glow | .3s ease |
| 16 | CTA ticker | always | vertical loop, reverse | 25px/s |
| 17 | Buttons | hover | background > paper 70% | .3s ease |

`prefers-reduced-motion`: reveals show instantly, counters show final values, and the tickers, slider auto-advance and video tilt stay still.

## 5. Colour role map (Framer > role > final)

| Framer value | Role | final theme |
|---|---|---|
| #0B0B0B | page | `--p-ink` |
| #141414 | panel | `--p-panel` |
| #1C1C1C | panel 2 | `--p-panel-2` |
| #FFFFFF | text, buttons | `--p-paper` |
| rgba(255,255,255,.4) | tertiary text | 55% in final (contrast lift) |
| #FFD980 | accent | `--p-gold` |
| #F8DF42 > #FFF599 > #E1A427 | badges, beams | `--p-gold-1..3` |
| #B0882C | price tail | `--p-gold-deep` |
| photo duotones | placeholders | derived from gold / paper / panel with `color-mix` |

Rebrand = edit the primitives in `.fx-page[data-theme="final"]`. Every placeholder, beam, arc, badge and gradient recolours from them.

## 6. Image slot map

| Slot | Framer asset | Size | Placeholder |
|---|---|---|---|
| `hero-video` | YouTube embed | 900x484 (16:9) | `ph-studio` + play |
| `road-1..4` | product / warehouse photos | 400x225 | `ph-boxes` v1-v4 |
| `win-1..3` | client portraits | 725x408 | `ph-grey` |
| `circle-photo` | creator at desk | 300x168 | `ph-studio` |
| `creator` | small avatar | 21x21 | `ph-face` |
| `t-jake`, `t-sarah`, `t-sophie`, `t-chris`, `t-ryan`, `t-natalie`, `t-priya`, `t-marcus`, `t-daniel` | testimonial avatars | 44x44 | `ph-face` |
| `team-1..5` | team portraits (mono) | 145x145 | `ph-mono` |
| `chat-1`, `chat-2`, `search-avatar`, `msg-avatar` | mockup avatars | 24-36 | `ph-face` |
| `LOGOS[]` | 8 client logos | ~100x20 | wordmarks |

Fill with `FX_CONFIG.IMAGES.final["slot"] = { src, alt, pos }`. `SHOW_SLOT_LABELS` prints each slot's name on the placeholder; turn it off before launch.

## 7. HTML file

[`coursekit-ghl.html`](./coursekit-ghl.html). Config 1 (JS) and config 2 (tokens) sit at the top and are the only places to edit. Everything is scoped under `.fx-page`, with no Framer class names, no external libraries and no tracking.

## 8. GHL Form Custom CSS (final theme)

The page has no form. [`ghl-form-custom.css`](./ghl-form-custom.css) is included so a form dropped onto the page later matches it. It uses 53px inputs in `#1C1C1C` with radius 10, the white 600-weight "Join our community" button and Inter Display, and every `!important` is commented.

## 9. Form Builder settings

Only if you add a form (see GHL-SETUP.md, step 3b):

- Fields: Email (required), then the Attribution hidden fields `utm_source`, `utm_medium`, `utm_campaign`, `utm_content`, `utm_term`, `page_source`, each with a matching Query Key.
- Styles: transparent background, width 100%, padding 0, no border or shadow, then paste the Custom CSS.
- On submit: redirect to your offer page or show a message.

## 10. Field mapping sheet

| Source | GHL field | How |
|---|---|---|
| `?utm_source=` etc. on the page URL | Attribution custom fields | form iframe URL carries them (hidden field Query Key) |
| `PAGE_SOURCE` ("coursekit-home") | `page_source` | appended to the form URL and to every CTA link |
| `gclid`, `fbclid` | optional fields | in `FORWARD_PARAMS`; add fields with the same Query Key to store them |
| CTA clicks | order form / funnel step | `LINKS.join` + UTMs + `page_source` appended when `UTM_TO_LINKS` is true |

## 11. Asset upload checklist

Upload to Media Library, then paste the URLs into the config:

- [ ] Wordmark (SVG / PNG, white) > `BRAND.final.logoSrc`
- [ ] Square icon > `BRAND.final.faviconSrc`
- [ ] Hero video: YouTube URL or MP4 > `VIDEO.final`
- [ ] 4 roadmap photos 16:9 (800x450) > `road-1..4`
- [ ] 3 win portraits (1450x816) > `win-1..3`
- [ ] Circle card photo (600x336) > `circle-photo`
- [ ] 9 testimonial headshots (88x88) > `t-*`
- [ ] 5 team portraits, transparent PNG, mono (290x290) > `team-1..5`
- [ ] 4 mockup avatars > `chat-1`, `chat-2`, `search-avatar`, `msg-avatar`
- [ ] Up to 8 client logos (white, transparent) > `LOGOS.final`
- [ ] Checkout / order form URL > `LINKS.join`

## 12. GHL setup

See [`GHL-SETUP.md`](./GHL-SETUP.md).

## 13. QA

| Check | Result |
|---|---|
| Section tops and heights vs Framer, 1440 | every section within 2px (hero +2, all others 0-1) |
| Same, 810 | every section height within 2px; tops drift up to 3px by the footer from sub-pixel rounding |
| Same, 390 | every section within 2px |
| Page height 1440 / 810 / 390 | 10524 / 10709 / 13743 vs Framer 10525 / 10713 / 13744 |
| Timeline dots | within 1px at all three widths |
| Line breaks | H1, hero body (balanced), headings and cards break where Framer's do |
| Horizontal overflow | 0 at 1440 / 810 / 390, both themes |
| Console errors / warnings | none, both themes, all widths |
| Framer class names in DOM | 0 |
| Interactions | nav dropdown, FAQ accordion, slider arrows and auto-advance, counters, tickers, video tilt: all pass |
| UTM passthrough | `?utm_source=fb&utm_campaign=launch&gclid=abc` > every CTA gets `...&page_source=coursekit-home`; test form iframe gets the same |
| Lighthouse mobile (final theme) | **Performance 98-100, Accessibility 100, Best Practices 100**, LCP 1.3s, TBT 40-170ms, **CLS 0** |
| Framer original, same run | Performance 71, Accessibility 89, Best Practices 96, LCP 4.8s, CLS 0.016 |
| Font swap | page height identical with fonts blocked at 1440; 21px taller at 390 |
| `!important` | only GHL overrides, each commented |

## 14. Deviations

| # | What | Why |
|---|---|---|
| 1 | All photos, the hero video and logos are rebuilt mockups (framer theme) | brief: no source images; slots take your assets |
| 2 | Light beams and gold arcs are SVG traced from Framer's PNGs, not the PNGs | keeps them themeable and dependency-free; geometry matches the originals |
| 3 | Phone feature cards keep their real titles (search, calendar, gamification, community, classroom) | Framer's phone variant repeats "All-in-one search" and the search description on several cards, a template bug. Desktop copy is used. |
| 4 | FAQ question 1 keeps Framer's answer (the refund answer) verbatim | copy rule; flagged so you can rewrite it |
| 5 | Phone module rows repeat the hours chip on a second line | Framer does this; kept for pixel parity (CSS `.fx-mdup`, delete the spans to drop it) |
| 6 | Copyright reads "CreatorKit" | Framer's text, kept verbatim |
| 7 | Features bento hidden at tablet | Framer hides it at 680-1199 too |
| 8 | "Made in Framer" badge and "Funnel Pages" switcher not rebuilt | Framer platform chrome, not page design |
| 9 | Final theme lifts 40% text to 55% | 40% on #141414 is 3.9:1; 55% passes AA. Framer theme keeps 40% |
| 10 | CTA ticker hidden below 1200 | Framer hides it too |
| 11 | Tablet section tops drift 1-3px by the footer | sub-pixel rounding across 13 sections; each section's own height is within 2px |
