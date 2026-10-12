# Fame Dentistry — ADA / WCAG 2.1 AA Audit & Remediation Plan

**Site:** fame-dentistry.webflow.io (Webflow Site ID `6a19a7f035bfcfdf2567bbcd`, live domains `famedentistry.co.uk` / `www.famedentistry.co.uk`)
**Audit date:** 2026-10-12
**Standard:** WCAG 2.1 Level A + AA (ADA Title II/III technical standard)
**Status:** Report and plan only. **No changes were made to the site.**

---

## 00. UPDATE: WAVE results for the homepage (supersedes my estimates below)

WAVE on `https://fame-dentistry.webflow.io/` (screenshots supplied by the client team): **5 Errors · 31 Contrast Errors · 2 Alerts · 10 Features · 5 Structure · 7 ARIA · AIM 3.7/10.** (WAVE's AIM score is its own calculation; it does not match the simple formula in the audit skill, so use WAVE's figure.)

| WAVE result | Verdict vs. my static audit |
|---|---|
| **4 × Missing form label** (name, email, phone, message in the enquiry form) | **Confirmed (E1).** Top priority. |
| **1 × Missing alternative text** (icon beside the "Select Treatment" dropdown) | **Confirmed in part (E3).** On Home it is the dropdown icon only. The tick/address/phone icons I listed were not flagged here (hidden success state or not on this page); re-check on other pages. |
| **31 × Very low contrast** | **I underestimated this badly** (I guessed 3–8). It is the largest issue. Visible in the screenshots: gold text on cream — "NOW OPEN · SCOTLAND STREET · GLASGOW", "REPLY FROM A REAL PERSON", the footer headings ADDRESS / NEED CARE NOW? / FOR DENTISTS / CONTACT, and the credential/pull-quote text. This is my **C5**, now confirmed and widespread. |
| **2 × Alerts** (one is "Redundant link") | Redundant link matches **A7**. My other alerts (no landmarks, form status, etc.) were not flagged by WAVE; they remain best-practice gaps, not WAVE alerts. |
| **7 × ARIA** (e.g. `aria-current="page"`) | **My statement "no ARIA found" was wrong.** Webflow adds `aria-current` automatically; my queries only looked for `aria-label`/`role`/`tabindex`. |
| `lang="en"` detected, logo alt present, h1/h2 present | Good (Features). |

**Corrections to my earlier estimates**
- Errors: I estimated 10–16; WAVE found **5**. Items **E2, E4, E5, E6, E7** (footer newsletter label, empty mobile CTA, social links, nav links, skip-link target) were **not flagged on Home**. The Home page uses its own navbar and the older `Footer` component, not `nav-bar-new` / `Footer new`, so those items are **unverified** and only apply to the ~20 pages that use the newer components. Treat them as "check on those pages".
- Contrast: **31, not 3–8.** The count is many instances of a few classes, so a small number of style changes should clear most of it.
- The old `Footer` component (1 instance, used on Home) was **not** in my audit; its gold-on-cream headings are among the contrast failures.
- One group of contrast markers sits on every word of a navy pull-quote ("…of our implant patients and the advanced dental work we are referred to us by other dentists."). That pattern suggests a word-by-word reveal animation (partial opacity) is being read as low contrast. Verify with the WAVE **Contrast** tab before changing colours.

