# Woodlands Dental — ADA / WCAG 2.1 AA Audit Report & Fix Plan

**Site:** https://woodlands-dental.webflow.io/ (Webflow Site ID `6a9e607d5376ce9ae24f8030`)
**Audit date:** 2026-10-12
**Standard:** WCAG 2.1 Level A + AA (the technical standard courts and the DOJ ADA rule use)
**Scope:** 49 published pages rendered in Chromium at 1366px, plus targeted mobile (390px) and reflow (320px) checks. Covers the homepage, all 19 `/treatment/*` pages, 12 `/braces-treatment/*` CMS pages, the blog, FAQs, fees, plans, finance, referrals, contact and team pages.
**Status:** **Audit only. No changes were made to the site, theme, styles or content.**

> **Method.** WAVE (wave.webaim.org) is a browser-driven tool and could not be run from this environment. I used the axe-core 4.10 engine (the same class of rules WAVE uses), a custom crawl for structure, keyboard, focus and motion, and a read of the live markup. Treat the numbers as a close **estimate** of a WAVE run. Please confirm by running WAVE on the homepage plus 2–3 template pages (Treatment, Braces, Treatments) after the fixes. Automated tools find roughly 30% of accessibility issues, so the manual checklist at the end still applies.

---

## 1. Summary

| Category (WAVE-style) | Result |
|---|---|
| **Errors** (hard fails, WAVE sense) | **0 missing alt, 0 empty links, 0 empty buttons, 0 empty headings, 0 missing form labels (site forms are a JotForm iframe).** Two structural errors below (no `<main>`, broken ARIA list). |
| **Contrast errors** | **~10 per page on 22 of 49 pages** (about 228 failing elements, from about 7 distinct colour pairs). The reviews carousel, gold accent text and "After" tags are the repeat offenders. |
| **Alerts** | Heading level skips on 32 pages; 8 `href="#"` links per page; no skip link; small tap targets; hidden text on gradient hero needs manual contrast check. |
| **Features** | Alt text on all 3,816 images (679 correctly marked decorative `alt=""`), `lang="en"`, unique page titles, exactly one H1 per page, iframes have titles, social and WhatsApp links have `aria-label`, `prefers-reduced-motion` CSS exists, no horizontal scroll at 320px. |
| **Estimated AIM score (homepage)** | **about 6.5 / 10 (yellow, needs work)** using `10 − (errors×2 + contrast×0.3 + alerts×0.1)`. Most of the loss is contrast. Fixing contrast and the landmark issues should move it to about 9+. |

**Overall:** the site is in better shape than most. Content accessibility (alt text, titles, headings, language) is solid. The gaps are almost all **template-level**, meaning a handful of fixes in the Navbar, Footer, a CMS template, and a few colour values clear the great majority of issues across the whole site.

---

## 2. Findings

Severity: **Critical** (blocks a user) · **Serious** · **Moderate** · **Minor**. "Pages" counts are out of the 49 audited.

### A. Errors

| # | Issue | WCAG | Severity | Pages | Where / evidence |
|---|---|---|---|---|---|
| E1 | **No `<main>` landmark; no `<header>` or `<footer>` landmark.** The whole page is a `<div class="page-wrapper">`. The only landmarks are 10 `<nav>` elements (mostly dropdown lists, unlabelled). Screen-reader users cannot jump to main content, and axe flags about 2,350 elements as "not contained by landmarks". | 1.3.1, 2.4.1 (A) | Serious | 49 | Page structure: `DIV.page-wrapper > …`. Footer is `DIV.footer`. |
| E2 | **No skip-to-content link.** There are 5+ focusable items (address, phone, social links, then a 10-item nav with dropdowns) before the content. | 2.4.1 (A) | Serious | 49 | `a[href^="#"]` with "skip" text: 0 on every page. |
| E3 | **Invalid ARIA in the reviews carousel.** `role="list"` on `.swiper-wrapper` contains children with `role="group"`, which is not allowed. | 1.3.1, 4.1.2 (A) | Serious | 12 (all `/braces-treatment/*` CMS pages, one shared template) | `#swiper-wrapper-…` `aria-required-children`. |
| E4 | **Mobile menu button is an `<a href="#">` wrapping two images** (alt "Open menu" and "Close menu", both always in the DOM). It has no `aria-expanded`, no `aria-controls`, and is not a button. A screen-reader user hears both labels and cannot tell the open/closed state. | 4.1.2, 2.1.1 (A) | Serious | 49 | `div.hamburger > a[href="#"] > img.ham-icon + img.ham-close`. |
| E5 | **Social links are `href="#"`** (Instagram, TikTok, and one more) with no destination. They are focusable, announced as links, and go nowhere. Facebook is correct. | 2.4.4 (A), 4.1.2 | Moderate | 49 | `.short-nav-flex > .social-media-block > a[href="#"]`, repeated in the footer. |

