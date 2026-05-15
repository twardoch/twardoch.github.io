---
title: Just the Docs on GitHub Pages
parent: Guides
nav_order: 1
---

# Add Just the Docs to any repo (Pages from `/docs`)
{: .no_toc }

A compact recipe for dropping a [Just the Docs] site into the `/docs`
folder of any GitHub repo and publishing it through the built-in
"Deploy from a branch" mode — no GitHub Actions workflow needed.

1. TOC
{:toc}

## 1. Create the files

```bash
mkdir -p docs && cd docs
```

### `docs/_config.yml`

```yaml
title: My Project
description: One-line description.
url: https://USER.github.io          # or your custom domain

remote_theme: just-the-docs/just-the-docs@v0.12.0
color_scheme: light                  # or dark
search_enabled: true

aux_links:
  GitHub: https://github.com/USER/REPO
aux_links_new_tab: true

# Edit-on-GitHub link in each page footer
gh_edit_link: true
gh_edit_link_text: Edit this page on GitHub
gh_edit_repository: https://github.com/USER/REPO
gh_edit_branch: main                 # or master
gh_edit_source: docs
gh_edit_view_mode: tree

plugins:
  - jekyll-seo-tag
  - jekyll-sitemap
  - jekyll-remote-theme

exclude: [Gemfile, Gemfile.lock, README.md, vendor/, node_modules/]
```

### `docs/Gemfile`

```ruby
source "https://rubygems.org"
gem "github-pages", group: :jekyll_plugins
```

### `docs/index.md`

```markdown
---
title: Home
layout: home
nav_order: 1
---

# My Project
Welcome.
```

### `docs/.gitignore`

```
_site/
.jekyll-cache/
.jekyll-metadata
.sass-cache/
vendor/
.bundle/
Gemfile.lock
```

## 2. (Optional) Custom domain

Add `docs/CNAME` with one line: `docs.example.com`. Point a CNAME at
`USER.github.io` in your DNS — on Cloudflare, leave the proxy off (grey
cloud), or GitHub can't issue the Let's Encrypt certificate.

## 3. Enable Pages

GitHub repo → **Settings → Pages**:

- **Source:** Deploy from a branch
- **Branch:** `main` (or `master`) — **folder `/docs`**

Push, wait ~1 minute, the site appears at
`https://USER.github.io/REPO/` (or your CNAME).

## 4. Add pages

Drop more `.md` files in `docs/`. Sidebar order is set in front matter:

```yaml
---
title: Installation
nav_order: 2
---
```

Nest a page under a parent:

```yaml
---
title: Quick start
parent: Installation
nav_order: 1
---
```

## 5. Local preview (optional)

```bash
cd docs
bundle install
bundle exec jekyll serve --livereload
```

## 6. Upgrade the theme

Bump the version pinned on the `remote_theme:` line in `_config.yml`.
That's it.

## Why this works without GitHub Actions

`jekyll-remote-theme` is on GitHub Pages' [plugin allowlist], and Just
the Docs is built to run on the Jekyll 3.10 that the `github-pages` gem
still ships. No workflow file, no plugin-gem juggling — push and it
publishes.

## What you get for free from `github-pages`

The `github-pages` gem is a meta-gem that pins exact versions of Jekyll,
its renderers, and a curated set of plugins. The authoritative manifest
is [`pages.github.com/versions.json`][versions]. Anything in there is
preinstalled in the Pages build environment; anything outside that list
is silently stripped from your `plugins:` block.

### Core build stack

| Component | Version | What it does |
| --- | --- | --- |
| `ruby` | 3.3.4 | The Ruby runtime your build executes on. |
| `jekyll` | 3.10.0 | The static-site generator. **Frozen at 3.x** — themes/plugins that require Jekyll 4 (e.g. Chirpy) won't run. |
| `github-pages` | 232 | The meta-gem itself; this number is what you'd see in [GitHub's dependency dashboard]. |
| `liquid` | 4.0.4 | The template language inside {% raw %}`{% %}`/`{{ }}`{% endraw %} tags. |
| `kramdown` | 2.4.0 | Default Markdown renderer for `.md` files (used unless you set `markdown: CommonMarkGhPages`). |
| `kramdown-parser-gfm` | 1.1.0 | GitHub-Flavoured Markdown parser plugged into kramdown. |
| `jekyll-commonmark-ghpages` | 0.5.1 | Opt-in CommonMark renderer that matches `github.com` rendering of `README.md`. |
| `rouge` | 3.30.0 | Syntax highlighter for fenced code blocks. |
| `jekyll-sass-converter` | 1.5.2 | Compiles `.scss`/`.sass` → `.css`. |
| `sass` | 3.7.4 | Pinned Sass implementation (LibSass-era, not Dart Sass). |
| `safe_yaml` | 1.0.5 | YAML parser used to read front matter without arbitrary code execution. |
| `nokogiri` | 1.16.7 | HTML/XML parser that several plugins use internally. |
| `html-pipeline` | 2.14.3 | Pipeline of filters (sanitisation, link rewriting, emoji, etc.) used by `jemoji` and `jekyll-mentions`. |
| `github-pages-health-check` | 1.18.2 | Backs the Pages "Your site is published at…" DNS/HTTPS check shown in *Settings → Pages*. |

