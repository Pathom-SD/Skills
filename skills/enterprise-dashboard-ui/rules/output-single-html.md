---
title: Single HTML File
impact: CRITICAL
impactDescription: Default deliverable for fast prototypes
tags: html, cdn, single-file
---

## Single HTML File

Default output: **one `.html` file** under `examples/` (or user path). Runs via double-click or static host — no npm.

**Incorrect:**

```html
<!-- Splitting into local siblings while staying "single-file" -->
<link rel="stylesheet" href="./styles.css" />
<script src="./app.js"></script>
```

**Correct:**

```html
<!-- All via CDN + inline blocks -->
<link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700&display=swap" rel="stylesheet" />
<script src="https://cdn.tailwindcss.com"></script>
<script src="https://cdn.jsdelivr.net/npm/chart.js@4/dist/chart.umd.min.js"></script>
<script src="https://unpkg.com/lucide@latest/dist/umd/lucide.min.js"></script>
<style>/* :root tokens */</style>
<script>/* mock data + charts */</script>
```

| Rule | Requirement |
|------|-------------|
| File count | One `.html` only |
| Data | Embed mock in `<script>` — no `fetch` unless user provides URL |
| Layout | Single page, full width, **no sidebar** |
| Theme | Light + dark required — `rules/theme-light-dark.md` |
| Language | TH + EN required — `rules/i18n-th-en.md` |
| Filename | kebab-case, e.g. `operations-summary.html` |

Tailwind config inline with `darkMode: ["class", '[data-theme="dark"]']`, brand primary `#da251c`.

Full CDN stack, skeleton, and Chart.js defaults: `references/reference.md`
