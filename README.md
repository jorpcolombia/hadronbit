# HadronBit — Static Site Migration

Migration of hadronbit.com from WordPress to a fully static site deployable on GitHub Pages / Cloudflare Pages.

---

## Goal

Scrape the live WordPress site at `hadronbit.com`, convert it to clean static HTML/CSS/JS, and deploy it as a zero-dependency static site. No build step, no framework required.

---

## Phase 1 — Scrape the live site

Use `wget` to crawl the live WordPress site and download a complete offline copy.

```bash
wget --mirror \
     --convert-links \
     --adjust-extension \
     --page-requisites \
     --no-parent \
     --wait=1 \
     -P scraped/ \
     https://hadronbit.com/
```

This downloads all HTML pages, images, CSS, JS, and fonts into `scraped/hadronbit.com/`.

---

## Phase 2 — Audit & clean

- Review the scraped output: `scraped/hadronbit.com/`
- Identify pages (home, about, services, blog, contact, etc.)
- Remove WordPress-specific cruft: admin bars, login links, query strings, emoji JS, etc.
- Strip `?ver=` cache-busting params from asset URLs
- Consolidate or remove duplicate/unused assets

---

## Phase 3 — Restructure as a clean static site

Target folder layout:

```
/                   ← root of the repo
├── index.html
├── about/
│   └── index.html
├── services/
│   └── index.html
├── blog/
│   ├── index.html
│   └── post-slug/
│       └── index.html
├── contact/
│   └── index.html
├── assets/
│   ├── css/
│   ├── js/
│   ├── fonts/
│   └── images/
├── _redirects          ← Cloudflare Pages redirect rules (if needed)
├── _headers            ← Cloudflare Pages custom headers (cache, security)
└── 404.html
```

---

## Phase 4 — Fix & polish

- Update all internal links to be root-relative (`/about/`, `/services/`)
- Replace WordPress REST API calls or dynamic features with static alternatives
- Replace contact form with a static form service (Formspree, Web3Forms, etc.)
- Verify fonts are self-hosted or loaded from a CDN
- Minify CSS/JS if desired (optional)
- Add `<meta>` tags, Open Graph, favicons where missing

---

## Phase 5 — Deploy

### GitHub

```bash
git add .
git commit -m "Initial static site"
git push origin main
```

Enable **GitHub Pages** on the `main` branch (root `/`).

### Cloudflare Pages

1. Connect the GitHub repo in the Cloudflare Pages dashboard
2. Build command: *(none — pure static)*
3. Output directory: `/` (root)
4. Set custom domain: `hadronbit.com`

---

## Phase 6 — Validate

- [ ] All pages load without 404
- [ ] Images, fonts, and styles render correctly
- [ ] Internal links work
- [ ] Contact form submits correctly
- [ ] Mobile responsive
- [ ] Lighthouse score ≥ 90 on Performance / SEO / Accessibility
- [ ] `_headers` file sets correct cache-control and security headers

---

## Tools used

| Tool | Purpose |
|------|---------|
| `wget` | Crawl and download the live site |
| Cloudflare Pages | Hosting |
| GitHub | Version control + CI |
| Formspree / Web3Forms | Static contact form backend |

---

## Current status

- [x] Phase 1 — Scrape *(scraped output in `scraped/`, gitignored)*
- [~] Phase 2 — Audit & clean *(5 pages committed: home, cybersec audit, cybersec services, IT services, contact — still contain `wp-content/` asset references)*
- [~] Phase 3 — Restructure *(clean-URL folder layout in place; `wp-content/` and `wp-includes/` dirs still committed; no `/assets/` folder yet; missing `_redirects`, `_headers`, `404.html`)*
- [ ] Phase 4 — Fix & polish *(links not yet updated; contact form not yet replaced)*
- [ ] Phase 5 — Deploy
- [ ] Phase 6 — Validate

### Pages committed

| URL slug | File |
|----------|------|
| `/` | `index.html` |
| `/auditoria-de-ciberseguridad-diagnostico-empresarial/` | `auditoria-de-ciberseguridad-diagnostico-empresarial/index.html` |
| `/serviciosciberseguridad/` | `serviciosciberseguridad/index.html` |
| `/servicios-it-para-empresas-en-colombia-outsourcing-soporte-y-microsoft-365/` | `servicios-it-para-empresas-en-colombia-outsourcing-soporte-y-microsoft-365/index.html` |
| `/contacto-para-servicios-de-tecnologia-y-ciberseguridad-en-colombia/` | `contacto-para-servicios-de-tecnologia-y-ciberseguridad-en-colombia/index.html` |

### Next steps

1. Move `wp-content/uploads/` images → `/assets/images/` and update all `src`/`href` references
2. Move theme CSS → `/assets/css/`, JS → `/assets/js/`, fonts → `/assets/fonts/`
3. Delete `wp-content/`, `wp-includes/`, `cdn-cgi/` from the repo
4. Add `_redirects`, `_headers`, `404.html`
5. Replace the contact form with Formspree or Web3Forms endpoint
