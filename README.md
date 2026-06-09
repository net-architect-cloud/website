# Net Architect — netarchitect.cloud

Personal tech blog and showcase by Kevin Allioli (cloud architecture, OpenStack,
open-source infrastructure). Built with **Hugo** on top of a customized
[hugo-clarity](https://github.com/chipzoller/hugo-clarity) theme.

## Requirements

- **Hugo extended ≥ 0.161** (the templates use APIs from 0.158+: `hugo.Data`,
  `site.Language.Locale`). The extended build is required for the SASS pipeline.
- Go ≥ 1.24 — the theme is pulled as a **Hugo Module** (`go.mod`), not vendored.

## Run

```bash
hugo server -D          # local dev server with drafts, live reload
hugo --gc --minify      # production build into ./public
hugo new posts/AAAA-MM-JJ-mon-titre.md
```

There is no unit-test suite. "Done" for a content/layout change is a clean
`hugo --gc --panicOnWarning` (zero deprecation warnings) plus a visual check in
both light and dark mode.

## Deploy

Auto-deployed by **Cloudflare Pages** from `main` (build command: `hugo`).

> [!IMPORTANT]
> **Pin the Hugo version in Cloudflare.** Set the environment variable
> `HUGO_VERSION=0.161.1` in the Pages project (Settings → Variables, for both
> *Production* and *Preview*). Cloudflare's default Hugo (0.147.x) is too old for
> the templates and the build fails with `can't evaluate field Data in type
> interface {}` / `field Locale`. Keep this in sync with the local Hugo version.

## Architecture

- `content/` — Markdown. Posts in `content/posts/` (`YYYY-MM-DD-slug.md`), French,
  YAML front matter (`thumbnail`, `abstract`, `url`, `tags`, `categories`).
- `layouts/` — local overrides that shadow the theme (the home is a fully custom
  `index.html`; other pages use clarity). See `CLAUDE.md` for the override map and
  the deprecation shims targeting Hugo 0.161.
- `assets/sass/` — `_override.sass` (theme variables) and `_custom.sass` (custom
  styles incl. the home redesign and self-hosted JetBrains Mono).
- `static/` — fonts (Metropolis, JetBrains Mono), icons, images.
- `config/_default/`, `hugo.toml`, `i18n/fr.toml` — configuration and UI strings.

Comments use [giscus](https://giscus.app); structured data (JSON-LD) is enabled
via `params.blogDir`.
