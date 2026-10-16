# Little Buds — ADA / WCAG 2.1 AA Audit Report & Fix Plan

**Site:** https://littlebuds.webflow.io/ (Webflow Site ID `6a96ff38b7d0003e252f494f`)
**Audit date:** 2026-10-12
**Standard:** WCAG 2.1 Level A + AA
**Scope:** Homepage in depth + structural scan of 9 inner pages (book-an-appointment, contact-us, our-story, meet-dr-patel, office-tour, services, insurance, referrals, blog). Service sub-pages and blog posts not scanned.
**Status:** Read-only audit. **No changes were made to the Webflow site or theme.**

## How this was run (read first)
This audit was done by analysing the live HTML and the compiled CSS (computed contrast from the real design tokens). The WAVE extension itself was **not** run: wave.webaim.org renders pages in a browser and was not reachable from this environment. Counts below are therefore *source-derived estimates*. Please run WAVE (https://wave.webaim.org/report#/https://littlebuds.webflow.io/) to confirm; the WAVE Details panel should match the items in sections 1–3. Automated checks cover only ~30% of WCAG, so section 5 (manual checks) matters.

## Estimated WAVE summary (homepage)
| Category | Est. count | Notes |
|---|---|---|
| Errors | 0 | Form labels are empty in static HTML but repaired by JS at load (see A3) |
| Contrast errors | 2–6 | Footer tagline, form placeholders (+ possibly more on inner pages) |
| Alerts | ~8–10 | Vague "Learn More" links ×6, redundant links, autoplay video, `_blank` links |
| Features | 50+ | 58 images with alt, labels, lang, landmarks, aria-labels |
| Structure | H1 ×1, H2 ×12, H3 ×16, `main`, `nav` ×4, `footer`, `banner` | |
| ARIA | ~12 | `aria-label`, `aria-current`, `aria-hidden`, `aria-live`, `aria-controls` |
| **Est. AIM score** | **≈ 8–9 / 10 (green)** | `10 − (0×2 + ~4×0.3 + ~9×0.1)` ≈ 7.9; ~9 if only 2 contrast errors. Confirm with WAVE |

The automated score will look good. The real exposure is the **keyboard/focus, form, motion and manual items** below, which WAVE does not score.

---

## 1. Errors & high-priority failures (fix first)

| ID | Issue | WCAG | Sev | Where | Evidence |
|---|---|---|---|---|---|
| A1 | **No skip-to-content link.** `<main id="main">` exists but nothing links to it. Keyboard users tab through topbar + full nav on every page. | 2.4.1 (A) | Serious | All pages | No `href="#main"` anchor in markup |
| A2 | **Focus indicators suppressed on nav controls.** Webflow default `outline:0` on `.w-nav-button`, `.w-dropdown-toggle`, `.w-dropdown-link` and only 1 `:focus-visible` rule in the sheet. Menu button and dropdowns show no visible focus. | 2.4.7 (AA) | Serious | Navbar, all pages | CSS: `.w-dropdown-toggle:focus{outline:0}` etc. |
| A3 | **Form labels have `for=""` in published HTML**; association is only created by an inline script at load (`label.setAttribute('for', id)`). Works in browsers/WAVE, but fails with JS off/slow and is fragile. Labels are `sr-only`, placeholder is the only visible label. | 1.3.1, 3.3.2, 4.1.2 (A) | Moderate | Appointment form on all 10 pages | `<label for="" class="sr-only">Name</label>` ×4 |
| A4 | **Consent checkbox label uses `<span for>`** (invalid `for` on span) – association relies on the wrapping `<label>`, which is OK, but the link inside the label ("Notice of Privacy Practices") is inside a clickable label (toggles checkbox on click). | 1.3.1, 3.2.2 | Moderate | Appointment form | `<span class="w-form-label" for="checkbox">` |
| A5 | **Autoplay looping hero video with no usable pause control.** `autoplay loop` runs >5 s; the Webflow pause button has class `hide`, so users cannot stop it. No `prefers-reduced-motion` handling for the video (CSS has 0 reduced-motion rules in the shared sheet; 3 in page embeds). | 2.2.2 (A) | Serious | Homepage hero | `<video autoplay loop muted>`; button `class="… hide …"` |
| A6 | **Dropdown triggers are `<div>` + `<p>`** (not buttons); keyboard operation depends on Webflow runtime adding role/tabindex. No `aria-expanded` verified in static HTML. | 2.1.1, 4.1.2 (A) | Moderate | "About Us", "Services" menus | `<div class="navbar_dropdown-toggle w-dropdown-toggle">` |

## 2. Contrast errors (computed from the real CSS tokens)

| ID | Element | Colours | Ratio | Needs | Verdict |
|---|---|---|---|---|---|
| C1 | Footer tagline (`.footer_tagline`) | `#b9c4a6` on `#4b5d3a` | **3.93 : 1** | 4.5 : 1 (small text) | **FAIL** – 1.4.3 |
| C2 | Form placeholders (act as visible labels) | `#8c8c7e` on `#fff` | **3.40 : 1** | 4.5 : 1 | **FAIL** – 1.4.3 |
| C3 | Form input borders | `#ddd6c2` on `#fff` | **1.45 : 1** | 3 : 1 | **FAIL** – 1.4.11 |
| C4 | Form field focus outline (`.form_input:focus`) | `#8fa06e` on `#fff` | **2.83 : 1** | 3 : 1 | **FAIL (marginal)** – 1.4.11 |
| C5 | Primary button text (`.button`, white on `#bb5a34`, 15 px bold) | | 4.54 : 1 | 4.5 : 1 | Pass – barely, **zero headroom** |
| C6 | Terracotta text `#bb5a34` on cream `#fbf7ef` (if used for text) | | 4.25 : 1 | 4.5 : 1 | **FAIL if used for text**; button boundary vs page only 4.25 (OK ≥3 for UI) |
| – | Body/heading/muted text, link `#a94f2e` on cream (5.11), `.is-small` btn (9.39), white on forest (7.18), topbar, footer links (4.64–5.43) | | | | Pass |
| ? | Hero title/subtext/badge over video + 50 % terracotta overlay | | | | **Not testable** – manual check (section 5) |

## 3. Alerts (human review)

| ID | Alert | WCAG | Detail |
|---|---|---|---|
| L1 | **Vague link text "Learn More" ×6** (hero/intro "Learn More", `#faq` "Learn More", 4× service-card "Learn More →") – identical text, different destinations | 2.4.4 (A) / 2.4.9 | Add `aria-label` e.g. "Learn more about preventive dentistry" |
| L2 | **Redundant adjacent links** – logo link, service cards, footer duplicates | 2.4.4 | Low risk |
| L3 | **`target="_blank"` without notice** ×4 (topbar address, footer address, Instagram, Facebook) | 3.2.5 (AAA)/best practice | Add "(opens in new tab)" via aria-label or sr-only text |
| L4 | **Social links point to generic `instagram.com` / `facebook.com`** root, not the practice's profiles | Content/usability | Confirm real profile URLs |
| L5 | **Heading skip h1 → h3** on Blog and Services (scanner-detected; h2 missing in hero→content) | 1.3.1, 2.4.6 | Change to h2 via `set_heading_level` (semantic only, no visual change) |
| L6 | **Same image appears twice with different alt** (reception: "Little Buds reception area" and "Bright reception area with…") – likely slider/gallery duplicates | 1.1.1 | Confirm; hide duplicate from AT |
| L7 | **Duplicate/similar page titles**: `/book-an-appointment` title is "Contact Little Buds… \| Book a Visit", too close to `/contact-us` | 2.4.2 | Unique titles |
| L8 | **Nested/duplicate navigation landmarks**: 4 `<nav>`, `<nav>` inside `<nav>` for dropdown lists, `role="navigation"` on a `<nav>` and `role="banner"` on a `<div>` (no `<header>`) | 1.3.1, 2.4.1 | Label nav landmarks (`aria-label`) |
| L9 | 12 empty-`alt` marquee/duplicate images on homepage are decorative | 1.1.1 | Confirm each is truly decorative |
| L10 | Placeholder text in static HTML is "Example Text" (JS replaces it). Without JS, visible hint is wrong | 3.3.2 | Fix with A3 |

## 4. Features (positive signals)
`lang="en"` ✔ · descriptive `<title>` ✔ · single H1 per page ✔ · `<main id="main">` ✔ · 58 images all have `alt` attribute (0 missing) ✔ · descriptive alt on meaningful photos ✔ · decorative images use `alt=""` ✔ · map iframe has `title` ✔ · SVG icons `aria-hidden` ✔ · social links have `aria-label` ✔ · `aria-current="page"` on active nav ✔ · `aria-live` region present ✔ · no positive `tabindex` ✔ · no duplicate IDs ✔ · viewport allows zoom ✔ · no empty links/buttons/headings ✔ · `role="list"` kept on lists ✔.

## 5. Manual checks WAVE cannot do (assign to QA)
1. Keyboard-only full pass: Tab order, dropdown open/close with Enter/Space/Esc, mobile menu button, sticky bottom bar ("Call Now", "VIP Waitlist") not covering focused elements (2.4.11 in WCAG 2.2).
2. Screen reader pass (NVDA/VoiceOver): form errors, success/fail messages announced (`aria-live`), dropdown state.
3. Hero text contrast over video/overlay at several frames.
4. 200 % zoom / 320 px reflow (1.4.4, 1.4.10) – sticky bar and topbar.
5. Form error identification and suggestions (3.3.1, 3.3.3), Turnstile CAPTCHA accessibility (3.3.8 in 2.2).
6. Alt text *quality* (all 58 images) and the 6 decorative marquee slides.
7. Motion: GSAP/ScrollTrigger animations respect `prefers-reduced-motion` (2.3.3 / best practice).
8. Touch target size ≥ 24×24 CSS px (small icon links, dots).
9. Link purpose in context for all "Learn More".
10. Blog posts and service sub-pages (not scanned).
11. Accessibility statement/contact method on the site (recommended).
12. PDF/documents (none found on scanned pages).

---

## 6. Fix plan (no theme/visual redesign – all changes are semantic, additive or token-neutral)

Guardrail: nothing below changes layout, fonts or brand palette. Where a colour must move, the change is the smallest step that reaches AA and needs your sign-off first (marked 🟠).

### Phase 1 — Zero-visual-risk (Webflow MCP or Designer, ~1–2 h)
| # | Fix | How in Webflow | Fixes |
|---|---|---|---|
| 1 | Add skip link | Designer: add Link Block as first child of Body, text "Skip to main content", `href="#main"`, class `sr-only` that becomes visible on `:focus`. Embed option: `<a href="#main" class="skip-link">…</a>` + 6 lines of CSS. *No element-creation API in MCP – Designer only.* | A1 |
| 2 | Label the appointment form natively | Form Block → each Field Label: set the label's **For** field to the input (Element settings), remove the JS patch dependency; keep `sr-only`. Also visible labels preferred (design call). | A3, L10 |
| 3 | Unique accessible names for repeated links | Link settings → add `aria-label` ("Learn more about Infant Dentistry", etc.) ×6; same for "Book Now" ×3 | L1 |
| 4 | New-tab warnings | Add `aria-label="… (opens in new tab)"` on the 4 `_blank` links | L3 |
| 5 | Label landmarks | `aria-label` on each `<nav>` ("Main", "About Us", "Services", "Footer", "Legal"); retag `role=banner` div → `<header>` if the Navbar component allows | L8 |
| 6 | Heading levels | `set_heading_level` on Blog & Services first content heading h3→h2 (semantic only) | L5 |
| 7 | Unique page titles | `update_page_settings` SEO title for `/book-an-appointment` | L7 |
| 8 | Confirm social URLs | Needs client input | L4 |

### Phase 2 — Small CSS additions (additive, low risk)
| # | Fix | How | Fixes |
|---|---|---|---|
| 9 | **Visible focus on all interactive elements** | Add one site-wide Embed (Head or Custom Code): `a:focus-visible,button:focus-visible,.w-nav-button:focus-visible,.w-dropdown-toggle:focus-visible,.w-dropdown-link:focus-visible,[tabindex]:focus-visible{outline:3px solid #3f4a32;outline-offset:2px}` (brand forest, ≥3:1 on cream/white). Also add `input:focus-visible` equivalent. | A2, C4 |
| 10 | Respect reduced motion | `@media (prefers-reduced-motion: reduce){ *{animation:none!important;transition:none!important;scroll-behavior:auto!important} }` + pause video via JS if reduce | A5, 7 |
| 11 | **Hero video pause control** | Un-hide Webflow's native pause button (remove `hide` class) – Designer on the Background Video element, or add Embed button. 🟠 Slight visual change to hero – confirm placement. | A5 |

### Phase 3 — 🟠 Colour tweaks (need your approval; smallest step to AA)
| # | Change | From → To | Result |
|---|---|---|---|
| 12 | Footer tagline | `#b9c4a6` → `#dce3cc` (already a theme token: *text-on-dark*) | 3.93 → **5.43** ✔ |
| 13 | Placeholder / helper text | `#8c8c7e` → `#66675a` (existing *text-muted*) | 3.40 → **5.9** ✔ |
| 14 | Input border | `#ddd6c2` → `#8c8c7e` (existing neutral) or `#8a8473` | 1.45 → **3.4** ✔ |
| 15 | Primary button headroom | Optional: `#bb5a34` → `#a94f2e` (existing hover colour) | 4.54 → **5.46** ✔ (currently passes) |
| 16 | Any text using `#bb5a34` on cream | → `#a94f2e` link token | 4.25 → **5.11** ✔ |

**Platform note (from the fixes playbook):** `data_variable_tool` has been unreliable and editing a shared variable has sitewide blast radius. Prefer editing the consuming classes (`.footer_tagline`, `.form_input`, `.form_input::placeholder`) via `update_style`, and confirm duplicate class names first. Custom attributes on Component-nested elements may not publish via MCP – use Designer for items 2, 3, 4, 5. Publish to all domains, wait for CDN, then verify with `curl` + grep.

### Phase 4 — Verify & document
1. Re-run WAVE on home + 3 key pages; target 0 errors, 0 contrast errors.
2. `curl` live HTML; grep for `href="#main"`, `aria-label="Learn more about`, `for="appt-name"`.
3. Keyboard + NVDA smoke test.
4. Write `{Project}-ADA-Handoff.md` per playbook.
5. Add accessibility statement page + feedback contact (recommended).

### Priority order
1. **A1 skip link, A2 focus, A5 video pause** – real keyboard/motion barriers
2. **A3 form labels, C2–C4 form contrast** – the contact form is the practice's conversion path
3. **C1 footer contrast, L1 link names**
4. L3–L10 clean-up, manual QA (section 5)

## Legal context
ADA Title II: WCAG 2.1 AA (DOJ final rule, 24 Apr 2024; compliance 26 Apr 2027 for ≥50 k population entities, 26 Apr 2028 smaller). Private-sector Title III (a dental practice): no final rule, courts apply WCAG 2.1 AA de facto. Automated testing covers ~30 % of issues; this report is a technical audit, not legal advice.

## Decisions needed from you
1. Approve Phase 3 colour tweaks (tokens already exist in the palette)?
2. Un-hide the hero video pause button (small visual change)?
3. Correct Instagram/Facebook URLs?
4. Want me to apply Phase 1–2 through the Webflow MCP now (site `6a96ff38b7d0003e252f494f`), with a pre-flight confirmation before each write and a publish only after your OK?
