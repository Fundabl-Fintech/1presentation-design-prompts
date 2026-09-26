# GHL setup: CourseKit page

Total time: about 20 minutes, most of it uploading your assets.

This is a sales page. It has no form: every "Join our community" / "Join Community" button sends the visitor to your offer (GHL order form, checkout or next funnel step) with their UTMs attached.

## 1. Custom fields (Settings > Custom Fields)

Create a folder named **Attribution**, then add six **Single Line** text fields:

`utm_source`, `utm_medium`, `utm_campaign`, `utm_content`, `utm_term`, `page_source`

On your **order form / checkout step**, add the same six as hidden fields with matching Query Keys. The page appends them to every CTA link, so they land on the contact at purchase.

## 2. The offer link

1. Build or open the funnel step people should land on (order form, checkout, application, booking).
2. Copy its live URL.
3. Paste it into `FX_CONFIG.LINKS.join`. Every CTA on the page (nav, hero, circle card, pricing, CTA section, mobile menu) uses it.

## 3. Optional: capture leads on the page

**3a. Build the form (Sites > Forms > Builder).** Add **Email** (required) and the six Attribution fields as **Hidden** fields, each with Query Key = its name. In **Styles**: background transparent, width 100%, padding 0, no border or shadow, font Inter. Paste `ghl-form-custom.css` into **Styles > Custom CSS**. Save, then copy the **Form ID**.

**3b. Place it on the page.** In the pasted code, add `<div class="fx-form" data-form="optin"></div>` where you want the form (the pricing card or CTA block works well), then set:

```js
FORMS: { optin: { id: "YOUR_FORM_ID", name: "Opt-in", height: 150 } },
```

The page loads GHL's native form with the visitor's UTMs and `page_source` pre-filled.

## 4. Automation (Automation > Workflows)

Trigger **Order Submitted** (or **Form Submitted: Opt-in**) > Add Tag `coursekit-buyer` > Send welcome email > Add to course / community.

## 5. The page (Sites > Funnels or Websites > Edit page)

1. Add a **Section**. In its settings:
   - Width: **Full Width**
   - Padding: 0 on all sides, Margin: 0
   - Background: none
   - **Advanced > Overflow: visible** (if the option exists). If GHL still wraps the element in `overflow: hidden`, the page un-clips those wrappers itself (`UNCLIP_WRAPPERS: true`) so the gold beams and full-bleed sections reach the screen edges.
2. Inside it, add one **Row**: full width, padding 0, margin 0, no background.
3. Add one **Column**: width 100%, padding 0, margin 0, no background.
4. Add one **Custom JS/HTML (Code)** element and paste the whole of `coursekit-ghl.html`.
5. In page **Settings > SEO**, set the title, description and favicon. The build adds no tracking scripts; add your pixel in the funnel's tracking code as usual.
6. Remove any default header or footer sections. The build ships its own fixed nav and footer.

## 6. Config (top of the pasted code, the only place you edit)

| What | Where |
|---|---|
| Theme | `const THEME = "final"` (`"framer"` shows the untouched CourseKit clone) |
| Palette | `.fx-page[data-theme="final"]` block: 11 primitives (ink, 2 panels, paper, 5 golds, mute, mock) |
| Fonts | `--f-display` / `--f-head`, plus the Google Fonts URL in the boot script (`EDIT: font URL`) |
| Brand name, logo, icon | `FX_CONFIG.BRAND.final` |
| Offer link | `FX_CONFIG.LINKS.join` (+ every nav, footer and social link in `LINKS`) |
| Hero video | `FX_CONFIG.VIDEO.final` (YouTube URL / ID or .mp4) |
| Images | `FX_CONFIG.IMAGES.final` (slot names in REPORT.md, section 6) |
| Client logos | `FX_CONFIG.LOGOS.final` (up to 8 URLs) |
| UTM passthrough | `FX_CONFIG.UTM_TO_LINKS`, `FORWARD_PARAMS`, `PAGE_SOURCE` |
| Sections on/off | `FX_CONFIG.SECTIONS` |
| Motion speed | `FX_CONFIG.MOTION_SPEED` (1 = Framer timing) |
| Slot labels | `FX_CONFIG.SHOW_SLOT_LABELS` (turn off before going live) |

You edit copy in place: every text block has an `<!-- EDIT: ... -->` tag above it.

## 7. Test before launch

1. Preview at desktop, then with the phone toggle. Open and close the mobile menu.
2. Open `yourpage.com/?utm_source=test&utm_campaign=launch` and hover a CTA. The link should end in `?utm_source=test&utm_campaign=launch&page_source=coursekit-home`.
3. Click through and complete a test order. In Contacts, confirm the `utm_source`, `utm_campaign` and `page_source` values.
4. Scroll the whole page:
   - The hero video tilts flat.
   - The counters run.
   - The wins slider auto-advances.
   - The FAQ rows open.
   - The CTA ticker scrolls.

## 8. Save it as a reusable template

- **Save Section:** in the builder, open the section menu and choose **Save**. The whole page becomes one reusable block.
- **Funnel template:** clone the funnel, or hand it to another sub-account with **Share Funnel**.
- **Snapshot:** go to Agency view > Account Snapshots > Create. Include:
  - the funnel
  - the order form
  - the **Attribution** custom fields
  - the workflow

  Every sub-account you load it into gets the full system, tracking fields included.
