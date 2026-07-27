# Site Teardown: The Present, Death of the Old World product page

**URL:** https://www.thepresent.club/shop/p/death-of-the-old-world-7-universal-truths-of-existence-ebook
**Platform:** Squarespace 7.1 Commerce (product page template)
**Date analyzed:** 2026-07-27
**Purpose:** migrate this page off Squarespace onto larayap.vercel.app as `book.html`, keeping the Mind Aligner dark palette and type instead of the Squarespace light theme.

## Tech Stack

| Technology | Evidence | Purpose |
|---|---|---|
| Squarespace 7.1 Commerce | `images.squarespace-cdn.com` asset host, `/shop/p/` product URL, cart state in header | Whole site, product page template |
| Squarespace product gallery | "Image 1 of 7" numbering in markup | 7-image carousel above the fold |
| Squarespace accordion block | 8 expandable FAQ items | FAQ section |
| Squarespace commerce button | "Add To Cart" / "Added!" states | Checkout, and the reason the button cannot be repointed by hand |

Nothing custom. No animation library, no scroll effects, no smooth scrolling, no custom cursor. This is a template page whose value is entirely in the **content structure and section order**, not in its engineering. That makes it cheap to rebuild and means nothing is lost by leaving Squarespace.

## Section order (top to bottom)

1. Header, logo + cart + social links
2. Breadcrumb, Shop > product name
3. **Product gallery**, 7 images, carousel
4. **Product header**, title + sale price ($7.00 struck from $18.00)
5. **Description**, 5 paragraphs, single column
6. **Benefits**, 7 bullets
7. **"This isn't for everyone"**, two-column tick and cross lists, 5 items each
8. **The seven truths**, 7 blocks, alternating image left / text right, one icon each
9. **"What it means to let your old world die"**, large centred image + 4 paragraphs
10. **Reader testimonial**, quote block, followed by 4 paragraphs of author response
11. **FAQ**, 8 accordion items
12. Footer, copyright only

## Design system (Squarespace original)

Light theme, cream/white background, serif headings, generic Squarespace spacing. **Deliberately not carried over.** The rebuild keeps the Mind Aligner tokens from `index.html` (`--dark #1c1410`, cream text at 92/70/50/30 percent, `BN` italic display, `Lora` body, `Poppins` small caps, the grain overlay) so the page reads as one property with the parent site.

What IS carried over is the **structure**: section order, the two-column for/against pattern, the alternating truth rows, the accordion FAQ, and the gallery.

## Assets (already supplied by Lara, in `Book/`)

| Asset | File | Notes |
|---|---|---|
| Product/cover image | `Book/Death of the Old World Ebook.png` | 1080x1350, RGB, has its own background |
| Preface spreads | `Book/Preface 1-4.png` | 1078x1532, opaque white book pages |
| Contents spreads | `Book/TOC 1-2.png` | 1078x1532, opaque white book pages |
| Seven truth icons | `Book/7 Truths Icon/Truth I-VII *.png` | 1800x1800 **transparent** PNG, maroon mark (#5e1803-ish) occupying roughly the centre 31 percent |

Those 7 gallery images are exactly cover + 4 preface + 2 contents.

**Two handling problems solved in the rebuild:**
- The truth icons are maroon on transparent, which is nearly invisible on `#1c1410`. Solved by seating each mark on a cream disc, which also matches The Present's own palette. The disc crops the icon's large empty margin via `overflow:hidden` and a 200 percent image width.
- The preface and contents scans are opaque white pages. Left white on purpose, framed with a soft border and shadow so they read as pages of a physical book rather than as broken transparency.

## Build plan

Static single file, no framework, no packages. Same approach as `index.html`: one HTML file, inline `<style>`, local `@font-face` pointing at the repo's existing OTF folders, Google Fonts for Lora/Poppins, MailerLite's `webforms.min.js` for the one form.

Section-by-section, with the commerce parts replaced:

1. **Top bar** kept from the first `book.html` build
2. **Hero**: cover + title + "The full book, free." + the MailerLite form. Replaces gallery + price + Add To Cart. **The price block is deleted, not restyled**, since the book is free.
3. **Look inside**: horizontal scroll strip of the 6 preface and contents scans. This is the Squarespace gallery, re-framed.
4. **Description** paragraphs
5. **What you will take from it**: the 7 benefit bullets
6. **This is not for everyone**: two columns, tick and cross
7. **The seven truths**: 7 alternating rows, icon disc + heading + her question-led paragraph
8. **What it means to let your old world die**: cover image + 4 paragraphs
9. **Testimonial** + author response
10. **FAQ**: 8 items, native `<details>/<summary>`, no JS
11. **Closing CTA** anchoring back to the form
12. **Footer**

## Notes

- FAQ items 7 and 8 both described the Squarespace download-link mechanics (24 hour link, lifetime access, no resale). Those describe a purchase flow that no longer exists. Rewritten to describe the email delivery instead, and the two near-duplicate questions collapsed into one.
- The "$7.00, was $18.00" framing is gone entirely. Nothing on the rebuilt page should imply a price ever existed, since the free edition is now edition one.
- Native `<details>` gives a working accordion with zero JavaScript and correct keyboard and screen reader behaviour, which the Squarespace block did not guarantee.
