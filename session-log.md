# Session Log: Rebuilding a Framer Page with Claude Code

This documents every step taken during the session, start to finish. It serves as a walkthrough showing how a designer can use Claude Code to build a production landing page from a Figma design.

## Starting point

A product designer exported a page from Framer and tried to use Claude Code to modify it. It didn't work because:

- The export was a 429KB monolithic HTML file (841 lines)
- All SVGs (App Store badges, Google Play, social icons) were inlined as raw path data
- Asset filenames were cryptic hashes (`32yEQtzFN2eAP3GfY0EV7fztsfI.png`)
- Multiple scattered `<style>` blocks, no component structure
- Claude Code couldn't even read the full file in one pass

## Step 1: Diagnose the problem

Analyzed the Framer export to identify why it was hostile to AI editing. Examined file size, structure, inline SVGs, scattered styles, and cryptic filenames.

## Step 2: Write the guide

Created `figma-to-code-with-claude.md` with actionable advice:

- Why Framer exports fail
- The screenshot-first workflow
- Section-by-section iteration strategy
- Asset naming conventions
- Design tokens approach
- Quick-start prompt template

## Step 3: Choose CSS approach

Evaluated plain CSS vs Tailwind CSS. Chose Tailwind for this project because:

- One file to manage (styles live on HTML elements)
- Claude Code produces accurate Tailwind output
- Zero build step (CDN script tag)
- Design tokens configurable in one place
- Responsive breakpoints are intuitive

Documented reasoning in `why-tailwind.md`.

## Step 4: Extract content from the Framer export

Used a subagent to read the 841-line HTML file in chunks and extract all text content organized by section: headings, paragraphs, button labels, FAQ Q&As, testimonials, footer links, phone numbers, legal disclaimers.

## Step 5: Identify the design from Figma

The user shared:
- A full-page screenshot of the Figma design
- The Figma file URL: `https://www.figma.com/design/YkmnztEbHm01QcGirgkAW0/Untitled?node-id=0-303`
- The live Framer site: `https://intelligent-gecko-264478.framer.app/`

From the screenshot, identified 10 distinct sections:
1. Navbar (dark green, dual logos, phone + CTA)
2. Hero (headline, description, CTAs, couple photo, trust badges)
3. "What is a LiveHealthy Visit?" (3 icon cards)
4. "Earn $150 Rewards" (3-step process cards)
5. "Not sure about tech?" (staff photo + support info)
6. "Trusted care from real doctors" (4-feature grid)
7. Testimonials (star ratings, 2 member quotes with avatars)
8. Schedule CTA banner (dark green)
9. Common Questions (FAQ accordion, 10 items)
10. Footer (multi-column links, app badges, social icons, legal)

## Step 6: Build the full page

Created `page.html` with all 10 sections in a single pass, using:

- Tailwind CSS via CDN with custom config for design tokens
- Gilroy Bold font (from assets) + Noto Sans (Google Fonts)
- Custom colors: primary `#0C847A`, dark green `#1B4332`, cream `#FFF8F0`, gold `#C4922A`
- Semantic HTML: `<nav>`, `<main>`, `<section>`, `<footer>`, `<details>`
- Native `<details>/<summary>` for FAQ accordion (no JavaScript needed)
- Responsive grid layouts with Tailwind breakpoints

## Step 7: Set up branch and version control

- Created branch `docs/figma-to-code-guide` from main
- First commit: the guide document
- Second commit: page.html + CLAUDE.md + why-tailwind.md
- Pushed to origin for GitHub Pages deployment

## Step 8: Deploy to GitHub Pages

Enabled GitHub Pages via `gh api` pointing to the `docs/figma-to-code-guide` branch. The page went live at:

- Our rebuild: https://asivura.github.io/figma-framer-claude/page.html
- Original Framer export: https://asivura.github.io/figma-framer-claude/index.html

## Step 9: Visual QA with Playwright

Installed Playwright and took full-page screenshots of:
- Our page at desktop (1440x900) and mobile (375x812) viewports
- The live Framer original for comparison

First round of screenshots revealed all images were broken because we used descriptive filenames that didn't match the actual asset files.

## Step 10: Identify and rename assets

Opened each cryptic image file to identify its content:

| Hash filename | Content | New name |
|---|---|---|
| `32yEQtzFN2eAP3GfY0EV7fztsfI.png` | Elderly couple on couch | `hero-couple.png` |
| `51wT7lXn8kGfjdURvdPMsc9TKBs.png` | Elderly woman portrait | `testimonial-woman.png` |
| `cvooq4lbXtBxlurDToeYAfu7VJ0.png` | Isaac Weaver (HealthTap staff) | `isaac-weaver.png` |
| `xB7Ll8KOzkE1Kcm3zkDzlGLt5w.png` | Elderly man portrait | `testimonial-man.png` |
| `a2rv0CcxjZMAHU7DaNgQ9rhc4.png` | HealthTap app icon | `healthtap-app-icon.png` |
| `cd1BlyS1PwT8LKjfV8JxsFed1A.svg` | HealthTap cross favicon | `favicon.svg` |
| `OYCpJpkl187FuBl4zBfhUJrQ5Jg.png` | QR code | `qr-code.png` |

Copied files with descriptive names (kept originals for the Framer export to still work).

## Step 11: Fix image references in page.html

Updated all `<img>` tags to use the renamed files. For assets that didn't exist in the export (Clover/HealthTap logos, app store badges):
- Used text-based logos in the navbar
- Created styled button links for App Store/Google Play

## Step 12: Compare and refine

Took fresh Playwright screenshots and compared against the Framer original. Identified remaining differences:

- Step 3 ("Earn rewards") needed dark green background with gold accent to match the Figma highlight
- Testimonial cards needed avatar photos next to the names

Fixed both issues in a dedicated commit.

## Final result

| Metric | Framer export | Our rebuild |
|---|---|---|
| File size | 429KB | ~28KB |
| Lines of code | 841 | ~510 |
| Files | 1 monolithic HTML | 1 HTML (with external Tailwind CDN) |
| Inline SVGs | ~105 SVG path lines | Small icons only (readable) |
| JavaScript | Custom accordion script | None (native `<details>` element) |
| CSS approach | 4+ scattered `<style>` blocks | Tailwind utilities inline |
| Responsive | Framer-generated | Tailwind breakpoints |
| Editable by AI | No (too large, no semantic structure) | Yes |

## Commits on the branch

1. `94601b0` - docs: add guide for building pages with Claude Code
2. `f7a0263` - feat: rebuild landing page with Tailwind CSS
3. `6ad2ed4` - fix: rename assets to descriptive names and fix image references
4. `cc5270c` - fix: match Figma design details

## Live URLs

- **Our rebuild:** https://asivura.github.io/figma-framer-claude/page.html
- **Original Framer export:** https://asivura.github.io/figma-framer-claude/index.html
- **Live Framer site:** https://intelligent-gecko-264478.framer.app/
