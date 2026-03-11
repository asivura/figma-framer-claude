# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

A reference project demonstrating how to build web pages from Figma designs using Claude Code. Contains a Framer export example (`index.html`) showing what NOT to do, and a guide (`figma-to-code-with-claude.md`) documenting the recommended workflow.

Fork of `etonev/figma-framer-claude`.

## Repository Structure

- `index.html` - Framer-exported page (429KB, monolithic). Used as a "before" example. Do not edit directly.
- `figma-to-code-with-claude.md` - The main guide for designers.
- `assets/images/` - Exported images with cryptic Framer-generated names.
- `assets/fonts/` - Gilroy Bold woff2 font.

## No Build System

This is a static HTML project with no package manager, bundler, linter, or test framework. To preview, open `index.html` in a browser.

## Design Reference

The page being rebuilt is a HealthTap/Clover Health landing page with these design tokens:

- **Colors:** Primary green `#0C847A`, dark green `#1B4332`, cream `#FFF8F0`
- **Fonts:** Gilroy Bold (headings), Noto Sans (body)
- **Max content width:** 1200px

## When Rebuilding the Page

Follow the approach in `figma-to-code-with-claude.md`: separate HTML from CSS, use descriptive asset filenames, build section by section, and use semantic HTML.