### Plugins (enable in `_config.yml` under `plugins:`)

| Plugin | Version | What it adds | Enable / config |
| --- | --- | --- | --- |
| `jekyll-feed` | 0.17.0 | Atom feed at `/feed.xml`. | `plugins: [jekyll-feed]` |
| `jekyll-sitemap` | 1.4.0 | `/sitemap.xml` listing every page. | `plugins: [jekyll-sitemap]` |
| `jekyll-seo-tag` | 2.8.0 | `<title>`, `<meta>` description, Open Graph, Twitter Card, JSON-LD. Put {% raw %}`{% seo %}`{% endraw %} in `<head>`. | `plugins: [jekyll-seo-tag]` |
| `jekyll-redirect-from` | 0.16.0 | Per-page `redirect_from:` front matter generates stub HTML pages with `<meta refresh>`. | Add `redirect_from: [/old/path/]` to a post. |
| `jekyll-remote-theme` | 0.4.3 | Lets you set `remote_theme: user/repo@ref` instead of a theme gem. | `remote_theme: just-the-docs/just-the-docs@v0.12.0` |
| `jekyll-include-cache` | 0.2.1 | Caches {% raw %}`{% include %}`{% endraw %} results across pages — silent speed-up that big themes (Minimal Mistakes, Just the Docs) rely on. | Just enable it. |
| `jekyll-paginate` | 1.1.0 | Old-style numbered paginator for `index.html`. Limited (one collection only, no nested paths). | `paginate: 5` + `paginate_path: "/page:num/"` |
| `jekyll-gist` | 1.5.0 | {% raw %}`{% gist user/id %}`{% endraw %} embeds a Gist. | Inline tag. |
| `jekyll-avatar` | 0.8.0 | {% raw %}`{% avatar user %}`{% endraw %} → GitHub avatar `<img>` with sensible defaults. | Inline tag. |
| `jemoji` | 0.13.0 | Converts `:smile:` style emoji shortcodes to images. | `plugins: [jemoji]` |
| `jekyll-mentions` | 1.6.0 | Auto-links `@username` to `https://github.com/username`. | `plugins: [jekyll-mentions]` |
| `jekyll-coffeescript` | 1.2.2 | `.coffee` files compile to `.js`. Legacy — modern projects ship `.js` directly. | `plugins: [jekyll-coffeescript]` |
| `jekyll-github-metadata` | 2.16.1 | Exposes `site.github.*` (repo URL, default branch, owner, contributors, etc.). Auto-enabled on Pages. | Read via {% raw %}`{{ site.github.repository_url }}`{% endraw %}. |
| `jekyll-relative-links` | 0.6.1 | Rewrites relative Markdown links (`[x](other.md)`) to their built URLs so the same `README.md` works on `github.com` and on the published site. | Auto-enabled (opt out with `relative_links: { enabled: false }`). |
| `jekyll-optional-front-matter` | 0.3.2 | Processes `.md` files **without** front matter (otherwise Jekyll ignores them). Lets a top-level `README.md` render as a page. | Auto-enabled. |
| `jekyll-readme-index` | 0.3.0 | Uses `README.md` as the index page of a directory when no `index.md` is present. | Auto-enabled. |
| `jekyll-default-layout` | 0.1.5 | Assigns `layout: page` (or `post`) by default so you can omit `layout:` from front matter. | Auto-enabled. |
| `jekyll-titles-from-headings` | 0.5.3 | If a page has no `title:` in front matter, uses its first `# Heading` as the title. | Auto-enabled. |

**Auto-enabled** plugins above are loaded by `github-pages` whether or
not you list them in `plugins:`; the others must be listed explicitly.

### Bundled themes (set as `theme:` or `remote_theme:`)

`minima` (2.5.1) is Jekyll's default. The `jekyll-theme-*` family —
`primer`, `architect`, `cayman`, `dinky`, `hacker`, `leap-day`,
`merlot`, `midnight`, `minimal`, `modernist`, `slate`, `tactile`,
`time-machine` — are the themes offered by the **Theme Chooser** in
*Settings → Pages*. `jekyll-swiss` (1.0.0) is a typography-focused
alternative. All are pinned at the versions above; any other theme must
be loaded via `remote_theme:`.

### What is *not* there (common surprises)

- **`jekyll-archives`** — tag/category index pages. Not allowlisted, so
  Chirpy and similar themes break on legacy Pages builds.
- **Jekyll 4.x** — Pages is stuck on 3.10. Themes built for 4.x Liquid
  tags (`where_exp` improvements, `find`, etc.) may fail or misrender.
- **Dart Sass / modern Sass syntax** — `sass` 3.7.4 is LibSass-era and
  rejects `@use` / `@forward`. Stick to the old `@import` syntax.
- **Custom Ruby plugins (`_plugins/*.rb`)** — Pages runs in
  `--safe` mode, which disables them. If you need a custom plugin,
  switch to a GitHub Actions build.
- **Any gem not in the manifest** — silently ignored. Run
  `bundle exec github-pages versions` locally before deploying to
  confirm what will actually load.

[Just the Docs]: https://just-the-docs.com
[plugin allowlist]: https://pages.github.com/versions/
[versions]: https://pages.github.com/versions.json
[GitHub's dependency dashboard]: https://pages.github.com/versions/
