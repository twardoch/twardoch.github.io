# twardoch.github.io

Source for [code.twardoch.com](https://code.twardoch.com).

The site lives in [`docs/`](./docs) and is built with Jekyll + the
[Just the Docs](https://github.com/just-the-docs/just-the-docs) theme,
pulled in via
[`jekyll-remote-theme`](https://github.com/benbalter/jekyll-remote-theme).

Just the Docs is compatible with GitHub's built-in `github-pages` gem,
so the site is published using the **legacy "Deploy from a branch"**
build (Settings → Pages → Source = `master` / folder = `/docs`). No
custom GitHub Actions workflow is needed.

See [`docs/README.md`](./docs/README.md) for local preview.

`4HUMAN.md` documents the Cloudflare DNS fix that was needed to get
HTTPS working on the custom domain.
