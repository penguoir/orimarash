# orimarash.com — Jekyll edition

Personal blog of Ori Marash, migrated from a Ghost site to Jekyll and hosted on GitHub Pages.

- **Live URL:** https://penguoir.github.io/orimarash/
- **Posts:** `_posts/` (one Markdown file per article, migrated from Ghost with full text + images)
- **Assets:** `assets/posts/<slug>/` (per-post images and downloads)
- **Theme:** a small custom theme — `_layouts/`, `index.html`, `assets/css/main.css`

## Local development

```bash
bundle install
bundle exec jekyll serve
# open http://localhost:4000/orimarash/
```

## Deployment

Pushing to `main` triggers `.github/workflows/pages.yml`, which builds the site
with Jekyll and publishes it to GitHub Pages.

## Migration notes

Content was migrated from the Ghost site at orimarash.com: every published post
and the About page, with body text converted to Markdown, internal links made
relative, and images (plus the CV PDF) downloaded into `assets/`.
