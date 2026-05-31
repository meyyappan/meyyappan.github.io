# Claude context

Personal ML research blog at meyyappan.github.io.

## Stack

- **Hugo** (v0.128.2 in CI, latest via nix locally) — static site generator
- **GitHub Actions** — builds and deploys on push to `master`
- No npm, no external CSS/JS frameworks, no CDN dependencies
- System font stack, CSS custom properties for theming

## Running Hugo locally

```bash
source ~/.bashrc   # ensure ~/bin is in PATH
hugo server        # preview at localhost:1313
hugo               # build to public/
```

`~/bin/hugo` is a shell wrapper: `nix --extra-experimental-features "nix-command flakes" run nixpkgs#hugo -- "$@"`

## Content

- Posts live in `content/posts/*.md`
- About page: `content/about.md`
- `draft: true` hides a post from the built site
- Paginate = 5 posts per page (set in `hugo.toml`)

## Styles

All CSS is in `static/css/main.css`. Theme switching is done via `data-theme` attribute on `<html>` (`dark` / `light`), persisted in `localStorage`. The anti-flash script is inlined in `layouts/partials/head.html`.

Syntax highlighting uses Hugo's built-in Chroma with the `dracula` style (inline, no separate CSS file needed).

## Templates

- `layouts/_default/baseof.html` — base shell
- `layouts/index.html` — homepage (paginates `posts` section)
- `layouts/_default/list.html` — section and taxonomy list pages
- `layouts/_default/single.html` — individual post/page
- `layouts/partials/` — head, header, footer, pagination

## Deployment

Push to `master` triggers `.github/workflows/deploy.yml`. Uses `peaceiris/actions-hugo@v3` to install Hugo extended, then `actions/upload-pages-artifact` + `actions/deploy-pages`.

GitHub Pages source must be set to **GitHub Actions** in repo settings.
