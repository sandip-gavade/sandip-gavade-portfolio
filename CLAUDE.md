# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Commands

```sh
npm install              # install deps
npm run dev               # dev server at localhost:4321 (use --port if occupied: npm run dev -- --port 4322)
npm run build              # production build to ./dist/
npm run preview            # serve the production build locally
npm run astro -- check     # type-check .astro files against tsconfig (strict mode)
```

There is no test suite and no linter configured — don't invent one unless asked.

## Architecture

This is a **single-page Astro 7 static site** (Sandip Gavade's portfolio). There is exactly one route: `src/pages/index.astro`. There are no components, layouts, or content collections — the entire page (markup, all styling, and client-side interactivity) lives in that one file, split into Astro's three sections:

1. **Frontmatter (lines 1–68)** — all page content (`metrics`, `projects`, `skills`, `jobs`, `contacts`) is hardcoded as plain JS arrays here, not pulled from a CMS or data file. To update resume/project/skill content, edit these arrays directly.
2. **Template** — sections in order: nav, hero (with animated terminal block), metrics bar, work/projects grid, about, skills grid, experience timeline, education, contact, footer. Each section maps `.map()` over one of the frontmatter arrays.
3. **`<style>` block** — one large plain-CSS block scoped to the page (no Tailwind, no CSS framework, no custom-property theming — raw hex colors). Fonts (`IBM Plex Mono`, `Fraunces`, `Inter`) are loaded via a Google Fonts `<link>` in `<head>`, not self-hosted.
4. **`<script>` block** — vanilla client-side JS handling: nav scroll shadow, mobile hamburger toggle, scroll-reveal via `IntersectionObserver` on elements with the `.reveal` class, and animated number count-up on elements with `data-count`/`data-suffix` attributes. Respects `prefers-reduced-motion`.

SEO metadata (title, description, OG tags, canonical URL `https://sandipgavade.uk`) is hardcoded in `<head>` in `index.astro` — update it there if content changes materially.

## Deployment

Deployed to **Cloudflare Pages** via GitHub integration (auto-deploy on push to `main`). Build command: `npm run build`; output directory: `dist`. The GitHub repo is `sandip-gavade/sandip-gavade-portfolio`.

Note: this project lives in a subdirectory (`sandip-gavade/`) with its own git repo, nested inside a parent folder (`portfolio-website/`) that also contains an unrelated sibling directory (`Portfolio index.html redesign/`) holding the original hand-authored HTML design mockup this site was ported from. That sibling directory is outside this repo and not part of the build.
