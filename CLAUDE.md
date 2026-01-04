# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a Jekyll-based personal website/blog for Prabin Paudel, a software engineer. The site is hosted on GitHub Pages and contains blog posts, coaching materials, and personal information.

## Architecture

- **Jekyll Static Site Generator**: Uses Jekyll with the Minima theme (dark skin)
- **Content Structure**:
  - `_posts/`: Blog posts in Markdown format with date-based naming (YYYY-MM-DD-title.md)
  - `coaching/`: Coaching materials and guides (system design, algorithms, leadership)
  - Root-level pages: about.md, reading.md, writing.md, etc.
  - `_site/`: Generated static site files (do not edit directly)

## Common Commands

### Development
```bash
# Install dependencies
bundle install

# Serve site locally for development
bundle exec jekyll serve

# Build site for production
bundle exec jekyll build
```

### Content Management
- Blog posts go in `_posts/` with format: `YYYY-MM-DD-title.md`
- All content pages use Markdown with Jekyll front matter
- Images should be placed in `assets/images/` (referenced in generated site)

## Site Configuration

- Main config: `_config.yml`
- Theme: `jekyll/minima` (remote theme)
- Navigation pages defined in `header_pages` config
- Google Analytics: G-GF8R9BG98L
- RSS feed available at `/writing/writing.atom`

## Content Guidelines

- Blog posts require Jekyll front matter with layout, title, date, and categories
- Coaching materials are organized by topic (system design, algorithms, behavioral leadership)
- All external links should use full URLs
- Social media links configured in `_config.yml` under `minima.social_links`