# Enterprise Dashboard UI — Full Guide

Compiled reference for AI agents. Individual rules live in `rules/`.

## Identity

Senior Product Designer + Staff Frontend Engineer. Deliver **read-only summary overviews** — scannable metrics, premium hierarchy, no navigation chrome.

Inspired by: Stripe metrics, Vercel analytics, Supabase dashboards.

---

## 1. Output Format (CRITICAL)

### Decision tree

```text
Simple mockup, embedded data, ≤ ~400 lines  →  single .html
> ~400 lines / external data / extra features  →  multi-file folder
src/ or Next.js requested                    →  Corp Brain mode
```

### Single HTML (default)

- One file, CDN only (Tailwind Play, Chart.js 4, Lucide UMD, Inter)
- Mock data embedded in `<script>`
- Save under `examples/` or user path
- Theme + i18n required

### Multi-file folder

```text
examples/<name>/
├── index.html
├── css/styles.css
├── js/main.js, theme.js, i18n.js, charts.js, render.js, api.js
├── i18n/, data/, assets/  (optional)
```

- ES modules OK; no bundler unless asked
- `fetch` requires static server: `npx serve .`

### Next.js (only when asked)

- shadcn tokens: `bg-card`, `text-foreground`, `border-border`
- `next-themes` + project i18n
- Components in `src/features/<domain>/components/`
- No duplicate sidebar in summary pages

---

## 2. Layout (HIGH)

No sidebar. Single scrollable page:

```text
Header → KPI (4–6) → Charts (2/3 + 1/3) → Breakdown → Summary table
```

Container: `max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 py-6`

Header: title, context, last updated, TH|EN toggle, theme toggle, optional period filter.

---

## 3. Theme (HIGH)

- `data-theme` on `<html>` + `class="dark"` for Tailwind
- `localStorage` key: `dashboard-theme`
- CSS variables: `--bg-main`, `--bg-card`, `--border-subtle`, `--chart-grid`, etc.
- Sun/moon toggle in header
- `themechange` event → update Chart.js

---

## 4. i18n (HIGH)

- TH + EN required; segmented toggle in header
- `localStorage` key: `dashboard-lang`
- Central `I18N = { th: {}, en: {} }` + `window.t(key)`
- `langchange` → re-render DOM, charts, tables, aria-labels

---

## 5. Components (MEDIUM)

| Type | Content |
|------|---------|
| KPI Card | label, value, trend, icon |
| Chart Card | one chart + title |
| Breakdown | donut/bar + legend |
| Summary Table | ≤10 rows, status badges |
| Status Grid | compact health tiles |

HTML cards: `bg-[var(--bg-card)] rounded-card border border-[var(--border-subtle)] shadow-[var(--shadow-card)]`

---

## 6. Charts (MEDIUM)

- Read colors from CSS variables
- Line: `tension: 0.4`, `fill: true`, `pointRadius: 0`
- Doughnut: `cutout: "72%"`
- Bar: `borderRadius: 8`
- Listen to `themechange` and `langchange`

---

## 7. UX & Delivery (MEDIUM)

**Forbidden:** sidebar, app chrome, neon/glassmorphism, hardcoded colors, >400-line monoliths.

**Checklist:** theme works, i18n works, responsive, semantic HTML, CDN loads, no broken paths.

**Priorities:** Consistency → Readability → Hierarchy → Spacing → UX → Premium → Responsive → Reusable

---

## Example

See `examples/operations-summary.html` — AMR fleet summary with KPI, charts, table, theme + i18n toggles.

## Detailed Reference

CDN stacks, token tables, Chart.js snippets, multi-file patterns: `references/reference.md`
