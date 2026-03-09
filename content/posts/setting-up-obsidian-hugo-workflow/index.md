---
title: "Setting Up an Obsidian to Hugo Publishing Workflow"
date: 2026-03-08
lastmod: 2026-03-08
draft: true
share: false
description: "How I set up Obsidian as a CMS for my Hugo blog with one-click publishing."
tags:
  - obsidian
  - hugo
  - workflow
  - tutorial
categories:
  - Tutorials
series:
  - Site Setup
cover:
  image: ""
  alt: ""
  caption: ""
ShowToc: true
TocOpen: true
---

## The Goal

I wanted a writing workflow that felt natural — write in Obsidian, use wiki-links freely, manage posts visually, and publish with a single click. No terminal commands, no manual file copying.

## The Stack

Here's what I landed on:

- **Obsidian** — for writing and organizing content
- **Bases CMS plugin** — visual card-based content management with bulk draft/publish toggling
- **GitHub Publisher plugin** — converts wiki-links and pushes content to GitHub
- **Hugo** — static site generator with the Chirpy theme
- **GitHub Actions** — automatic build on push
- **GitHub Pages** — free hosting

## How It Works

### Writing

I write posts as normal Obsidian notes. I use [[wiki-links]] to reference other posts, and the GitHub Publisher plugin converts these to standard Hugo links when publishing.

Each post lives in its own folder under `content/posts/` with an `index.md` file. Images go in the same folder.

### Managing Content

The Bases CMS plugin gives me a visual dashboard of all my posts. I can see thumbnails, tags, and draft status at a glance. Toggling a post from draft to published is just a checkbox click.

### Publishing

When I'm ready to publish, I hit `Cmd+Shift+P` (my custom hotkey for GitHub Publisher). The plugin:

1. Finds all notes where `share: true`
2. Converts wiki-links to standard markdown
3. Pushes the converted files to my GitHub repo
4. GitHub Actions picks up the push and builds the site

The site updates within about 90 seconds.

## Tips

- Keep a `_private` folder for notes you never want published — it's excluded in the GitHub Publisher config
- Use the `templates` folder for Obsidian templates — also excluded
- The `draft: true` frontmatter property tells Hugo to skip the post during builds, while `share: false` tells GitHub Publisher to skip it during uploads. Use both for work-in-progress content.
