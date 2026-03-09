# Hughes Co — Site Setup Guide

This guide covers the manual steps needed to get your Obsidian + Hugo + GitHub Pages workflow running.

## Prerequisites

- [Hugo Extended](https://gohugo.io/installation/) installed locally (`brew install hugo` on Mac)
- [Git](https://git-scm.com/) installed
- [Obsidian](https://obsidian.md/) installed
- A GitHub account

## Step 1: Create the GitHub Repository

```bash
# From this project folder:
cd /path/to/hghs_co_CMS

git init
git branch -M main
```

Then go to https://github.com/new and create a new repository. Suggested name: `hghs-co` or `yourusername.github.io` (for a user site).

```bash
git remote add origin https://github.com/YOUR_USERNAME/YOUR_REPO_NAME.git
```

## Step 2: Install the Hugo Theme

The site uses the [Mana](https://github.com/Livour/hugo-mana-theme) theme — a futuristic dark theme with clean aesthetics:

```bash
git submodule add https://github.com/Livour/hugo-mana-theme.git themes/mana
```

If you switch themes, update the `theme` value in `hugo.toml`.

## Step 3: Test Hugo Locally

```bash
hugo server -D
```

Visit http://localhost:1313 to see your site. The `-D` flag includes draft posts.

## Step 4: Configure GitHub Pages

1. Push your initial commit:
   ```bash
   git add .
   git commit -m "Initial site scaffold"
   git push -u origin main
   ```

2. Go to your repo on GitHub → **Settings** → **Pages**
3. Under **Build and deployment**, set Source to **GitHub Actions**
4. The workflow file (`.github/workflows/hugo-deploy.yml`) is already in place — it will trigger on the next push to `main`

## Step 5: Open as an Obsidian Vault

1. Open Obsidian
2. Click **Open folder as vault**
3. Select this project folder (`hghs_co_CMS`)
4. When prompted about community plugins, enable them

## Step 6: Install Obsidian Plugins

You need to install these two plugins from the Obsidian Community Plugins browser:

### Bases CMS
1. Go to Settings → Community Plugins → Browse
2. Search for "Bases CMS" by David V. Kimball
3. Install and enable it
4. **Also enable** the core "Bases" plugin: Settings → Core Plugins → toggle on "Bases"

### GitHub Publisher
1. Go to Settings → Community Plugins → Browse
2. Search for "GitHub Publisher" by Mara-Li
3. Install and enable it
4. Go to Settings → GitHub Publisher → GitHub Configuration:
   - **GitHub username**: your GitHub username
   - **GitHub repository**: your repo name
   - **Branch**: `main`
   - **GitHub token**: Click "Create token" to generate a Personal Access Token with `repo` scope
5. The rest of the settings are pre-configured in `.obsidian/plugins/obsidian-github-publisher/data.json`

## Step 7: Update Configuration

Edit `hugo.toml` and replace:
- `baseURL` — your GitHub Pages URL (e.g., `https://yourusername.github.io/repo-name/`)
- Social links — uncomment and fill in your profiles

Edit `.obsidian/plugins/obsidian-github-publisher/data.json` and replace:
- `REPLACE_WITH_YOUR_GITHUB_USERNAME` — your GitHub username
- `REPLACE_WITH_YOUR_REPO_NAME` — your repository name

## How to Use

### Daily Writing Workflow

1. **Create a new post**: Make a new folder in `content/posts/` with an `index.md` file, or duplicate the template from `templates/new-post.md`
2. **Write freely**: Use Obsidian wiki-links `[[like this]]`, they'll be converted automatically
3. **Manage content visually**: Open `content/posts/Posts Dashboard.base` and switch to the CMS view to see all posts as cards

### Publishing a Post

1. In the post's frontmatter, set:
   - `draft: false` (tells Hugo to include it in the build)
   - `share: true` (tells GitHub Publisher to upload it)
2. Press `Cmd+Shift+P` (or `Ctrl+Shift+P` on Windows) to publish the current note
3. Or press `Cmd+Shift+Alt+P` to publish ALL shared notes at once
4. The site rebuilds automatically — check your GitHub Actions tab for build status

### Keeping Posts as Drafts

- `draft: true` + `share: false` = work in progress (not uploaded, not built)
- `draft: true` + `share: true` = uploaded to repo but not visible on site (useful for previewing in CI)
- `draft: false` + `share: true` = live on the site

### Private Notes

Put anything in the `_private/` folder — it's gitignored and excluded from GitHub Publisher.

## Folder Structure

```
hghs_co_CMS/
├── .github/workflows/    # GitHub Actions (auto-deploy)
├── .obsidian/            # Obsidian vault config + plugins
├── _private/             # Your private notes (gitignored)
├── archetypes/           # Hugo content templates
├── assets/images/        # Site-wide images
├── content/
│   ├── pages/            # Static pages (about, etc.)
│   └── posts/            # Blog posts (each in its own folder)
│       └── Posts Dashboard.base  # Bases CMS view
├── data/                 # Hugo data files
├── layouts/              # Hugo layout overrides
├── static/               # Static files (copied as-is)
├── templates/            # Obsidian templates for new posts
├── themes/               # Hugo themes (git submodule)
├── hugo.toml             # Hugo configuration
├── .gitignore            # Git ignore rules
└── SETUP.md              # This file
```

## Troubleshooting

**Hugo build fails**: Run `hugo server -D --verbose` locally to see detailed errors.

**Wiki-links not converting**: Ensure GitHub Publisher's conversion settings have `wiki: true` under `conversion.links` in the plugin config.

**Posts not appearing**: Check that both `draft: false` AND `share: true` are set in frontmatter.

**GitHub Pages not updating**: Check the Actions tab in your GitHub repo for build errors. The workflow triggers on pushes to `main`.

**Theme not loading**: Make sure you ran `git submodule add` and the theme folder exists in `themes/`. After cloning, run `git submodule update --init --recursive`.
