# Changelog

All notable changes to this repository are recorded here. The format follows
[Keep a Changelog](https://keepachangelog.com/en/1.1.0/), and versions track the
git tags (`vX.Y.Z`).

## [Unreleased]

### Added
- GitHub Actions **Build** workflow that builds the Jekyll site and checks
  internal links on every push and pull request. It validates only — deployment
  still runs through GitHub Pages "Deploy from a branch".

### Changed
- Rewrote the README: clearer purpose line, a build-and-deploy section covering
  the new CI, and removed a dangling reference to a `4HUMAN.md` file that no
  longer exists.

### Fixed
- Stopped tracking the stray 1.5 MB `ruvector.db` binary and the local `.omc/`
  tooling state; both are now in `.gitignore`.

## [2.0.2] — 2026-06-28

- Site content and theme configuration for `code.twardoch.com`.

[Unreleased]: https://github.com/twardoch/twardoch.github.io/compare/v2.0.2...HEAD
[2.0.2]: https://github.com/twardoch/twardoch.github.io/releases/tag/v2.0.2
