# Site Teardown: The Present, Death of the Old World product page

**URL:** https://www.thepresent.club/shop/p/death-of-the-old-world-7-universal-truths-of-existence-ebook
**Platform:** Squarespace 7.1 Commerce (product page template)
**Date analyzed:** 2026-07-27
**Purpose:** migrate this page off Squarespace onto larayap.vercel.app as `book.html`, replicated exactly: same palette, type, sizing, ratios, and whitespace.

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
8. **The seven truths**, 3-column grid, 3 rows, seventh cell centred. Each cell: icon, name, script label, description
9. **"What it means to let your old world die"**, large centred image + 4 paragraphs
10. **Reader testimonial**, quote block, followed by 4 paragraphs of author response
11. **FAQ**, 8 accordion items
12. Footer, copyright only

## Design system (Squarespace original)

Measured live off the page with `getBoundingClientRect` and `getComputedStyle` at a 1265px content width. **Carried over exactly**, per Lara's instruction on 2026-07-27 to replicate rather than reinterpret.

| Token | Value |
|---|---|
| Paper | `#f4f2eb` |
| Ink (all text) | `#5e1803` |
| Heading font | `now-jqaerb` (Now). Already in this repo as OTF. |
| Body font | Libre Baskerville |
| Script accent | `golden-trexhl`. Not licensed here, **Caveat stands in.** |

| Role | Size at 1265 | cqw |
|---|---|---|
| h1 / h2 | 39.648px / lh 1.2 and 1.1136, ls -0.02em | 3.1343 |
| h3 (column labels) | 13px, weight 700, ls 1.5px, uppercase | 1.0277 |
| h4 (truth names) | 16.608px, ls -0.02em | 1.3129 |
| Body | 15.072px / lh 1.5, para margin 12px | 1.1915 |
| Truth descriptions | 12.768px / lh 1.5 | 1.0093 |
| Metamorphosis copy | 18.144px / lh 1.5 | 1.4343 |
| Script heading | 37.3px then 48.7px | 2.9486 / 3.8498 |

**Geometry (page 1265, side margin 51.2, inner 1162.6):**
- Product grid: gallery 581.3 / gap 25.6 / details 555.7. Thumb rail 50 wide, 3:4 thumbs, 10px gaps; stage 521.3 x 695.1, also 3:4.
- The **details column starts at the top and the gallery is pushed down 155.7px**, not the other way round. Getting this backwards was the first build's biggest visual error.
- "This Isn't For Everyone" block 800 wide, centred; the two columns inside are 327.5 each with a 73 gutter, so 728 total.
- **The seven truths are a 3-column grid, not alternating rows.** Columns 380.2 wide, 11px gutters, 487px row pitch, the seventh cell centred in the middle column. Cell order is image, name (h4), script "Truth N" label, description.
- Truth images are 380.2 x 279.5 (1.36:1), so the square 1800px source icons are cropped by `object-fit: cover`.
- Metamorphosis copy block 967 wide centred, book image 478 x 493 centred, quote indented 40 and 887 wide.
- FAQ block 720 wide centred.
- Section top paddings: product 32.1, not-for-everyone 148.5, truths 117.8, metamorphosis 244, FAQ 144.4, digital-product 38.4.

Everything is expressed in `cqw` against a `container-type: inline-size` wrapper capped at 1265px, so the whole page scales as a unit and is pixel-identical to the original at the reference width.

## Assets (already supplied by Lara, in `Book/`)

| Asset | File | Notes |
|---|---|---|
| Product/cover image | `Book/Death of the Old World Ebook.png` | 1080x1350, RGB, has its own background |
| Preface spreads | `Book/Preface 1-4.png` | 1078x1532, opaque white book pages |
| Contents spreads | `Book/TOC 1-2.png` | 1078x1532, opaque white book pages |
| Seven truth icons | `Book/7 Truths Icon/Truth I-VII *.png` | 1800x1800 **transparent** PNG, maroon mark (#5e1803-ish) occupying roughly the centre 31 percent |

Those 7 gallery images are exactly cover + 4 preface + 2 contents.

Gallery order, read off the original's thumbnail rail: cover, Contents 1, Contents 2, Preface 1 to 4.

On the original's cream paper both the maroon icons and the white page scans sit correctly with no treatment at all. An earlier dark-background version of this rebuild needed cream discs behind the icons; replicating the original removed that problem rather than solving it.

## Build plan

Static single file, no framework, no packages. One HTML file, inline `<style>`, local `@font-face` for Now, Google Fonts for Libre Baskerville and Caveat, ~25 lines of vanilla JS for the gallery, MailerLite's `webforms.min.js` for the form.

Section order is the original's, unchanged. Only the commerce parts differ:

1. Header: Home / The Present / social icons
2. Breadcrumb
3. **Product**: thumb rail + stage with click-to-view and prev/next arrows, alongside title, description, benefits
4. **Price becomes "Free"**, and the Add To Cart button becomes the MailerLite email capture in the same position
5. **This Isn't For Everyone**, two columns
6. **The seven truths**, 3-column grid under the script heading
7. **What it means to let your old world die** + the reader testimonial
8. **FAQs**, native `<details>`, no JS
9. **How you'll receive your file**, rewritten from the purchase steps to the email steps
10. Footer

## Notes

- FAQ items 7 and 8 both described the Squarespace download-link mechanics (24 hour link, lifetime access, no resale). Those describe a purchase flow that no longer exists. Rewritten to describe the email delivery instead, and the two near-duplicate questions collapsed into one.
- The "$7.00, was $18.00" framing is gone entirely. Nothing on the rebuilt page should imply a price ever existed, since the free edition is now edition one.
- Native `<details>` gives a working accordion with zero JavaScript and correct keyboard and screen reader behaviour, which the Squarespace block did not guarantee.
