# Flame Point Brewing — Claude Code Guide

## Project Overview

Hugo static site for Flame Point Brewing (`https://flamepointbrewing.com`). Uses a custom theme called **brewgo** stored as a git submodule at `themes/brewgo` (repo: `https://github.com/cgbaker/brewgo.git`).

- Hugo minimum version: 0.124.0
- Build: `hugo` (no Makefile; `hugo server` for local dev)

## Repository Structure

```
content/
  beers/[slug]/index.md     # Beer pages (page bundles with image assets)
  beers/_index.md           # Beers section index
  post/[date-slug].md       # Brewer's Notes blog posts
  page/about.md             # Static pages
static/images/              # Site-wide images (logo, favicon, hero)
layouts/partials/           # Project-level partial overrides
  head_custom.html          # Loads Montserrat font
  footer_custom.html        # Empty (placeholder)
themes/brewgo/              # Git submodule — theme source
hugo.toml                   # Site configuration
```

## Content Types

### Beers (`content/beers/[slug]/`)
Page bundles: each beer is a directory containing `index.md` and an image (`.jpg` or `.png`).

```yaml
---
title: "Beer Name"
weight: 1                  # display order (lower = first)
params:
  bjcp_url: "https://www.bjcp.org/style/2021/3/3A/czech-pale-lager/"
  bjcp_name: "Czech Pale Lager"
  bjcp_id: "3A"
  og: 1.031
  fg: 1.005
  ibus: 31
  abv: 3.4
  untappd: "https://untappd.com/b/flame-point-brewing-[slug]/[id]"
  festivals:
  - 2025 St. Mark's Beer Festival
  awards:
    gold:
    - 2025 Some Competition
    silver:
    - 2025 Some Competition
    bronze:
    - 2025 Some Competition
---
```

New beers: `hugo new beers/[slug]` (uses `themes/brewgo/archetypes/beers/index.md`).

### Posts (`content/post/[date-slug].md`)

```yaml
---
title: "Post Title"
date: 2025-01-29
author: Chris
---
```

Optional fields: `subtitle`, `tags`, `image`, `video`, `summary`.

### Pages (`content/page/[slug].md`)

```yaml
---
title: About Us
comments: false
bigimg: [{"src": "/images/equipment.jpg"}]
---
```

## Theme (brewgo)

**Important:** The theme is a git submodule. Changes to files under `themes/brewgo/` must be committed inside the submodule first, then the parent repo's submodule pointer updated.

```bash
git -C themes/brewgo add <files> && git -C themes/brewgo commit -m "..."
git add themes/brewgo && git commit -m "update brewgo submodule: ..."
```

Key theme layouts:
- `themes/brewgo/layouts/beers/single.html` — two-column beer detail (image left, specs right)
- `themes/brewgo/layouts/beers/list.html` — beer index table (title, BJCP style, ABV)
- `themes/brewgo/layouts/_default/single.html` — blog post layout
- `themes/brewgo/layouts/index.html` — homepage

CSS variables (defined in `themes/brewgo/static/css/main.css`):
```css
--midnight: #0f0b51       /* dark blue */
--flame-point: #fc7010    /* orange — primary brand color */
--soft-ember: #fca804     /* amber — links */
--pale-ale: #ffe9bf       /* cream */
--foam: #ffffff            /* white */
```

To override a theme layout, create the same path under `layouts/` in the project root.

## Hugo Configuration Highlights

- `mainSections = ["beers"]` — beers are the primary content type
- `params.since = "2019"` — used in footer copyright range
- Menu weights: About Us (1), Our Beers (2), Brewer's Notes (3)
- Comments, reading time, and word count are disabled
- Syntax highlighting: Pygments/trac style, using CSS classes
