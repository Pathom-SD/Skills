---
title: Choose Output Format
impact: CRITICAL
impactDescription: Wrong format causes unmaintainable monoliths or over-engineered builds
tags: output, architecture, html, nextjs
---

## Choose Output Format

Pick the deliverable shape before writing code. Default to the simplest option that fits.

**Incorrect (always one giant file or always Next.js):**

```text
User asks for a quick KPI mockup → deliver 900-line HTML monolith
User asks for API-connected dashboard → cram fetch + 12 features in one file
User asks for HTML prototype → scaffold full Next.js app unprompted
```

**Correct (decision tree):**

```text
Simple mockup, embedded data, ≤ ~400 lines  →  single .html file
> ~400 lines, external data, extra features  →  multi-file folder (index.html + css/ + js/)
Production app in src/                      →  Next.js (Corp Brain) — only when asked
```

| Trigger | Action |
|---------|--------|
| Embedded mock data only | Single HTML |
| `fetch` to API or local JSON | Multi-file folder |
| Export, polling, complex filters | Multi-file folder |
| User says `src/features/` or Next.js | Corp Brain mode |

No bundler unless the user explicitly requests npm/Vite/webpack.

Reference: `rules/output-single-html.md`, `rules/output-multi-file.md`, `rules/output-nextjs.md`
