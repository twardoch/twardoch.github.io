# code.twardoch.com

Source for [code.twardoch.com](https://code.twardoch.com), built with
[Jekyll](https://jekyllrb.com/) and the
[Just the Docs](https://github.com/just-the-docs/just-the-docs) theme,
pulled in via
[`jekyll-remote-theme`](https://github.com/benbalter/jekyll-remote-theme).

The theme version is pinned on the `remote_theme:` line in
`_config.yml`. Bump it to upgrade.

## Local preview

```bash
cd docs
bundle install
bundle exec jekyll serve --livereload
```

## Deployment

Hosted on GitHub Pages from this folder. In **Settings → Pages**:

- **Source:** Deploy from a branch
- **Branch:** `master` / folder `/docs`

The custom domain (`code.twardoch.com`) comes from the `CNAME` file in
this folder.
