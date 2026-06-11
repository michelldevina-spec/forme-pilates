# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a single-file, static marketing landing page for **FORME PILATES**, a boutique reformer Pilates studio in Singapore. Everything — HTML, CSS, and JavaScript — lives in `index.html`. There is no build step, package manager, bundler, linter, or test suite.

The full creative/copy/technical brief is `FORME_Pilates_Landing_Page.pdf` — treat it as the source of truth for design intent (monochrome editorial aesthetic, copy tone, page structure, and section-by-section requirements) when making changes.

## Running / Previewing

There is no dev server or build command. Open `index.html` directly in a browser (`file://` path) to preview. Use browser DevTools responsive mode to check mobile (~375px), tablet, and desktop (~1440px) breakpoints — the design is mobile-first.

## Deployment

`.github/workflows/deploy.yml` publishes the repo root to GitHub Pages via `actions/upload-pages-artifact` + `actions/deploy-pages` on every push to `main` (or manual `workflow_dispatch`). The repo's **Settings → Pages → Source** must be set to **GitHub Actions**. Because the whole repo root is published as-is, the root-level `index.html` and `FORME_Pilates_Logo*.png` files are the canonical, referenced copies — the `FORME pilates logo/` folder holds the original source assets and is not referenced by the page.

## Architecture (single file: `index.html`)

The file is organized top-to-bottom as:

1. `<head>` — SEO meta tags, Google Fonts (`Marcellus` for serif headings, `Jost` for sans body/UI), and one large `<style>` block.
2. `<style>` block — organized into clearly commented sections in this order: CSS variables/reset → typography/eyebrow labels/buttons → navbar → one block per page section (hero, difference cards, timeline, classes, schedule widget, testimonials, intro offer, booking form, FAQ accordion, final CTA, footer, sticky mobile CTA).
3. `<body>` — page sections in the same order as the CSS, each as a `<section id="...">` (e.g. `#hero`, `#difference`, `#experience`, `#classes`, `#schedule`, `#testimonials`, `#offer`, `#booking`, `#faq`), followed by a `<footer>` and a sticky mobile CTA bar.
4. A single `<script>` IIFE at the end of `<body>` containing all JS.

### Design system

All colors are defined as CSS custom properties on `:root` and must be used via `var(--name)` — the palette is strictly monochrome (no accent colors):
- `--fog` (#F5F5F4) page background
- `--dove` (#D9D9D6) borders/dividers
- `--stone` (#A8A8A4) decorative/secondary micro-copy (eyebrows, tags)
- `--graphite` (#5C5C59) body/secondary text (used where readability matters more than `--stone`)
- `--charcoal` (#1F1F1E) headlines, primary buttons, footer/CTA backgrounds
- `--white` (#FFFFFF) cards on fog background

Section "eyebrow" labels use the `.eyebrow` class (uppercase, letter-spaced) and follow the brief's literal letter-spaced copy (e.g. "T H E M E T H O D"). Buttons use `.btn` + a variant: `.btn-primary`, `.btn-secondary`, `.btn-light`.

### JavaScript (in the closing `<script>` IIFE)

- **Schedule widget**: `scheduleData` is a flat array of `{day, time, cls, instructor, duration, spots}` objects covering Monday–Saturday. `setActiveTab()`/`renderDay()` render `#scheduleList` from this array based on the clicked day tab in `#dayTabs`; defaults to the current day on load (Monday if today is Sunday); Sunday shows a "studio closed" message.
- **Navbar**: scroll listener toggles `.scrolled` on `#navbar`; `#navToggle` opens/closes the mobile `#navLinks` panel.
- **Sticky mobile CTA**: `#stickyCta` is hidden via `IntersectionObserver` once `#booking` scrolls into view.
- **Scroll animations**: any element with class `.fade-in` is revealed (`.visible`) via `IntersectionObserver`.
- **FAQ accordion**: `.faq-item` / `.faq-question` / `.faq-answer` — single-open accordion using `max-height` transitions.
- **Booking form** (`#bookingForm`): client-side validation (name, email, Singapore mobile `^[89]\d{7}$` normalized to `+65 ...`) then submits via `fetch` (AJAX) to FormSubmit (`https://formsubmit.co/...`) so success/error messages (`#formSuccess` / `#formError`) display inline without a page redirect.

## Placeholder Contact Details

The footer and booking form contain placeholder studio details that should be swapped for real values before launch: street address, nearest MRT station, WhatsApp number (`wa.me` link), contact email, opening hours, and Instagram/TikTok/Facebook handles. The FormSubmit recipient email in `#bookingForm`'s `action` attribute is already set to the real address (`michelldevina@gmail.com`).
