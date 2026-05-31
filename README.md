# meyyappan.github.io

Personal blog. Built with Hugo, deployed to GitHub Pages.

## New post

```bash
hugo new posts/my-post-title.md
# edit content/posts/my-post-title.md
```

Front matter:
```yaml
---
title: "Post title"
date: 2026-05-30
draft: false
tags: ["ml", "transformers"]
description: "One line summary shown in post list."
---
```

Set `draft: true` to hide from the site.

## Preview locally

```bash
hugo server
# open http://localhost:1313
```

Drafts are hidden by default. To preview them:
```bash
hugo server -D
```

## Deploy

Push to `master` — GitHub Actions builds and deploys automatically.

```bash
git add content/posts/my-post-title.md
git commit -m "new post: my post title"
git push
```

Live in ~1 minute. Watch the build: https://github.com/meyyappan/meyyappan.github.io/actions

## Structure

```
content/
  posts/       ← blog posts (markdown)
  about.md     ← about page
static/
  css/main.css ← all styles (dark/light mode via CSS variables)
layouts/       ← HTML templates
hugo.toml      ← site config (title, paginate, etc.)
```

## Hugo not found?

```bash
source ~/.bashrc   # adds ~/bin to PATH
```

`~/bin/hugo` is a wrapper around `nix run nixpkgs#hugo`. First run downloads Hugo; subsequent runs use the nix cache.
