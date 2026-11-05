# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this site is

Personal academic website for Phuong Q. Le (social psychologist, UChicago Booth), built on the **al-folio** Jekyll theme and deployed to GitHub Pages. The al-folio theme source lives directly in the repo (not as a gem), so layouts, includes, sass, and plugins are all editable.

## Building and running locally

**Preferred (Docker):**
```
docker compose up
```
Serves at `http://localhost:8080` with live reload on port 35729.

**Without Docker:**
```
bundle exec jekyll serve
```
Serves at `http://localhost:4000`.

**Deployment** is automatic: pushing to `master` triggers `.github/workflows/deploy.yml`, which builds in production mode, runs PurgeCSS, and deploys `_site/` to GitHub Pages. No manual steps needed.

## Active pages and where to edit content

Only four pages are built and visible (the rest are excluded in `_config.yml`):

| Page | File | Notes |
|------|------|-------|
| Homepage | `_pages/about.md` | Bio, profile photo, social links |
| Research | `_pages/research.md` | Research themes, paper summaries |
| Publications | `_pages/publications.md` + `_bibliography/papers.bib` | BibTeX-driven via jekyll-scholar |
| News | `_pages/news.md` + `_news/*.md` | News announcements (currently disabled on homepage) |

Pages that exist in `_pages/` but are deliberately excluded from the build: `blog.md`, `cv.md`, `projects.md`, `profiles.md`, `repositories.md`, `teaching.md`.

## Content editing workflows

**Adding/updating a publication:** Edit `_bibliography/papers.bib`. The custom BibTeX fields `pdf`, `website`, and `abstract` render as buttons/modals on the publications page. Co-author profile links are configured in `_data/coauthors.yml`.

**Citation checklist (new or changed paper):** A paper can live in up to three places; check each one.
1. `_bibliography/papers.bib` (Publications page). Entries are appended in chronological order. Key format is `lastname_keyword_year` (e.g., `le_tradeoffs_2026`). Write authors as `Last, First and Last, First` with full first names; if a first name is unknown, use initials as given and ask the user rather than guessing. Include `abstract` when supplied (use real curly quotes/em dashes, no escaping needed), plus `doi`/`website`/`pdf` when available. The file uses CRLF line endings, so keep them consistent when editing from PowerShell.
2. `_pages/research.md` (Research page), only if the paper belongs to one of the research themes. Follow the simplified citation format (see the citation-formatting memory): authors, year or status, sentence-case title, journal; no volume/issue/pages.
3. The paper's PDF, if any, goes in `assets/pdf/` (name pattern `Le_etal(2024).pdf`).

**In-press papers:** The Publications page shows an "In press" section above the year groups (`_pages/publications.md` runs two `{% bibliography %}` queries). An entry appears there only if it has `inpress = {true}` in papers.bib. For in-press papers, add that field, put `(in press)` in the `journal` (or `booktitle` for chapters) field, and use the expected publication year. Once the paper is published, delete the `inpress` line, remove "(in press)", and add volume/pages/doi as available, so it falls into its year group. Also update its research.md line from "in press"/"under review"/"in prep" to the new status. Any other custom field (`inpress`) is safe: `hide-custom-bibtex.rb` only affects the BibTeX modal.

**Verifying in the preview:** After editing, wait about 10-20 seconds for Jekyll to regenerate, then fetch `http://localhost:8080/publications/` and `/research/` and confirm the entry, author names, and section placement render correctly before committing.

**Updating profile photo:** Replace `assets/img/prof_pic.jpg`. ImageMagick auto-generates WebP variants at build time.

**Adding PDFs:** Drop them in `assets/pdf/`. The CV PDF filename is `PhuongLe_CV (website).pdf` (the homepage button links to this exact name, so replace the file in place when the CV is updated).

**Site-wide settings** (name, email, social accounts, analytics, page visibility): `_config.yml`.

## Architecture notes

- Templates use `.liquid` extension (Liquid templating, not `.html`)
- Styles are in `_sass/` (SCSS); theme tokens in `_sass/_themes.scss`, layout in `_sass/_layout.scss`
- `_layouts/about.liquid` controls the homepage structure; `_layouts/bib.liquid` controls how each publication entry renders
- `_plugins/` contains custom Ruby plugins — notably `google-scholar-citations.rb` (fetches citation counts) and `hide-custom-bibtex.rb` (strips internal-only fields from rendered output)
- Google Analytics ID: `G-M91TL1BZB0` (set in `_config.yml`)
- `assets/json/resume.json` is loaded at build time via `jekyll-get-json` — update it if CV data changes

## Linting

Prettier is configured for Liquid/HTML/Markdown/YAML via a pre-commit hook (`.pre-commit-config.yaml`) and a CI workflow. Config is in `.prettierrc` (printWidth 150, uses `@shopify/prettier-plugin-liquid`). To run manually: `npx prettier --write .`
