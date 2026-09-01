# Stateless CMS

A **zero-build, single-file CMS**. One `index.html` that runs entirely in the browser as a
GitHub REST API client + Google Gemini client. No backend, no build step, no framework —
built to still work in 10 years. Hosted on GitHub Pages.

## Use it for any site

Open the CMS pointed at a content repo:

```
https://rjvdburg.github.io/stateless-cms/?repo=OWNER/REPO
```

- Your **GitHub token** and **Gemini key** live only in your browser's `localStorage`
  (shared across every site you edit) — they are never written to any repo.
- **Per-site settings** come from a `cms.config.json` in the content repo's root:

```json
{
  "siteName": "My Site",
  "siteUrl": "https://example.com",
  "branch": "main",
  "imgDir": "images",
  "ga4": "G-XXXXXXX",
  "model": "gemini-flash-latest",
  "autoJsonld": true,
  "sysPrompt": "House style / SEO instructions for the AI co-pilot."
}
```

## Features
- Browse, edit and commit any file in the repo (UTF-8-safe base64).
- Live HTML/Markdown preview with click-to-edit and in-preview image replace + reposition.
- Image Manager: optimises images in-browser (resize + WebP/quality) before committing.
- AI co-pilot (Gemini): rewrite/improve selected text, auto JSON-LD, GA4 injection.

The token needs **Contents: Read & Write** on the target repo (fine-grained PAT).
