# Hughes Co — Site Setup Guide

This guide covers how the Obsidian + Hugo + GitHub Pages workflow is set up and how to use it.

## Prerequisites

- [Hugo Extended](https://gohugo.io/installation/) installed locally (`brew install hugo` on Mac)
- [Git](https://git-scm.com/) installed
- [Obsidian](https://obsidian.md/) installed
- A GitHub account with SSH key configured

## Site Architecture

- **Hugo** — static site generator
- **Mana theme** — dark, clean aesthetic ([repo](https://github.com/Livour/hugo-mana-theme))
- **Obsidian** — writing and content management
- **Enveloppe plugin** — publishes notes from Obsidian to GitHub (converts wiki-links)
- **GitHub Actions** — builds the site automatically on push
- **GitHub Pages** — hosts the site at https://rchrdhghs.github.io/hghs-co/

## Folder Structure

```
hghs_co_CMS/
├── .github/workflows/    # GitHub Actions (auto-deploy)
├── .obsidian/            # Obsidian vault config + plugins
├── _private/             # Your private notes (gitignored)
├── archetypes/           # Hugo content templates (used by `hugo new`)
├── assets/               # Site-wide assets (images, etc.)
├── content/
│   ├── pages/            # Static pages (about, etc.)
│   └── posts/            # Blog posts (each in its own folder)
├── data/                 # Hugo data files
├── layouts/              # Hugo layout overrides
├── static/               # Static files (copied as-is to site root)
├── templates/            # Obsidian templates for new posts
├── themes/mana/          # Hugo theme (git submodule)
├── hugo.toml             # Hugo configuration
├── .gitignore            # Git ignore rules
└── SETUP.md              # This file
```

## Images

Images go **in the same folder as the post's `index.md`**:

```
content/posts/my-post/
├── index.md
├── screenshot.png
└── diagram.jpg
```

Reference them in your post with a relative path:

```markdown
![Alt text](screenshot.png)
```

For site-wide images (favicon, logo, etc.), use the `assets/` or `static/` folder.

## Creating a New Post

### Option 1: From Obsidian

1. Create a new folder in `content/posts/` (e.g., `my-new-post`)
2. Inside it, create `index.md`
3. Copy frontmatter from `templates/new-post.md`

### Option 2: From the terminal

```bash
hugo new posts/my-new-post/index.md
```

This uses the archetype in `archetypes/posts.md`.

## Frontmatter Reference

```yaml
---
title: "My Post Title"
date: 2026-03-08
lastmod: 2026-03-08
draft: true          # true = excluded from production build
share: false         # true = Enveloppe will push to GitHub
description: "A short summary"
tags: [tag1, tag2]
categories: [Category]
series: [Series Name]
---
```

### Draft / Publish States

| `draft` | `share` | Result |
|---------|---------|--------|
| `true`  | `false` | Work in progress — local only |
| `true`  | `true`  | Pushed to repo but not visible on live site |
| `false` | `true`  | **Live on the site** |
| `false` | `false` | Built locally but not pushed by Enveloppe |

## Publishing a Post

1. Set `draft: false` and `share: true` in frontmatter
2. Press `Cmd+Shift+P` to publish the current note via Enveloppe
3. Or press `Cmd+Shift+Alt+P` to publish ALL shared notes at once
4. GitHub Actions builds and deploys automatically (~90 seconds)

## Local Preview

```bash
hugo server -D
```

Visit http://localhost:1313 — the `-D` flag includes draft posts.

## Obsidian Plugin: Enveloppe

The Enveloppe plugin (formerly "GitHub Publisher") handles pushing notes to GitHub:

- **Install**: Settings → Community Plugins → Browse → search "Enveloppe"
- **Configure**: Settings → Enveloppe → GitHub Configuration:
  - GitHub username: `rchrdhghs`
  - GitHub repository: `hghs-co`
  - Branch: `main`
  - GitHub token: generate a Personal Access Token with `repo` scope

The plugin converts `[[wiki-links]]` to standard markdown links when publishing.

## Private Notes

Put anything in the `_private/` folder — it's gitignored and excluded from Enveloppe.

## Custom Domain (Optional)

To use a custom domain like `hghs.co`:

1. In your DNS provider (e.g., Cloudflare), add:
   - 4x A records for `@` pointing to `185.199.108.153`, `185.199.109.153`, `185.199.110.153`, `185.199.111.153`
   - CNAME record: `www` → `rchrdhghs.github.io`
   - Set proxy to **DNS only** (grey cloud) if using Cloudflare
2. In GitHub repo → Settings → Pages, enter your custom domain
3. Update `baseURL` in `hugo.toml` to `https://hghs.co/`
4. Check "Enforce HTTPS"

Until the custom domain is set up, the site is at `https://rchrdhghs.github.io/hghs-co/`.

## Troubleshooting

**Hugo build fails**: Run `hugo server -D --verbose` locally to see detailed errors.

**Wiki-links not converting**: Check Enveloppe settings — ensure link conversion is enabled.

**Posts not appearing**: Check that both `draft: false` AND `share: true` are set in frontmatter.

**GitHub Pages not updating**: Check the Actions tab in your GitHub repo for build errors. The workflow triggers on pushes to `main`.

**Theme not loading**: Run `git submodule update --init --recursive` to pull the theme files.