### B. Contrast errors (WCAG 1.4.3, AA, 4.5:1 for text)

| # | Foreground on background | Ratio | Where | Pages |
|---|---|---|---|---|
| C1 | `#688b78` (sage green) on `#fffdf9` | **3.72:1** | Reviewer names and locations in the reviews carousel ("Sarah M. (North Harrow)", "— David R.") | 22 (shared component) |
| C2 | `#cfa555` (gold) on `#fffdf9` | **2.25:1** | "Enquire →" links in the Treatments hub cards | `/treatments` (about 13 cards) |
| C3 | `#fffdf9` (white) on `#cfa555` (gold), 10px bold | **2.25:1** | "After" tag on the smile gallery | `/smile-gallery` + home interactive gallery |
| C4 | `#c5a059` (gold) on `#f5efde` | **2.13:1** | "Most Popular", "Flexible" ribbons | `/patient-finance` |
| C5 | `#c5a059` on `#faf8f5` | **2.31:1** | "0% APR" | `/patient-finance` |
| C6 | **Needs manual check:** text over the hero image/gradient ("Book Now" nav button, hero tag, H1) | undetermined | Axe cannot compute contrast on gradients or images. Test by eye or with a contrast picker on the live hero. | Home, plus any page with the hero overlay |

### C. Alerts

| # | Issue | WCAG | Pages | Detail |
|---|---|---|---|---|
| A1 | **Heading level skips (H1 → H3).** The sidebar card title ("About Us", "General & Family Dentistry", "Cosmetic Dentistry"…) is an `<h3>` directly after the H1, with no H2. | 1.3.1, 2.4.6 | 32 | `.treatment-side-bar > h3.card-title` in the treatment/braces templates. The homepage pattern H1 → H2 → H3 is fine. |
| A2 | **No visible keyboard focus on main-menu items.** `Home` shows a focus ring, but `About Us`, `Treatments` and the dropdown toggles compute `outline: none`, and the site CSS has `.w-dropdown-toggle:focus { outline: 0 }` with no replacement. Keyboard users cannot see where they are. | 2.4.7 (AA) | 49 | Confirm visually by tabbing through the nav. |
| A3 | **Dropdown nav exposes an unresolved `aria-controls`.** `role="button"` on a `<div>` with `aria-controls="w-dropdown-list-N"`. The target does not always exist in the DOM when closed. Enter opens and Escape closes (works), but the pattern is Webflow's default. | 4.1.2 | 49 | Low risk; Webflow-generated. Verify with a screen reader. |
| A4 | **Auto-moving content has no pause control.** The reviews and hero carousels (Swiper, 6s transitions) have no pause/stop button and no visible controls. The hero did not advance in a 9s test, so confirm whether it auto-rotates. CSS honours `prefers-reduced-motion`, which helps. | 2.2.2 (A) | Home + carousel pages | Add a pause control or stop autoplay when the user hovers/focuses. |
| A5 | **Small tap targets.** Phone, address, footer links and "Enquire →" are about 18–20px tall (below the 24px minimum). | 2.5.8 (AA, WCAG 2.2) | 49 | Not a WCAG 2.1 failure but flagged by WAVE-style tools and recommended. |
| A6 | **Lightbox links all announce "open lightbox".** Three video tiles use `aria-label="open lightbox"`, so screen-reader users hear three identical labels. | 2.4.4, 4.1.2 | Home | Give each a unique label ("Play video: Dental Implants"). |
| A7 | **Repeated alt text on icons** ("Google Reviews logo" ×36, "Phone icon", "Location pin icon") adds screen-reader noise. | 1.1.1 | Most | Icons beside visible text should be `alt=""`. |
| A8 | **Links open in a new tab without warning** (4–5 per page: maps, WhatsApp, Facebook). | 3.2.5 (AAA), best practice | 49 | Add "(opens in new tab)" to the `aria-label`. |
| A9 | **Third-party JotForm iframe.** It has a proper title but its internals could not be tested. | 1.3.1, 3.3.2 | 48 | Test the form separately (labels, error messages, keyboard). |

