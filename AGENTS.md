# Sample AGENTS.md file

## Tech Stack
- Hugo Extended (required — theme uses SCSS via `toCSS`)
- Node.js (managed via asdf)
- Netlify deployment
- Theme: custom "hero" theme

## Hugo Extended
Hugo still maintains separate extended and non-extended builds. The extended build includes Dart Sass support and WebP encoding. The hero theme uses `toCSS` for SCSS compilation, so **extended is required**.

The `extended_` prefix is still valid and necessary in `.tool-versions` for asdf. If `asdf install` returns a 404 for Hugo, the fix is to update the asdf Hugo plugin (`asdf plugin update hugo`), not to remove the `extended_` prefix.

## Version History
- Upgraded from `hugo extended_0.147.3` + `nodejs 22.15.1` to `hugo extended_0.157.0` + `nodejs 24.14.0` (March 2026)
- Versions pinned in: `.tool-versions` and `netlify.toml` (`HUGO_VERSION`)

## Hugo Front Matter: Build Options
Hugo 0.145.0 renamed `_build` to `build` in front matter. Use `build:` (no underscore).

## Completed Deprecation Fixes (March 2026)
- `headless: true` → `build: {render: never, list: never}` in `content/homepage/index.md`
- `$.Scratch` / `Page.Scratch` → `Page.Store` in 4 shortcode/template files
- `$.Scratch` → `$.Store` in `themes/hero/layouts/photos/photos.html`

## Remaining Modernization (Phase 3, deferred)
- LibSass via `toCSS` in `themes/hero/layouts/_default/baseof.html` — Dart Sass migration (longer term, low urgency)