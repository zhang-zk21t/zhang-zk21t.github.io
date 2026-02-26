# Blog Website Design

## Overview

A personal blog for a CS PhD student, built with Hugo and the PaperMod theme. Primarily personal posts with occasional technical content. Authored in markdown, deployed to GitHub Pages.

## Requirements

- **Authoring**: Markdown files committed to git
- **Generator**: Hugo (static site generator)
- **Theme**: PaperMod
- **Hosting**: GitHub Pages via GitHub Actions
- **Design style**: Minimal and clean, typography-focused

### Features

| Feature             | Implementation                                         |
|---------------------|--------------------------------------------------------|
| Syntax highlighting | Hugo's built-in Chroma highlighter (zero JS)           |
| Tags/categories     | Hugo taxonomies + PaperMod templates                   |
| Table of contents   | PaperMod's `ShowToc: true` param                       |
| Search              | PaperMod's built-in Fuse.js search                     |
| Math/LaTeX          | KaTeX, loaded conditionally when `math: true` in front matter |

## Project Structure

```
opencode-try/
├── hugo.yaml                 # Main Hugo configuration
├── content/
│   ├── _index.md             # Homepage content
│   ├── posts/                # Blog posts
│   │   └── my-first-post.md
│   ├── about.md              # About page
│   └── archives.md           # Archives page
├── static/
│   └── images/               # Static assets
├── themes/
│   └── PaperMod/             # Theme (git submodule)
├── .github/
│   └── workflows/
│       └── deploy.yml        # GitHub Actions deployment
└── docs/plans/
```

## Configuration

Hugo config (`hugo.yaml`) includes:

- **Site metadata**: title, description, author, base URL
- **PaperMod params**: home-info mode for landing page, social icons, search, TOC
- **Taxonomies**: tags and categories
- **Syntax highlighting**: Chroma with a clean style
- **Math**: KaTeX via PaperMod's math extension
- **Search**: Fuse.js search page
- **Menu**: Home, Archives, Tags, Search, About

## Content Workflow

1. Write posts as `.md` files in `content/posts/`
2. Front matter: title, date, tags, categories, math (bool), summary, draft (bool)
3. Push to `main` branch
4. GitHub Actions builds with `hugo --minify` and deploys to `gh-pages` branch
5. GitHub Pages serves the site

## Deployment

- GitHub Actions triggers on push to `main`
- Builds with Hugo, deploys output to `gh-pages` branch
- GitHub Pages configured to serve from `gh-pages`

## Decisions Made

- **Hugo over other SSGs**: fastest builds, minimal dependencies, mature markdown support
- **PaperMod over other themes**: most popular, actively maintained, supports all required features out of the box
- **Markdown in repo over CMS**: simpler workflow, version-controlled content, no external dependencies
- **GitHub Pages over other hosts**: free, integrates with git workflow, automatic deploys
- **KaTeX over MathJax**: faster rendering, smaller bundle size
