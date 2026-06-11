# FORME PILATES — Landing Page

A single-file, static marketing landing page for **FORME PILATES**, a boutique reformer Pilates studio in Singapore. The design follows a monochrome editorial aesthetic with a mobile-first layout.

🔗 **Live site:** https://michelldevina-spec.github.io/forme-pilates/

## Overview

Everything — HTML, CSS, and JavaScript — lives in a single [index.html](index.html) file. There is no build step, package manager, bundler, linter, or test suite.

The full creative/copy/technical brief is [FORME_Pilates_Landing_Page.pdf](FORME_Pilates_Landing_Page.pdf) — it is the source of truth for design intent (page structure, copy tone, and section-by-section requirements).

## Page sections

- Hero
- The Difference (feature cards)
- Experience (timeline)
- Classes
- Schedule (interactive weekly widget)
- Testimonials
- Intro Offer
- Booking form
- FAQ (accordion)
- Final CTA
- Footer + sticky mobile CTA

## Running / previewing locally

No dev server or build command is required. Open [index.html](index.html) directly in a browser (`file://` path).

Use your browser's DevTools responsive mode to check the mobile (~375px), tablet, and desktop (~1440px) breakpoints — the design is mobile-first.

## Deployment

[.github/workflows/deploy.yml](.github/workflows/deploy.yml) publishes the repo root to **GitHub Pages** via `actions/upload-pages-artifact` + `actions/deploy-pages` on every push to `main` (or manual `workflow_dispatch`).

The repo's **Settings → Pages → Source** must be set to **GitHub Actions**. Because the whole repo root is published as-is, the root-level `index.html` and `FORME_Pilates_Logo*.png` files are the canonical, referenced copies — the `FORME pilates logo/` folder holds the original source assets and is not referenced by the page.

## Assets

- `FORME_Pilates_Logo.png`, `FORME_Pilates_Logo_Fog.png`, `FORME_Pilates_Logo_Inverted.png` — referenced logo variants used by the page.
- `FORME pilates logo/` — original source logo files (not used directly by the page).

## Booking form

The booking form (`#bookingForm`) validates input client-side (name, email, Singapore mobile number) and submits via AJAX to [FormSubmit](https://formsubmit.co/) so success/error messages display inline without a page redirect.

## Placeholder details to replace before launch

The footer and booking form contain placeholder studio details — swap these for real values:

- Street address
- Nearest MRT station
- WhatsApp number (`wa.me` link)
- Contact email
- Opening hours
- Instagram / TikTok / Facebook handles