### D. Features (passing)

- **Images:** 3,816 images checked, 0 missing `alt`; 679 are correctly decorative (`alt=""`).
- **Page language** `lang="en"` on every page; unique, descriptive `<title>` on every page.
- **Exactly one H1 per page.**
- **Iframes** (form, map) have descriptive `title` attributes.
- **No** positive `tabindex`, no vague "click here" links, no duplicate IDs (sampled), no text-only autoplay video found.
- **Social/WhatsApp icon links** have `aria-label`s.
- **Reflow:** no horizontal scroll at 320px or 390px.
- **`prefers-reduced-motion`** media query is present.

### E. Structure and ARIA (what exists)

- **Landmarks:** 10 `<nav>`, 0 `<header>`, 0 `<main>`, 0 `<footer>`.
- **Headings (home):** H1 → H2 → H3 order is clean. Footer has no headings.
- **ARIA:** Webflow dropdown buttons (`role="button"`, `aria-expanded`, `aria-haspopup`), Swiper `aria-live="off"`, and the invalid `role="list"` / `role="group"` mix in E3.

---

## 3. Fix plan

The plan is ordered by impact. **Nothing below has been applied.** Items marked 🎨 touch visual styling, so they need your sign-off first. The rest are invisible markup/semantic changes.

### Phase 1: Invisible structural fixes (no visual change). ~1–2 hrs

| Fix | Resolves | How (Webflow) | Risk |
|---|---|---|---|
| **1. Add landmarks.** Change the Navbar wrapper's tag to `header`, wrap the page body section in `main` (or set the tag of the content wrapper to `main`), and change the footer div's tag to `footer`. | E1 | Select the element, then Settings panel (gear) → **Tag** dropdown. Do it once per shared Component (Navbar, Footer). For `main`, add it once in each page/template, since there is one per page. In the Designer this can be done via `set_tag` on the element. | Low. No styling change because tag only. |
| **2. Add a skip link.** First element in the Navbar component: a Link Block "Skip to main content" pointing to `#main-content`, plus `id="main-content"` on the `<main>`. Hide it off-screen until focused (small custom CSS in the existing site embed). | E2 | Add Link Block → Link Settings → Element ID. Needs the Designer (no element-creation API). | Low. A tiny CSS block is the only styling. 🎨 |
| **3. Fix the mobile menu button.** Change `a[href="#"]` to a real `<button>` (or add `role="button"`, `aria-expanded`, `aria-label="Open menu"`) and make the two images `alt=""`. | E4 | Webflow's native Navbar element does this automatically. This site uses a custom hamburger with an interaction. Switching the tag to `button` and adding `aria-label` + `aria-expanded` via the Settings panel's Custom attributes is the lightest route. | Medium. Interaction triggers must be re-tested on mobile. |
| **4. Fix the reviews carousel ARIA.** Remove `role="list"` from `.swiper-wrapper` (or remove `role="group"` from slides). | E3 | Settings → Custom attributes on the Collection List wrapper in the Braces template. | Low |
| **5. Fix the heading skip.** Change the sidebar `.card-title` from H3 to H2 (or retag to a `<p>` styled the same). | A1 | `set_heading_level` is semantic-only, so there is zero visual change since the class controls size. The fix lives in the sidebar Component and clears all 32 pages. | Low |
| **6. Give social links real URLs** (or remove them if the accounts do not exist). Currently Instagram/TikTok are `#`. | E5 | Needs real URLs from the client. | Low |
| **7. Unique lightbox labels, `alt=""` on decorative icons, "(opens in new tab)" in aria-labels.** | A6, A7, A8 | Settings → Custom attributes / Image alt. | Low |

### Phase 2: Contrast and focus (touches colours 🎨). Needs your approval

