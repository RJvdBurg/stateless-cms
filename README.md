# Stateless CMS

A **zero-build, single-file CMS** plus a visual **Theme Builder**. Everything runs entirely
in the browser as a GitHub REST API client + Google Gemini client — no backend, no build step,
no framework. Hosted on GitHub Pages. Built to still work in 10 years.

## Use it for any site

Open the CMS pointed at a content repo:

```
https://rjvdburg.github.io/stateless-cms/?repo=OWNER/REPO
```

- Your **GitHub token** and **Gemini key** live only in your browser's `localStorage`
  (shared across every site you edit) — they are never written to any repo.
- **Per-site CMS settings** come from `cms.config.json` in the content repo:

```json
{
  "siteName": "My Site",
  "siteUrl": "https://example.com",
  "branch": "main",
  "imgDir": "images",
  "ga4": "G-XXXXXXX",
  "model": "gemini-flash-latest",
  "autoJsonld": true,
  "themeBase": "https://OWNER.github.io/THEME-REPO/",
  "sysPrompt": "House style / SEO instructions for the AI co-pilot."
}
```

## What's in the topbar

| Button | Does |
|--------|------|
| ➕ **Nieuwe site** | Wizard: create a new repo + theme (clone the neutral `starter-theme` or reuse one), scaffold the site and enable Pages |
| 🖼 **Afbeeldingen** | Image manager — lists repo images, optimises (resize + WebP/quality) and replaces them |
| 🎨 **Theme Builder** | Opens `theme-builder.html` — visually edit `site.json` + the theme's colours/fonts with a live preview, then save & bake |
| 🧱 **Platte HTML** | Bakes every page into flat, self-contained HTML (see below) |
| ⚙ **Instellingen** | GitHub token, repo, Gemini key, model, GA4, AI system prompt |

Plus: browse/edit/commit any file, live HTML/Markdown preview with click-to-edit, an AI
co-pilot (Gemini) for rewriting selected text, auto JSON-LD and GA4 injection.

## Flat-HTML baking

The design lives in a **theme** (a separate repo: `theme.css` + `theme.js` + `site.json`), but
the published pages are **flat, self-contained HTML**: the baker inlines `theme.css`, bakes the
header/footer/WhatsApp + a small behaviour script into each page, and removes all external theme
references. Delimited by `<!-- CMS:THEMECSS|HEADER|CHROME:START/END -->` markers, so re-baking is
idempotent and each page's `<main>` content is preserved.

- **Edit content** → edit the page in the CMS and publish.
- **Change design / menu / footer / logo** → use the **Theme Builder** (or hand-edit
  `site.json` / `theme.css`), then **🧱 Platte HTML** to re-bake every page.

The chrome is defined once in `theme.js` (pure builders `window.Theme.headerOuterHTML/…` +
`bakePage`), used by both the runtime and the baker — one source, no drift.

The token needs **Contents: Read & Write** on the target site repo (and, to save theme colours
from the Theme Builder, on the theme repo too). Fine-grained PAT.

## Starting a new site

The **➕ Nieuwe site** wizard creates a fresh website end-to-end: it makes a new content repo, either clones the neutral [`starter-theme`](https://github.com/RJvdBurg/starter-theme) into a new theme repo or reuses an existing theme, scaffolds `cms.config.json` + `site.json` + flat starter pages + a placeholder logo, enables GitHub Pages, and opens the new site in the CMS.

> **Token note:** creating repos needs a token that is allowed to create repositories — a classic PAT with `repo` scope, or a fine-grained token with account-level repo-creation. A fine-grained PAT scoped only to existing repos cannot create new ones; the wizard reports this clearly.

## Files
- `index.html` — the CMS engine
- `theme-builder.html` — the visual theme / `site.json` editor

Related repos: [`starter-theme`](https://github.com/RJvdBurg/starter-theme) (neutral base cloned by the wizard).
