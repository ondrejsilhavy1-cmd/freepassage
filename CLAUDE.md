# CLAUDE.md

Guidance for working in this repository.

## Purpose

**Free Passage — Privacy Primer** is a single-page static website that argues for online privacy and lists recommended privacy tools. It is aimed at journalists, activists, survivors, legal and medical professionals, dissidents and everyone else. It contains:

- A **Manifesto**: the case against encryption bans, age/ID verification and mass surveillance.
- **Who This Is For**: six audience cards.
- **The Toolkit**: curated, open-source-first tools grouped into collapsible categories (VPN & network, messaging & email, privacy finance, browser & search, operating systems, no-KYC directories, privacy news sources). The page states there are no affiliate links.
- An **FAQ** with expandable answers, some citing external sources.
- A footer linking to EFF, Privacy Guides and EPIC.

The site describes itself as "No tracking · No cookies".

## Stack

- Plain **HTML + inline CSS + inline vanilla JavaScript**, all in one file.
- No framework, no build step, no package manager, no dependencies.
- The only external resource is **Google Fonts** (Barlow and Barlow Condensed), loaded from `fonts.googleapis.com`.
- The JS (about 30 lines at the end of the file) handles:
  - a custom cursor that follows the mouse (`#cursor`, with `.hover` growth on links, tool categories and audience cards)
  - opening and closing accordions (`toggleCat()` for tool categories, `toggleFaq()` for FAQ items)
  - a scroll-reveal animation using `IntersectionObserver`, which adds `.visible` to `.reveal` and `.manifesto-p` elements.

## Structure

```
.
├── index.html   # the whole site (~1350 lines)
└── CLAUDE.md
```

Layout of `index.html`:

| Lines (approx.) | Content |
|---|---|
| 1–8 | `<head>`: meta, title, Google Fonts link |
| 9–743 | `<style>`: CSS variables in `:root` (`--black`, `--white`, `--mid`, `--display`, `--body`, …), then per-section styles; responsive breakpoints at 900px and 600px (~line 646) |
| 747–758 | Cursor element and fixed `<nav>` (anchors `#manifesto`, `#who`, `#tools`, `#faq`) |
| 761–790 | Hero section, including 4 headline stats |
| 793–811 | `#manifesto` |
| 814–857 | `#who`: six `.who-card` audience cards |
| 860–1186 | `#tools`: `.tool-category` blocks (A, B, …), each containing `.tool-item` entries with `.tool-name`, `.tool-desc` and a `.tool-url` link |
| 1189–1302 | `#faq`: `.faq-item` blocks |
| 1305–1320 | `<footer>` |
| 1322–1352 | `<script>` |

Conventions:
- External links use `target="_blank" rel="noopener"` and end with ` ↗`.
- Each tool category header shows a hand-written count (for example `5 Tools`). Update it whenever you add or remove a tool.
- Sections are numbered by hand (`01`–`04`) and tool categories are lettered by hand (`A`, `B`, …).

## How to run locally

No build is needed. Either:

- open `index.html` directly in a browser, or
- serve the folder statically, for example `python3 -m http.server 8000`, then open http://localhost:8000.

An internet connection is needed for the Google Fonts to load. Without it, the page falls back to a sans-serif font.

## How to test

The repo has no automated tests, linter or CI. Check changes by hand:

1. Load the page and check that every nav anchor scrolls to its section.
2. Click every tool category and FAQ header to check that it expands and collapses.
3. Scroll the page and check that the reveal animations play and no content stays invisible (elements start at `opacity: 0`).
4. Check the layout at desktop width, at ≤900px and at ≤600px (at ≤600px the nav links are hidden).
5. Optionally, validate the HTML (for example with the W3C validator) and run a link checker over the external URLs.

## Deployment

Verified facts:

- The default branch is **`main`**.
- The repository contains only `index.html`. There is no README, no `package.json`, no GitHub Actions workflows, and no `vercel.json`, wrangler, railway, `Dockerfile` or netlify config.
- **GitHub Pages is not enabled.**
- The repo's **homepage URL is empty**.
- The repo has **no GitHub Deployments**.

**Hosting and auto-deploy cannot be confirmed from the repo or from GitHub metadata.** If the site is hosted somewhere, that integration is not visible here. For example, it could be a hosting provider connected through its own dashboard, or a manual upload. Do not assume that pushing to `main` publishes the site. Ask the maintainer how it is deployed before relying on it.

Because the site is one static file, any static host works with no build command and `index.html` as the entry point.

## Environment variables

**None.** The code reads no environment variables. There is no `.env.example`, no server-side code and no build-time configuration.

## Known issues / TODOs

- **The hero tool count is wrong.** The hero stat says "23 Curated open-source tools", but the page lists 28 `.tool-item` entries: 22 tools in categories A–E plus 2 directories and 4 news sources. Fix either the number or the wording.
- **The "No tracking" claim conflicts with Google Fonts.** The page says "No tracking · No cookies · No surveillance", yet it loads Google Fonts from Google's servers, which exposes visitors' IP addresses to Google. To keep the claim true, self-host the fonts.
- **Dead CSS** remains from a removed news-feed feature: `.feed-wrap`, `.feed-controls`, `.feed-btn`, `.feed-grid`, `.feed-card`, `.feed-source`, `.spinner` and `@keyframes spin`. None of these classes are used in the HTML, but the responsive rules still mention them.
- **Accessibility gaps:**
  - The accordions are `<div onclick>` elements, so they cannot be reached or operated by keyboard and have no `aria-expanded`.
  - `body { cursor: none }` hides the system cursor and relies on a JS cursor. If the JS fails, mouse users see no cursor.
  - There is no `prefers-reduced-motion` handling for the animations.
- **Possibly outdated link:** Tails points to `tails.boum.org`; the project has since moved to `tails.net`. Periodically check all external tool and citation links, since the FAQ cites dated news articles.
- **No mobile navigation:** at ≤600px the nav links are hidden and nothing replaces them.
- Section numbers, category letters and tool counts are all hard-coded and must be kept in sync by hand.
- There is no README, LICENSE, favicon, or social/OpenGraph meta tags.