I will not change the theme or palette on my own. These are the smallest possible tweaks that reach 4.5:1, so please approve or substitute your own brand-approved values.

| Current | Suggested | New ratio | Used for |
|---|---|---|---|
| `#688b78` on `#fffdf9` (3.72) | **`#4f705f`** | 5.42:1 | Reviewer names / locations (C1) |
| `#cfa555` text on `#fffdf9` (2.25) | **`#8a6a1f`** | 4.97:1 | "Enquire →" text links (C2) |
| `#c5a059` text on `#f5efde` / `#faf8f5` (2.1–2.3) | **`#7d5f17`** | 5.19–5.63:1 | Finance ribbons, "0% APR" (C4, C5) |
| White `#fffdf9` on gold `#cfa555` (2.25) | **dark text `#101010`** (or darken the gold background) | 8.31:1 | "After" tag (C3) |
| Focus outline: none on nav items | **2–3px outline in a contrasting colour, 2px offset** on `:focus-visible` | n/a | Nav links and dropdown toggles (A2) |

Webflow note: do the colour edits on the **consuming classes** (e.g. the reviewer-name class), not the global colour variable. The variable tool is unreliable, and a variable change would also recolour decorative uses of the gold. The gold is fine for non-text decoration (borders, icons ≥ 3:1 on background).

### Phase 3: Behaviour and verification

| Fix | Resolves | Notes |
|---|---|---|
| Add a pause/play button to the hero and reviews carousels, or disable autoplay. | A4 | Needs a design decision. |
| Manually check the hero text over imagery (C6). Add a darker gradient scrim if below 4.5:1. | C6 | 🎨 Visual change, so please approve. |
| Increase tap-target padding on footer/contact links to ≥ 24px. | A5 | 🎨 Small spacing tweak. |
| Test the JotForm embed (labels, errors, keyboard) in JotForm's own accessibility settings. | A9 | Third-party. |
| Re-run WAVE and axe on the homepage + 3 templates after publishing, and verify via `curl` that the new tags/attributes appear in the live HTML. | All | Confirms fixes actually published (attribute writes on Component-nested elements can silently fail to publish). |

### Expected effect

| After | Errors | Contrast errors | Est. AIM |
|---|---|---|---|
| Phase 1 | 0 | ~10 | ~7 |
| Phase 1 + 2 | 0 | 0 on tested text (hero still to verify) | **~9.5–10** |

---

## 4. Manual checks still required (cannot be automated)

1. Full keyboard walkthrough: Tab order, no keyboard traps, dropdown + mobile menu operable.
2. Screen reader pass (NVDA / VoiceOver) on the nav, forms, accordions (FAQs) and carousels.
3. Alt text *quality* on before/after gallery images and team photos.
4. Contrast on hero images, gradients and hover/focus states.
5. 200% zoom and text-spacing overrides (1.4.4, 1.4.12).
6. Form error messages and instructions (JotForm).
7. Video captions and transcripts for the three "Video" lightbox tiles (1.2.x).
8. Link purpose in context (e.g. repeated "Enquire →").
9. Accessibility statement and contact route (recommended).

## 5. Notes and caveats

- This is the `webflow.io` staging domain (`Woodlands Dental` has no custom domain yet). Re-audit the production domain at launch, since custom code and scripts can differ.
- The draft/CMS templates (`detail_blogs`, `detail_braces-treatment`, `detail_faqs`, etc.) are the source for the CMS pages. Fixing the template fixes every item.
- Only 2 blog posts were reachable by link from the site; unpublished or unlinked CMS items were not audited.
- Per the `webflow-ada-wcag-fixes` skill, a handoff file would normally be saved to `C:\Users\reala\OneDrive\Desktop\remedo\{Client}\ADA-WCAG\`. That path is not reachable from this cloud session, so this report lives in the repo. No fixes have been applied, so no handoff is needed yet.

## 6. Key IDs

- Site ID: `6a9e607d5376ce9ae24f8030`
- Home page ID: `6ab4a962b8a4162159d0c7a2`
- Last published: 2026-10-12T04:22:32Z (webflow.io subdomain only)
- Crawl evidence and raw axe output are reproducible with the scripts in the session scratchpad.
