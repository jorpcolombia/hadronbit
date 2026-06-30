# CLAUDE.md — HadronBit Static Site

## Project overview

This repo is a static rebuild of **hadronbit.com** (originally WordPress).
Target deployment: **Cloudflare Pages** (primary) + **GitHub Pages** (backup).

No framework, no build step. Pure HTML/CSS/JS files that deploy as-is.

## Key conventions

- All pages live in their own folder with an `index.html` (clean URLs): `/about/index.html` → `hadronbit.com/about/`
- Internal links are always **root-relative**: `/about/`, `/services/`, never `./about.html`
- Assets go under `/assets/{css,js,images,fonts}/`
- Do not add WordPress-specific markup, query strings (`?ver=`), or admin cruft
- Do not add build tools, bundlers, or npm packages unless explicitly requested

## Current project state (as of 2026-06-30)

Scraping is complete. Five pages are committed in clean-URL layout (`/page-slug/index.html`).
**Active work: asset restructuring.** Pages still reference `wp-content/uploads/` and
`wp-content/themes/customify/assets/` paths. The `wp-content/`, `wp-includes/`, and `cdn-cgi/`
directories are committed and must be removed once assets are migrated to `/assets/`.

Still missing: `_redirects`, `_headers`, `404.html`, contact form replacement.

## Scraping workflow

Raw scraped output lives in `scraped/hadronbit.com/` (gitignored — large binary assets).
Cleaned output is committed at the repo root.

```bash
wget --mirror --convert-links --adjust-extension --page-requisites \
     --no-parent --wait=1 -P scraped/ https://hadronbit.com/
```

## Cloudflare Pages config

- Build command: *(none)*
- Output directory: `/`
- `_redirects` — URL redirect rules
- `_headers` — cache-control and security headers

## Contact form

WordPress forms replaced with a static provider (Formspree or Web3Forms).
Form `action` URLs must not contain API keys in source — use environment variables or
the provider's endpoint only.

## What NOT to do

- Do not commit the `scraped/` directory (gitignored)
- Do not add React, Vue, or any SPA framework
- Do not add a `package.json` unless the user asks
- Do not inline credentials or API keys in HTML files
