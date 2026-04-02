# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a static, AI-first knowledge endpoint for Italian singer-songwriter **Arnaldo Furioso** — a machine-readable artist page that serves both humans and AI systems. No build tools, no dependencies, no framework.

## Local Development

Serve the site with any static file server:

```bash
python -m http.server 8000
# then open http://localhost:8000
```

There are no build steps, tests, or package managers.

## Architecture

### Dual-layer content system

All artist data is maintained in two places that must stay in sync:

1. **`arnaldo-furioso-schema.json`** — the canonical structured data source (Schema.org `@graph` with `Person`, `MusicRecording`, `VideoObject`, `CreativeWorkSeries` entities, cross-linked via `urn:arnaldo-furioso:*` identifiers)
2. **`index.html`** — hardcoded HTML cards that mirror the schema data for visual display; a client-side `fetch()` also renders the raw JSON into the "SCHEMA.ORG MIRROR" panel at the bottom of the page

When adding or updating a release, both files must be edited manually.

### Adding a new song/video

1. Add `MusicRecording` and/or `VideoObject` entries to `arnaldo-furioso-schema.json`
2. Add the cover image to `img/covers/`
3. Add lyrics PDF and press release PDF to `docs/` (naming pattern: `0N-lyrics-slug.pdf`, `0N-press-release-slug.pdf`)
4. Add the corresponding HTML card(s) to `index.html` following the existing patterns

### Entity identifiers

All schema entities use URN identifiers of the form `urn:arnaldo-furioso:<type>-<slug>` (e.g. `urn:arnaldo-furioso:track-ahi`, `urn:arnaldo-furioso:video-plastica`). These link schema entries to HTML cards semantically.

### Image conventions

- Cover art in `img/covers/` is grayscale by default, colorized on hover (CSS `filter` effect)
- Platform icons (Spotify, Apple Music, etc.) live in `img/`
- OpenGraph / favicon assets live in `imgmeta/`

## Key Files

| File | Role |
|------|------|
| `index.html` | Single-page UI + embedded CSS + fetch script |
| `arnaldo-furioso-schema.json` | Schema.org structured data (source of truth for metadata) |
| `ai-policy.txt` | Machine-usage policy (referenced in `<head>` via `rel="policy"`) |
| `docs/` | 12 PDFs: lyrics + press releases, one pair per track |
| `img/covers/` | One cover image per song |
