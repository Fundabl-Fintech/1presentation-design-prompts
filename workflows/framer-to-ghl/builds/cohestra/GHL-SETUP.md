# GHL setup: Cohestra page

Total time: about 20 minutes, most of it uploading your assets.

## 1. Custom fields (Settings > Custom Fields)

Create a folder named **Attribution**, then add six **Single Line** text fields:

`utm_source`, `utm_medium`, `utm_campaign`, `utm_content`, `utm_term`, `page_source`

## 2. The form (Sites > Forms > Builder)

1. New form, name it **Starter Kit**.
2. Add **Email** (standard field). Label `Email`, placeholder `example@cohestra.com`, required.
3. Add the six Attribution fields as **Hidden** fields. For each one, set **Query Key** to the same name (`utm_source`, and so on).
4. Button text: `Send Me the Kit`.
5. **Styles:** background transparent, width 100%, padding 0, border none, shadow none, font Outfit.
6. **Styles > Custom CSS:** paste all of `ghl-form-custom.css`. Use the light variant at the bottom only if you set `MODE: "light"`.
7. **Options > On Submit:** Open URL = your thank-you page (put the same URL in `FX_CONFIG.REDIRECT_URL`), or show a message.
8. Save, then copy the **Form ID** (the last part of the form's share URL).

## 3. Automation (Automation > Workflows)

Trigger **Form Submitted: Starter Kit** > Add Tag `starter-kit` > Send Email (the kit) > Wait 1 day > nurture into "Join the Next Cohort".

## 4. The page (Sites > Funnels or Websites > Edit page)

1. Add a **Section**. In its settings:
   - Width: **Full Width**
   - Padding: 0 on all sides, Margin: 0
   - Background: none
   - **Advanced > Overflow: visible** (if the option exists). If GHL still wraps the element in something with `overflow: hidden`, the page un-clips those wrappers itself (`UNCLIP_WRAPPERS: true`) so the backgrounds reach the screen edges. If the page body itself clips, the sticky nav and founders stack switch to a JS fallback and log a note in the console.
2. Inside it, one **Row**: full width, padding 0, margin 0, no background.
3. One **Column**: width 100%, padding 0, margin 0, no background.
4. Add one **Custom JS/HTML (Code)** element and paste the whole `cohestra-ghl.html`.
5. Page **Settings > SEO**: title, description, and favicon. The build adds no tracking scripts.
6. Remove any default header or footer sections on that page. The build ships its own nav and footer.

## 5. Config (top of the pasted code, the only place you edit)

| What | Where |
|---|---|
| Theme | `const THEME = "final"` (`"framer"` shows the untouched Cohestra clone) |
| Palette | `.fx-page[data-theme="final"]` block: 7 primitives |
| Font | `--f-display` / `--f-body`, plus the Google Fonts URL in the boot script (`EDIT: font URL`) |
| Brand name + logo | `FX_CONFIG.BRAND.final` |
| Form | `FX_CONFIG.FORMS.kit.id` = your Form ID |
| Redirect | `FX_CONFIG.REDIRECT_URL` |
| Links | `FX_CONFIG.LINKS` (every button, menu, footer and article link) |
| Images / videos | `FX_CONFIG.IMAGES.final`, `FX_CONFIG.VIDEOS.final` |
| Sections on/off | `FX_CONFIG.SECTIONS` |
| Default mode | `FX_CONFIG.MODE` = `"dark"`, `"light"` or `"system"` |
| Motion speed | `FX_CONFIG.MOTION_SPEED` (1 = Framer timing) |
| Smooth scroll | `FX_CONFIG.SMOOTH_SCROLL.enabled` |
| Slot labels | `FX_CONFIG.SHOW_SLOT_LABELS` (turn off before going live) |

Copy is edited in place. Every text block has an `<!-- EDIT: ... -->` tag above it.

## 6. Test before launch

1. Preview at desktop, then with the phone toggle.
2. Open `yourpage.com/?utm_source=test&utm_campaign=launch` and submit a real email.
3. In Contacts, confirm the email and the `utm_source`, `utm_campaign` and `page_source` values.
4. Confirm the redirect or thank-you message and the workflow email.
5. Scroll the whole page. The founders stack should pin and the blog should drift up (parallax).

## 7. Save it as a reusable template

Pick one of these (or all three):

- **Save Section:** in the builder, open the section menu > **Save** (Global / Saved Section). Every future page can drop in the whole Cohestra page in one click.
- **Funnel template:** clone the funnel (Funnels > ... > Clone), or share it with **Share Funnel** to hand the page to another sub-account.
- **Snapshot:** Agency view > Account Snapshots > Create. Include the funnel/website, the **Starter Kit** form, the **Attribution** custom fields and the workflow. Load it into any sub-account to get the whole system, including tracking fields.
