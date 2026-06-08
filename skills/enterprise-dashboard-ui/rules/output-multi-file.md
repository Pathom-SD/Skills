---
title: Multi-File Folder
impact: CRITICAL
impactDescription: Maintainable structure when dashboards grow beyond ~400 lines
tags: html, modules, fetch, folder
---

## Multi-File Folder

Split when code is large, external data is needed, or features multiply. Still **no bundler** unless user asks.

**Incorrect (800-line monolith or unnecessary npm):**

```text
dashboard.html  (850 lines, fetch + export + filters inline)
package.json + vite.config.ts for a static dashboard prototype
```

**Correct:**

```text
fleet-summary/
├── index.html
├── css/styles.css
├── js/main.js, theme.js, i18n.js, charts.js, render.js, api.js
├── i18n/th.json, en.json    (optional)
├── data/mock.json           (optional)
└── assets/
```

| Rule | Requirement |
|------|-------------|
| Entry | `index.html` |
| JS | `<script type="module" src="./js/main.js">` — local `import` OK |
| CDN | Chart.js + Lucide as UMD globals in `index.html` |
| Theme/i18n | Same rules as single-file, split into `theme.js` + `i18n.js` |
| Local JSON | `fetch('./data/mock.json')` requires static server |

Serve folder when using `fetch` or ES modules:

```bash
npx --yes serve fleet-summary
```

`api.js` must handle loading, empty, and error UI states.

Full conventions: `references/reference.md#multi-file-folder`
