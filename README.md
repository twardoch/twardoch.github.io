# twardoch.github.io

Source for **[code.twardoch.com](https://code.twardoch.com)** — Adam Twardoch's
notes and projects on type design, font technology, OpenType, variable fonts
and Unicode.

The whole site lives in [`docs/`](./docs). It is a [Jekyll](https://jekyllrb.com/)
site using the [Just the Docs](https://github.com/just-the-docs/just-the-docs)
theme, loaded through
[`jekyll-remote-theme`](https://github.com/benbalter/jekyll-remote-theme). The
theme version is pinned on the `remote_theme:` line in
[`docs/_config.yml`](./docs/_config.yml) — bump it there to upgrade.

## Build and deploy

Just the Docs rides on GitHub's built-in `github-pages` gem, so no custom
deploy workflow is needed. The site publishes with the legacy **Deploy from a
branch** build: **Settings → Pages → Source =** branch `master`, folder
`/docs`. The custom domain comes from [`docs/CNAME`](./docs/CNAME).

The [Build workflow](./.github/workflows/build.yml) does not deploy — it builds
the site and checks internal links on every push and pull request, so a broken
build never reaches `master`.

## Local preview

```bash
cd docs
bundle install
bundle exec jekyll serve --livereload
```

Then open <http://localhost:4000>. See [`docs/README.md`](./docs/README.md) for
more.