**Gold text contrast (computed).** Brand gold `#C9A96E` is **2.07:1 on cream (#F8F6F2) and 2.24:1 on white**; it needs 4.5:1 for body-size text. Gold is fine on navy (7.1:1) and as a decorative border. Candidate darker text-only gold: `#8A6D2F` = 4.51:1 on cream / 4.87:1 on white (borderline), `#85692A` = 4.8:1 / 5.18:1 (comfortable), `#7A5F24` = 5.57:1 / 6.02:1. Recommended approach: add one darker "gold-text" shade used **only for text on light backgrounds**, and leave the existing gold for borders, buttons and text on navy. This is a colour change and **needs your approval** (Phase 3).

**Revised priority order**
1. **Contrast (31):** gold text on light backgrounds — highest count, few classes to change, needs colour approval.
2. **Form labels (4 on Home; same component on 19 pages)** and the dropdown icon alt text.
3. Redundant link on Home.
4. Then the "unverified" component items on the ~20 pages using `nav-bar-new` / `Footer new`, and the best-practice gaps (landmarks, live-region form status, email `type`/autocomplete).

---

## 0000. HOME-V2 (`/home-v2`, page `6a8aea0bef4915729a2ec633`) — scope changed 2026-10-12: current Home is out of scope

Live scan of `https://www.famedentistry.co.uk/home-v2` (axe-core + WAVE-style DOM checks): lang OK, 1 h1, no heading skips, no missing alt, no empty headings, skip link target exists (`#main-content`).

| # | Issue | Where | Status |
|---|---|---|---|
| 1 | Close button has no accessible name (`a.cross-btn`, empty link) | page-level | **Fixed, saved, not published**: `aria-label="Close"` |
| 2 | Lightbox link name does not contain its visible text ("Richard's story · Watch") | page-level | **Fixed, saved, not published**: `aria-label="Richard's story · Watch"` (axe flagged only this lightbox; Webflow's lightbox script may override it, so re-check live) |
| 3 | 4 enquiry-form fields + footer email field have no label | **shared components** `Enquiry Form` (19 pages), `Footer new` (20 pages) | Not changed (outside Home-v2 scope; needs your OK) |
| 4 | Duplicate id `Email` (enquiry form and footer form) | shared components | Not changed, same reason |
| 5 | No `<main>`, `<header>`, `<footer>` landmarks. `#main-content` is only the hero div, so retagging it `main` would be wrong | page structure + shared components | Not changed: needs content wrapped in a `main` (Designer) |
| 6 | Colour contrast, 10 axe nodes: gold `#BD9242`/`#C9A96E` on white, `#616670` on navy (`.text-light`, `.case`), `#7D5E17` on navy (`.people-tag`) | **global classes** | Not changed: edits would apply sitewide and need colour approval |

**Shared-component fixes (approved 2026-10-12, saved in Designer, NOT yet published):**
- `Enquiry Form` component (`22531e8b-e222-bfa7-c9b2-747b16cdefb8`, 19 pages): `aria-label` + `autocomplete` on Name, Email, Phone; `aria-label` on Message.
- `Footer new` component (`275904d5-ad88-c63b-78e0-299d97ff71bd`, 20 pages): newsletter email input `aria-label="Email address for newsletter"`, `autocomplete="email"`, and DOM id changed `Email` → `newsletter-email` (removes the duplicate id with the enquiry form).
- Read-back via the Webflow MCP confirmed the values. **Webflow can silently drop attributes on component-nested elements at publish time, so these must be verified on the live HTML after publishing.**

**Contrast fixes (approved 2026-10-12, saved in Designer, NOT yet published)** — colour changes only:

| Style | Before | After | Ratio | Scope |
|---|---|---|---|---|
| `text-light` (grey caption on navy) | `rgba(248,246,242,.32)` ≈ `#616670` | `#C2C7D1` | 2.75 → 9.4:1 | global; only ever on navy |
| `Testimonial-type-block` | `#C9A96E` on white | `#856421` | 2.23 → 5.5:1 | global; only on white |
| `Text Block 5` ("Video Testimonial") | gold variable `#BD9242` on white | `#856421` | 2.85 → 5.5:1 | one element |
| `Form-label` | `#737373` on cream | `#666666` | 4.39 → 5.4:1 | global |
| `button-primary:hover` background | `#C9A96E` (white text) | `#8A6824` (white text) | 2.24 → 5.14:1 | global (option B) |
| NPS Rating tag (`People-tag`) | `#7D5E17` on navy | new combo `People-tag.tag-on-navy` = `#C9A96E` | 2.63 → 7.1:1 | that one element only; `People-tag` unchanged elsewhere (it passes on light backgrounds) |

**Deliberately not changed:** `section-meta-3/4`. They only fail on the current Home (out of scope), and `section-meta-4` is also used on navy, where a darker bronze would fail. Fix needs a combo class on that page's elements.

### Home-v2 WAVE run (webflow.io/home-v2): 9 errors · 363 contrast · 15 alerts · AIM 1.6 — follow-up fixes (saved, NOT yet published)

**Root cause of the 363 contrast errors:** every letter of the small gold uppercase labels is its own `span.char` inside `.typewriter` (letter-by-letter reveal), so WAVE counts one error per letter. The colour comes from the global `.typewriter` style (gold `#BD9242`, 2.2–2.65:1 on the cream/beige backgrounds).

| Fix | Detail |
|---|---|
| `typewriter` colour → `#7A5B1D` | 4.8–6.3:1 on every beige/white background it sits on (darkest `#E8E0D0` = 4.79:1) |
| New combo `typewriter.typewriter-on-dark` = `#C9A96E` | applied to the only navy-background label ("The enquiry process", Concierge page, element `ccd9ccd4-5d7e-ef83-15c5-dadc2ba60e6d`) so it stays gold (7.1:1) |
| Very small text (6 alerts) | `text-light` and `Testimonial-type-block` font-size 10px → 12px |
| Long alt text (1 alert) | hero image alt shortened from 125 characters to "Glasgow skyline at sunrise over the River Clyde" |
| Suspicious link text (1 alert) | the two "Read more →" links on the founder cards now have `aria-label` "Read more about Dr Ferhan Ahmed" / "…Dr Tariq Bashir" |

**Left alone:** Noscript element (the Google Tag Manager noscript iframe, standard and harmless); Redundant links (6): these are header/footer links that repeat body links to the same pages (nav/footer components, intentional); and the 9 "missing alt" errors are still under investigation (live DOM shows `alt=""` on 17 decorative images; WAVE may be treating some as needing text).

## 000. FIX LOG (2026-10-12) — saved in the Designer, **NOT yet published**

Home page (`6a19a7f335bfcfdf2567bbd4`) only. No visual change by design. Read back via the Webflow MCP after each write (API read-back only; **not yet verified on the live HTML or by re-running WAVE**).

| # | WAVE item | Fix applied | Read-back |
|---|---|---|---|
| 1 | Missing form label ×4 | Added `aria-label` to Name ("Your name"), Email ("Email address"), Phone ("Phone number"), Message ("Anything else? (optional)") — each equals the visible label text. Also added `autocomplete` = `name` / `email` / `tel` (WCAG 1.3.5). | ✅ attributes present on all 4 elements |
| 2 | Missing alternative text ×1 | Dropdown arrow `<img class="arrow">` in the HTML embed (`43a85220-9feb-e333-8f55-853507131545`) now has `alt=""` and `aria-hidden="true"`. No other embed code changed. | ✅ embed code read back with the change |

**Not changed / still open**
- Redundant link (WAVE alert): two `For Dentists` links to the same page (`7dbe0a6b…`, `6aaf3856…`). Probably desktop and mobile copies; removing one could break a breakpoint, so left for a decision.
- **Custom "Select Treatment" dropdown is mouse-only** (div-based, click handlers only, no `tabindex`/role/keyboard handling) — a real WCAG 2.1.1 keyboard failure WAVE does not flag. Fixing it means editing the embed JavaScript; not done yet.
- Same fixes still to do in the shared `Enquiry Form` component (19 pages) and the dropdown embed copy there.
- Contrast (31 errors): awaiting colour approval.
- Embed CSS contains an invalid rule (`background: #rgb(231 224 212)` on `.select-list:hover`), so the hover colour never applies. Not an accessibility error; noted only.
- **Publishing:** pending. A site publish also pushes any other unpublished edits.

### Live verification (2026-10-12, after publish) — `https://www.famedentistry.co.uk/`

Published at 10:37 UTC to the webflow.io subdomain and both custom domains. The live HTML was fetched and scanned with axe-core (WCAG 2.0/2.1 A+AA rules) in headless Chromium. (WAVE itself was not re-run by me.)

| Check | Live result |
|---|---|
| 4 form fields | ✅ `aria-label` "Your name" / "Email address" / "Phone number" / "Anything else? (optional)" and `autocomplete` name/email/tel are in the live markup |
| Dropdown arrow image | ✅ live `<img class="arrow" … alt="" aria-hidden="true">` |
| axe: label / alt / ARIA / link / heading / landmark rules | ✅ **0 violations** |
| axe: colour contrast | ❌ **5 nodes**, all `#bd9242` gold text on `#f8f6f2` cream = **2.64:1** (needs 4.5:1): `.section-meta-3` and the four `.section-meta-4` footer headings |

**Correction to my earlier contrast numbers:** the gold text on cream is actually `#BD9242` (2.65:1), not `#C9A96E`. Darker text-only candidates against cream `#F8F6F2`: `#8A6824` = 4.76:1, `#856421` = 5.06:1, `#7F5F1F` = 5.47:1 (all pass 4.5:1). axe counts 5 nodes where WAVE counts 31, because axe skips elements it cannot measure reliably (e.g. animated or overlapping text). The contrast classes to change are `section-meta-3` / `section-meta-4` and other uses of `#BD9242` text on light backgrounds.

---

## 0. How this audit was done (read first)

| Item | Detail |
|---|---|
| Live HTML / WAVE | **Not run.** The sandbox's egress policy blocks `*.webflow.io` (CONNECT 403), so I could not fetch the rendered page or run WAVE. |
| What was actually inspected | Read-only Webflow MCP: page list, **Home page element tree**, the three shared Components (`nav-bar-new` ×22 pages, `Footer new` ×20, `Enquiry Form` ×19), all 46 forms, and the CSS for key classes. |
| Contrast figures | Computed from Designer style values. Backgrounds are **assumed** (brand navy `#1A2233`) where the style didn't state one; those rows are marked "verify". |
| Not yet inspected | Element-level detail of the other 26 pages and CMS templates (Blogs, Blog Treatments, FAQs). They reuse the same shared Components, so most sitewide findings below will repeat there. |
| Confidence key | **C** = confirmed in Designer data · **L** = likely, needs WAVE/browser confirmation · **M** = manual test required |

Because this is static analysis, I have **not** computed an AIM score — it would be invented. Please run WAVE (https://wave.webaim.org/report#/https://fame-dentistry.webflow.io/) and compare against the "Expected WAVE item" column in section 2; each finding is written so you can confirm it in under a minute.

**Estimated shape of the WAVE result for the homepage:** Errors ≈ 10–16, Contrast Errors ≈ 3–8, Alerts ≈ 6–12. With that many errors the AIM score will be low (<5) until the form-label and landmark fixes ship. Most of those errors come from **three shared Components**, so the fix effort is far smaller than the count suggests.

---

## 1. Executive summary

1. **Forms are the biggest legal exposure.** In the `Enquiry Form` Component (live on 19 pages incl. the homepage), every field "label" is a plain `<div>`, not a `<label>`. None of the 5 inputs (name, email, phone, message, interest dropdown) has a programmatic label. The footer newsletter input (20 pages) has no label at all.
2. **No semantic landmarks.** The homepage and all three shared Components are built from `<div>`s — no `<main>`, `<nav>`, `<header>`, `<footer>`. The existing "Skip to main content" link (good) very likely points at nothing.
3. **Images without alt** (L): success-message tick icon (×2 per form), address/phone/footer icons, in components used on ~20 pages.
4. **Empty links/controls**: mobile CTA link in the nav has no text; social links in the footer have no destination set; hamburger is an animation with no accessible name (L).
5. **Contrast**: footer secondary text, input borders, and the button hover state fall below WCAG minimums. These fixes touch brand colours, so they need your sign-off (see section 4).
6. **Housekeeping**: a published page titled `test`, plus duplicate/parallel pages (`/home-v2`, `/for-dentists` vs `/for-dentist`).

---

## 2. Findings

### 2A. ERRORS (definite WCAG failures)

| # | Finding | WCAG | Where | Conf. | Expected WAVE item | Scope |
|---|---|---|---|---|---|---|
| E1 | **Form inputs have no associated `<label>`** — label text sits in `div.form-field-label` blocks. Inputs: `Name`, `Email`, `Phone-num`, `Message`, and the "What brings you to FAME?" dropdown (HTML Embed). | 1.3.1, 3.3.2, 4.1.2 (A) | `Enquiry Form` Component `22531e8b-e222-bfa7-c9b2-747b16cdefb8` | C | "Missing form label" ×4–5 | 19 pages |
| E2 | **Footer newsletter email input has no label and no placeholder** on some instances (form "Email Form 4": placeholder `null`). | 1.3.1, 3.3.2 (A) | `Footer new` `275904d5-ad88-c63b-78e0-299d97ff71bd` (input `…721e`) | C | "Missing form label" ×1 | 20 pages |
| E3 | **Images with no alt text set**: success-message tick (`Image 2`, `Image 3`, asset `6a2c0a610353c1a4b6d8bfd9`, ×2 per form incl. homepage), address & phone icons in Enquiry Form (`…efce`, `…efd2`), phone icon in footer (`…ca58f05`). | 1.1.1 (A) | Enquiry Form + Footer new + Home | L (alt field empty in Designer; confirm rendered `alt`) | "Missing alternative text" ×4–6 | ~20 pages |
| E4 | **Empty link**: mobile CTA `btn-gradient-wrap 13 / nav-btn-mobile` contains only an empty div — no text, no aria-label. | 2.4.4, 4.1.2 (A) | `nav-bar-new` `3481175f-4a6c-1fb4-42bf-abde466d5830`, el. `…5875` | C | "Empty link" | 22 pages (visible on mobile) |
| E5 | **Footer social links (Facebook/Instagram/LinkedIn) have no link destination** in Designer; they are image-only links. Either they render as dead `<a>` or the href is set another way. | 2.4.4 (A) | `Footer new`, els. `…7227`, `…7229`, `…722b` | L | "Linked image" / possible "Empty link" | 20 pages |
| E6 | **Nav links with no visible text/destination in the tree**: three `nav-link 2` items (`…583a`, `…586b`, `…5872`) and homepage `text-link` links (`373744ec…`, `4bbb7440…2b01`) show no children and no link in settings. Likely text/href supplied through Component props, but must be verified. | 2.4.4 (A) | `nav-bar-new`, Home | L | "Empty link" ×3–5 | 22 pages |
| E7 | **Skip link target likely missing.** `nav-skip-link` exists but there is no `<main>` and no element with an id matching "main". | 2.4.1 (A) | `nav-bar-new` el. `…c0792381`; Home | L | "Broken skip link" | 22 pages |
| E8 | **Page title "test"** on a published page (`/test`, id `6aa20f90a648ed23565cb10b`). | 2.4.2 (A) | Page | C | "Missing or uninformative page title" | 1 page |

### 2B. CONTRAST ERRORS (computed; backgrounds assumed unless noted)

| # | Element | Foreground / Background | Ratio | Needs | Conf. |
|---|---|---|---|---|---|
| C1 | `.t-light` (14px) and `.text-light` (10px) — cream at 32% opacity | `rgba(248,246,242,.32)` on navy `#1A2233` | **≈2.75:1** | 4.5:1 | L (verify bg) |
| C2 | **Button hover** — `.button-primary:hover` white text on gold | `#fff` on `#c9a96e` | **2.24:1** | 4.5:1 | C |
| C3 | **Form input borders** (`.vs-enq-input`) | gold 35% alpha on white | **≈1.3:1** | 3:1 (1.4.11) | C |
| C4 | Footer sub-text `.copyright-subtext` (16px) | cream 50% on navy | ≈4.7:1 (passes narrowly) | 4.5:1 | L — fails if the real bg is lighter |
| C5 | Gold text on white anywhere (e.g. eyebrows using the colour variable) | `#c9a96e` on `#fff` | 2.24:1 | 4.5:1 | M — locate usages |
| C6 | Optional-label text (12px, inherits label colour) | `#1A2233` | passes colour; **12px is very small** | — | C |

Passing, for reference: placeholder `#4f4b44` on white = 8.7:1; navy on gold = 7.1:1; gold on navy = 7.1:1; button default state white on navy.

### 2C. ALERTS (human judgement)

| # | Finding | WCAG | Notes |
|---|---|---|---|
| A1 | **No landmark regions** — Body → div → divs. No `<header>`, `<nav>`, `<main>`, `<footer>` on Home or in the Components (`#site-header` is a div). | 1.3.1, 2.4.1 | Biggest structural gap |
| A2 | **Form headings are not headings** — "Begin Your Enquiry" is a styled `div`; footer column titles ("Practice", "Treatments"…) are `div`s. | 1.3.1, 2.4.6 | "Possible heading" |
| A3 | **Email field uses `type="text"`** in the Enquiry Form (phone correctly `tel`). No `autocomplete` tokens anywhere. | 1.3.5 (AA), 3.3.2 | Footer email is correctly `type="email"` |
| A4 | **Error/success messages have no live region** (`#error_message`, Webflow success/error blocks). Screen-reader users aren't told the form result. | 3.3.1, 4.1.3 | Add `role="alert"` / `aria-live` |
| A5 | **Dropdown toggles** ("About", "Treatments") contain a bare icon; verify keyboard and screen-reader behaviour. Hamburger is an `Animation` element (`open-menu`) with no accessible name. | 4.1.2, 2.1.1 | Test manually |
| A6 | **Typewriter text and slide-class images** on Home — if auto-animating >5s there must be a pause control. | 2.2.2 | Manual |
| A7 | Duplicate adjacent links: two `Link Block 3` links to the same page (`/for-dentists`) in Home. | 2.4.4 | Merge or differentiate |
| A8 | **Redundant/duplicate pages**: `/home-v2` (published), `/for-dentists` vs `/for-dentist`, draft `/cosmetic-dentistry`. | — | Content hygiene; thin/duplicate pages hurt UX and SEO |
| A9 | Logo alt text is long and repeated in header and footer (`FAME Dentistry - dental and implant practice in Glasgow`); two different logo alts on Home (`FAME Dentistry logo`). | 1.1.1 | Shorten to "FAME Dentistry" |

### 2D. FEATURES (working — keep)

- **Skip link exists** and is hidden-until-focus (`left:-9999px` → `16px` on `:focus`).
- **Exactly one `<h1>`** on Home, h2s follow (no level skips found in page-level headings).
- Most content images have **descriptive alt text** (founders, reception, logo, social icons).
- **Footer links use descriptive text**; phone is a real `tel:` link; address links to Maps.
- Checkbox uses a **wrapping inline label** (correct pattern) and links the Privacy Policy.
- **Unique, descriptive page titles and meta descriptions** on all real pages.
- Phone input uses `type="tel"`; required fields flagged; no positive `tabindex` found; no `outline:none` rules found in the styles.

### 2E. STRUCTURE (Home)

H1 ×1 (`hero-title`), H2 ×2 page-level + H2 inside Enquiry Form. Landmarks: **none**. Lists: not used in nav/footer (links are in divs) — consider `<ul>` for nav.

### 2F. ARIA

No `aria-*` or `role` attributes found on Home (queries for `aria-label`, `role`, `tabindex` returned 0). Missing: names on icon-only controls, live regions for form status, `aria-expanded`/menu semantics on the hamburger (Webflow's native dropdown supplies its own).

### 2G. MANUAL CHECKS STILL REQUIRED

1.4.4 text resize 200% · 1.4.10 reflow at 400% · 1.4.12 text spacing · 1.4.13 hover/focus content · 2.4.3 focus order · **2.4.7 focus visible** (no `:focus-visible` styles found on `button-primary` or `nav-fame` — likely relying on browser default over dark/gold backgrounds) · 1.4.1 colour-only meaning · 3.3.3/3.3.4 error handling on the form · 2.2.2 motion · 1.2.x if any video/audio is added.

---

## 3. Remediation plan

Guiding rule from the brief: **no theme-level changes on my own.** Every fix is tagged:

- 🟢 **Zero visual change** — markup/attributes only. Safe to do.
- 🟡 **Minor visual change** — colour/size tweak. **Needs your approval before I touch it.**
- 🔵 **Designer-only** — the Webflow MCP cannot do this (no element creation; attributes on Component-nested elements don't reliably publish). Someone must do it in the Designer.

### Phase 1 — Shared Components (highest leverage, ~20 pages each)

| Step | Fix | Type | Detail |
|---|---|---|---|
| 1.1 | **Enquiry Form labels (E1)** | 🔵🟢 | In Component edit mode, replace each `div.form-field-label` with a Webflow **Form Label** element (or add `aria-label` custom attributes to each input as the fastest interim). Keep the existing classes so appearance is unchanged. For the dropdown embed, add `<label for=…>` / `aria-label` inside the embed code. |
| 1.2 | **Footer email label (E2)** | 🔵🟢 | Add `aria-label="Email address for newsletter"` on the input; set a placeholder. |
| 1.3 | **Alt text (E3)** | 🟢 | Decorative icons (tick, address, phone): mark **Decorative** so `alt=""`. Can be set on Component instances' elements via the Designer; the MCP reports success but verify with live HTML. |
| 1.4 | **Empty mobile CTA (E4)** | 🔵🟢 | Add `aria-label="Enquire now"` to link `…5875` (or put text in the child div). |
| 1.5 | **Social links (E5)** | 🔵🟢 | Set real URLs on the three links and add `aria-label`s; if the profiles don't exist yet, remove the links. **Needs URLs from you.** |
| 1.6 | **Landmarks (A1, E7)** | 🔵🟢 | Change root div tags: `nav-bar-new` root → `<header>` with inner `<nav aria-label="Primary">`; `Footer new` root → `<footer>`; wrap page content in `<main id="main-content">` and point the skip link `href` at `#main-content`. Tag changes don't alter styling. |
| 1.7 | **Form status (A4)** | 🔵🟢 | Add `role="alert"` (error) and `role="status"` (success) to the message blocks, plus `aria-live`. |
| 1.8 | **Email type + autocomplete (A3)** | 🟢 | Set Email input type to `email`; add `autocomplete` = `name`, `email`, `tel`. |
| 1.9 | **Headings (A2)** | 🟢 | Retag "Begin Your Enquiry" and footer column titles to h2/h3/`<p>` as appropriate. Because retagging keeps classes, appearance doesn't change; check there is no heading-level skip afterwards. |

### Phase 2 — Homepage & page-level

| Step | Fix | Type |
|---|---|---|
| 2.1 | Resolve/verify unlabeled `text-link` and unstyled links (E6): confirm the rendered text/href and fix. | 🟢 |
| 2.2 | Merge or differentiate the duplicate `/for-dentists` links (A7). | 🟢 |
| 2.3 | Shorten and standardise logo alt (A9). | 🟢 |
| 2.4 | Decide on the `/test` page: unpublish or retitle (E8); decide on `/home-v2` and `/for-dentists` vs `/for-dentist` (A8). **Your call — content decision.** | 🟢 |
| 2.5 | Give hamburger (`open-menu` Animation) an accessible name / button wrapper (A5). | 🔵 |

### Phase 3 — Contrast & focus (need approval)

| Step | Proposed change | Visible impact |
|---|---|---|
| 3.1 | `.t-light` / `.text-light`: raise opacity from .32 to ≥ .75 (≈ 7:1) — or use solid `#C9CED9`-type tint. | Footer secondary text slightly brighter |
| 3.2 | Button hover: keep gold background but use navy text (7.1:1), or keep white text and darken hover bg to `#8a6d2f`. **Pick one.** | Hover look changes |
| 3.3 | Input border: raise to a solid mid-tone (e.g. `#8C7A55`, ≥3:1 on white) or add a 2px navy bottom border. | Slightly stronger input outline |
| 3.4 | Add a global `:focus-visible` outline (2px `#005FCC`/gold with offset) to links, buttons, inputs. | Visible keyboard focus only |
| 3.5 | Find gold-on-white text usages (C5) and verify each. | TBD |

Per the Webflow-fix skill, contrast must be fixed by editing the **consuming style classes** (`data_style_tool → update_style`), not the colour variable (variable tool is unreliable). Duplicate style names (e.g. several `button-primary N`) can't be targeted reliably — I'd add a higher-specificity combo class instead.

### Phase 4 — Rest of site & verification

1. Run the same element queries on the remaining pages and CMS templates (Blogs, Blog Treatments, FAQs: rich-text images, tables, heading order, link text like "Read more").
2. Publish to **both** custom domains, wait for CDN, then verify with `curl` on the live HTML (grep for `<label`, `<main`, `alt=`), re-run WAVE, and record before/after counts.
3. Complete the manual checks in 2G.
4. Write the project handoff file (`FameDentistry-ADA-WCAG-Handoff.md`) per the Webflow-fix skill.

### Effort / risk

| Phase | Effort | Risk |
|---|---|---|
| 1 | Medium (Designer time, ~2–3 h) | Low visual risk; affects 20+ pages at once — test on 2 pages before publishing |
| 2 | Low | Low |
| 3 | Low | Visual — approval needed |
| 4 | Medium | — |

---

## 4. Decisions needed from you

1. **Contrast changes (Phase 3):** OK to adjust footer text opacity, hover state and input border? Which hover option (3.2)?
2. **Social links:** provide the Facebook/Instagram/LinkedIn URLs, or remove the icons.
3. **Pages:** unpublish `/test`? Keep or redirect `/home-v2` and `/for-dentists`?
4. **Designer access:** Phase 1 needs hands in the Designer (or a Designer-connected MCP session). Who does it?
5. **WAVE run:** please share the WAVE report (or screenshots) for Home so I can confirm the "L" items before we spend Designer time.

---

## 5. Caveats

- Automated tools find roughly 30% of accessibility issues; manual keyboard and screen-reader testing is still required.
- Alt-text presence is checkable; quality needs human review.
- Legal basis: ADA Title II rule (DOJ, 24 Apr 2024) adopts WCAG 2.1 AA; compliance dates 26 Apr 2027 (≥50k population entities) / 26 Apr 2028 (smaller). Private-sector Title III has no final rule, but courts apply WCAG 2.1 AA in practice. The practice is UK-based, where the Equality Act 2010 and EN 301 549 / WCAG 2.1 AA are the relevant benchmarks; ADA is the standard requested here.
