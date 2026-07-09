# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this is

A French-language advocacy/campaign website opposing an industrial veal-calf farm
project (**SAS DU MOYER**, 560 *veaux de boucherie*) at **Le Moyer, Verrie (49400)**, near
Saumur. The site presents the case, mirrors the official dossier, hosts a petition, and
directs visitors to file an *observation* with the préfecture before the public consultation
closes (21 July 2026). All visitor-facing content is in French — keep new content in French.

The site is a **Jekyll static site** living entirely in `docs/`, published via **GitHub Pages**.

## Build, preview, publish

- **Publishing is `git push` to `main`.** GitHub Pages builds from the `main` branch, `/docs`
  folder (Settings → Pages). There is no CI, no test suite, no lint step. The custom domain
  `verrie-bad-trip.houla.net` is set by `docs/CNAME` — do not delete or rename it.
- **No local toolchain is committed** (no `Gemfile`). To preview locally you must have Jekyll
  installed yourself, then run from inside `docs/`:
  ```bash
  cd docs && jekyll serve   # or: bundle exec jekyll serve, if you add a Gemfile
  ```
  Do not assume `jekyll` is available — earlier attempts found it absent on this machine.

## Layout & content architecture

Every page uses the single layout `docs/_layouts/default.html`, which pulls in three includes:
`header.html` (nav), `footer.html`, and `petition-form.html`. The nav in `header.html` is
hand-maintained — if you add a top-level page, add its link there too.

Pages are Markdown/HTML with Jekyll front matter. Convention in this repo:
- Set an explicit `permalink:` on every page except `index.md` (e.g. `permalink: /dossier/`).
- `title:` in front matter is auto-rendered as the page `<h1>` by the layout and prefixed
  into `<title>` — do not also write a manual `# Heading` at the top, it would duplicate.
- Link between pages/assets with Liquid filters, never hardcoded paths:
  `{{ '/dossier' | relative_url }}`, `{{ '/assets/images/x.jpg' | relative_url }}`.

The main content pages:
- `index.md` — landing page (hero, facts, petition, préfecture observation CTA). Written as
  raw HTML sections inside the Markdown for layout control.
- `dossier.md` — the case file: summary of the ICPE enregistrement application plus a table
  linking each *pièce* in three forms (PDF / OCR text / AI summary).
- `revue-de-presse.md` — press review. **Entries are ordered most-recent-first**; add new
  articles at the top. Each entry follows a fixed shape: `## Source — date`, bold headline,
  source/author/date line with a `[Lire l'article](url)` link, then a factual summary.

## The dossier pipeline (pièces)

Each official document exists in three parallel forms, and the three must stay in sync:
1. `docs/pieces/<name>.pdf` — the original scanned PDF (source of truth).
2. `docs/pieces-markdown/<name>-brute.md` — raw OCR text, **no correction or interpretation**
   (uncorrected OCR errors are expected and disclosed as such).
3. `docs/pieces-markdown/<name>-resume.md` — an AI-written summary of that OCR.

All three are surfaced from the table in `dossier.md`. When adding a new pièce, create all
three artifacts, give the two `.md` files explicit permalinks under `/pieces-markdown/...`,
and add a row to the `dossier.md` table.

## Petition & social sharing

- The petition (`_includes/petition-form.html`) is a third-party **Brevo** embed. The form
  POSTs to `sibforms.com` and loads Brevo's CSS/JS from CDNs — it is not self-hosted and has
  no backend in this repo. Treat it as vendor markup; avoid reformatting it wholesale.
- **Open Graph / Twitter Card** tags live in `_layouts/default.html` and drive link previews
  on Facebook, X, WhatsApp, etc. They build absolute image URLs from `site.url` +
  `site.image` (set in `_config.yml`) via the `absolute_url` filter, so `site.url` must stay
  set to the real domain. A page can override the preview with `image:` / `description:` in
  its front matter. After changing the share image, social platforms cache previews — refresh
  via the Facebook Sharing Debugger / X Card Validator.

## Not part of the public site

`notes-privees/` is gitignored and confidential — internal working notes, not published
content. Do not link to it from site pages or move its content into `docs/`.
