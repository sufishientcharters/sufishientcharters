# Sveltia CMS Integration — Design

**Date:** 2026-03-05
**Status:** Approved

## Goal

Add Sveltia CMS so that Matt (non-technical site owner) can edit site content through a browser-based GUI. Changes are committed back to GitHub, triggering an automatic Netlify redeploy.

## Decisions

| Question | Decision |
|---|---|
| CMS | Sveltia CMS (open source, Decap-compatible) |
| Authentication | Netlify Identity (invite-only) |
| GitHub write access | Netlify Git Gateway |
| Editorial workflow | None — publish immediately |
| Collections | File + Folder (Approach A) |
| Media uploads | Yes, to `static/images/` |

## Architecture

Sveltia CMS is a single-page app served from `static/admin/`. It loads from a CDN and is configured via `static/admin/config.yml`. No backend is required — Netlify Identity handles auth and Netlify Git Gateway handles GitHub commits.

**User flow:**
1. Matt visits `https://sufishientcharters.com/admin/`
2. Logs in with Netlify Identity credentials (email + password)
3. Edits content through a form-based GUI
4. Hits "Save" — Sveltia commits the updated markdown to GitHub
5. Netlify detects the push and rebuilds the site (~1 minute)

## Files Changed

### New files

- `static/admin/index.html` — loads Sveltia CMS from CDN
- `static/admin/config.yml` — collection and field definitions

### Modified files

- `netlify.toml` — add `POST /api/auth` redirect for Netlify Identity widget

## Collections

### Folder collections (add / edit / delete)

**Charters** (`content/charters/`)
- title (string)
- summary (string)
- heroHeading (string)
- heroSubHeading (string)
- heroBackground (image)
- icon (image)
- featured (boolean)
- weight (number)
- button (string)
- buttonLink (string)
- fareharborId (number)
- body (markdown)

**Reviews** (`content/reviews/`)
- author (string)
- permalink (string — Google Maps URL)
- featured (boolean)
- weight (number)
- body (markdown)

### File collections (edit only)

**Homepage**
- `content/_index.md` — heroHeading, heroSubHeading, heroBackground
- `content/homepage/welcome.md` — title, body, background image
- `content/homepage/about.md` — title, body, background image

**About**
- `content/about/captain.md` — title, body
- `content/about/boat.md` — title, body
- `content/about/why.md` — title, body

**Contact**
- `content/contact/_index.md` — title, body

**Book**
- `content/book/_index.md` — title, body

## Media

- Upload folder: `static/images/`
- Public path: `/images/`
- Matt gets a media browser when inserting images into any field

## Post-Deploy Steps (Netlify Dashboard)

1. Enable **Netlify Identity** on the site
2. Set registration to **Invite only**
3. Enable **Git Gateway** integration
4. Invite Matt by email
