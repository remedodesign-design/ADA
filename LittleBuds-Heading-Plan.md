# Little Buds: one H1 and one H2 per page (plan, not yet applied)

Source: live HTML on https://littlebuds.webflow.io/ (published 16 Oct 2026). Not applied: the Webflow connector currently points to a different workspace than Little Buds (site `6a96ff38b7d0003e252f494f`).

## Rule
- Keep the one H1 already on each page (all 17 pages have exactly one).
- Keep exactly one H2 per page: the first real section heading after the H1.
- Change every other H2 to H3 using `set_heading_level` only. Classes are not touched, so font size, weight and spacing stay identical (for example `heading-style-h2` keeps its size; only the tag changes).
- Nothing is published until you approve.

## Shared components (change once, applies everywhere)
| Component heading | Where it appears | Change |
|---|---|---|
| "Join our VIP waitlist" / "Request an appointment" (`is-appointment`) | 15 pages | H2 to H3 |
| "Let's make their smile shine!" (`cta_title`) | 8 pages | already styled as h3; H2 to H3 |
| "Something Special Is Coming!" (`lb-title`) | all 17 pages | H2 to H3 |
| "Give your child a reason to smile" (`home-cta-card_title`) | home | H2 to H3 |

The "VIP waitlist" wording in these headings also needs rewriting, as you asked for the SEO fields; heading text is a separate decision.

## Per page: the single H2 that stays
| Page | Stays H2 | Everything else |
|---|---|---|
| / | Meet Dr. Vidisha Patel! | 9 other H2s become H3 |
| /our-story | Story behind the practice | 6 become H3 |
| /meet-dr-patel | About Dr. Vidisha Patel | 6 become H3 |
| /insurance | Which insurance plans do you accept? | 5 become H3 |
| /referrals | Send a referral | 4 become H3 |
| /book-an-appointment | Request an appointment | 3 become H3 (this one is a shared heading, see note 1) |
| /contact-us | Frequently asked questions | 2 become H3 (see note 1) |
| /services/preventive-dentistry | Is your child due for preventive care? | 7 become H3 |
| /services/infant-dentistry | Is it time for your baby's first dental visit? | 7 become H3 |
| /services/restorative-dentistry | Does your child need restorative care? | 7 become H3 |
| /services/emergency-dentistry | Is your child having a dental emergency? | 7 become H3 |
| /blog/gum-bubble-tooth-abscess-warning-signs | Is it really an abscess? | 11 become H3 |
| /blog/why-do-baby-teeth-need-crowns | Why baby teeth are worth protecting | 9 become H3 |
| /blog, /services | No section heading exists after the H1 (see note 2) | |
| /privacy-policy, /terms-and-conditions | Information We Collect / Use of Our Website | rest become H3 (see note 3) |

## Notes that need your call
1. Contact and Book: their first section heading after the H1 is a shared component heading. Making it H3 would jump H1 to H3 (a WCAG heading-skip warning). Fix: use the shared appointment heading as the one H2 on those two pages only, or add a hidden H2.
2. Blog index and Services: the first headings after the H1 are card titles. Demoting them to H3 skips a level. Cleanest fix is one visually hidden H2 ("Latest articles" / "Our services"), but creating an element needs the Designer (the MCP cannot create elements). Alternative: keep one card title as H2, which is semantically wrong.
3. Legal pages: demoting all but the first section makes the other sections look like sub-sections of "Information We Collect". A hidden H2 avoids that; same Designer limit.
4. One H2 per page is not standard SEO practice (long pages usually use several H2s), but it is your call and it is fully reversible.

## Verify before publishing
- Re-run the heading crawl: 17/17 pages with exactly one H1 and one H2, no skipped levels, no repeated text.
- Playwright at 320, 360, 375, 414, 480, 600, 768, 820, 991, 1024, 1280, 1440, 1920: screenshots before and after, and computed font-size of every changed heading must be identical.
- WAVE on home, a service page, a blog post and Contact.
