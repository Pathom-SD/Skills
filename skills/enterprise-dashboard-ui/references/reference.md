# Enterprise Dashboard UI — Reference

Detailed design system reference. Use when implementing specific visual tokens, chart/table styling, or single-file HTML output.

> **Mode:** Single HTML file (default) → inline or `<style>`. Multi-file folder → `css/styles.css` + `js/*.js`. Next.js → `src/app/globals.css` tokens. Rule index: `../SKILL.md`.

---

## Single-file HTML

### Approved CDNs

| Purpose | CDN | Notes |
|---------|-----|-------|
| CSS utility | `https://cdn.tailwindcss.com` | In-browser compiler; fine for prototypes |
| Charts | `https://cdn.jsdelivr.net/npm/chart.js@4/dist/chart.umd.min.js` | Use `Chart` global |
| Icons | `https://unpkg.com/lucide@latest/dist/umd/lucide.min.js` | Call `lucide.createIcons()` after DOM |
| Fonts | `https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700&display=swap` | Primary typeface |
| Light interactivity (optional) | `https://cdn.jsdelivr.net/npm/alpinejs@3/dist/cdn.min.js` | Date filter, chart tabs — defer |

Do **not** use: React/Vue CDN + JSX (too heavy for one file), multiple chart libraries, or npm package paths.

### Chart.js defaults (premium minimal look)

Read colors from CSS variables so charts follow the active theme:

```javascript
function chartThemeColors() {
  const s = getComputedStyle(document.documentElement);
  return {
    text: s.getPropertyValue("--chart-text").trim() || "#6b7280",
    grid: s.getPropertyValue("--chart-grid").trim() || "rgba(0,0,0,0.06)",
    primary: s.getPropertyValue("--primary").trim() || "#da251c",
  };
}

function applyChartThemeDefaults() {
  const { text, grid } = chartThemeColors();
  Chart.defaults.font.family = "Inter, system-ui, sans-serif";
  Chart.defaults.color = text;
  Chart.defaults.borderColor = grid;
  Chart.defaults.plugins.legend.labels.boxWidth = 12;
  Chart.defaults.plugins.legend.labels.usePointStyle = true;
}

applyChartThemeDefaults();

// Example line chart dataset (build after applyChartThemeDefaults)
const { primary } = chartThemeColors();
{
  borderColor: primary,
  backgroundColor: "rgba(218, 37, 28, 0.12)",
  borderWidth: 2,
  tension: 0.4,
  fill: true,
  pointRadius: 0,
  pointHoverRadius: 4,
}

// Example bar chart
{
  borderRadius: 8,
  backgroundColor: primary,
  maxBarThickness: 48,
}

// Doughnut — cutout 70%+, soft palette (last slice = muted track)
{ cutout: "72%", backgroundColor: ["#da251c", "#22c55e", "#f59e0b", "#06b6d4", "#374151"] }
```

On `themechange`, re-run `applyChartThemeDefaults()` and `chartInstance.update()` for each chart.

Destroy/recreate charts on resize only if needed; prefer `responsive: true, maintainAspectRatio: false` inside fixed-height containers (`h-64`, `h-80`).

### Single-page summary layout (HTML)

No sidebar. Use a centered content column:

```html
<body class="min-h-screen bg-[var(--bg-main)] ...">
  <div class="mx-auto max-w-7xl px-4 py-6 sm:px-6 lg:px-8">
    <header><!-- title + period --></header>
    <section class="grid gap-4 sm:grid-cols-2 xl:grid-cols-4"><!-- KPI cards --></section>
    <section class="mt-8 grid gap-6 lg:grid-cols-3">
      <div class="lg:col-span-2"><!-- primary chart card --></div>
      <div><!-- donut / secondary --></div>
    </section>
    <section class="mt-8"><!-- summary table --></section>
  </div>
</body>
```

Grid breakpoints:

| Viewport | KPI grid | Chart grid |
|----------|----------|------------|
| Mobile | 1 col | 1 col stacked |
| `sm` | 2 cols | 1 col |
| `xl` | 4 cols KPI | `lg:grid-cols-3` (2+1) |

### Embedded mock data pattern

```html
<script>
  const MOCK = {
    meta: { title: "Operations Summary", period: "Today", updatedAt: "14:32" },
    kpis: [
      { label: "Active Units", value: "24", trend: "+12%", up: true },
      { label: "Tasks Completed", value: "1,842", trend: "+3%", up: true },
      { label: "Errors", value: "7", trend: "-2", up: false },
      { label: "Utilization", value: "87%", trend: "+5%", up: true },
    ],
    chartSeries: { labels: ["Mon", "Tue", "Wed"], values: [120, 190, 150] },
    breakdown: [
      { label: "Running", value: 18, color: "#22c55e" },
      { label: "Idle", value: 4, color: "#9ca3af" },
      { label: "Error", value: 2, color: "#ef4444" },
    ],
    summaryRows: [
      { id: "AMR-01", status: "running", metric: "142 tasks" },
    ],
  };
</script>
```

Render KPIs/tables from `MOCK` in the same script block — keeps HTML declarative and data in one place.

### What to avoid in single-file output

- Sidebar, `<aside>` navigation, or hamburger menu
- Splitting into `.css` / `.js` sibling files (use [multi-file folder](#multi-file-folder) instead)
- ES module `import` from CDN (use UMD globals: `Chart`, `lucide`)
- `fetch('./data.json')` on `file://` — move to multi-file + static server
- Hard dependency on build tools or TypeScript

---

## Multi-file folder

Use when the dashboard outgrows one HTML file — see SKILL.md triggers (size, external data, extra features).

### Recommended structure

```text
example/operations-summary/
├── index.html
├── css/
│   └── styles.css
├── js/
│   ├── main.js
│   ├── theme.js
│   ├── i18n.js
│   ├── charts.js
│   ├── render.js
│   └── api.js          # only if external data
├── i18n/
│   ├── th.json
│   └── en.json         # optional split when I18N is large
├── data/
│   └── mock.json       # optional static seed
└── assets/
```

### Module responsibilities

| File | Responsibility |
|------|----------------|
| `theme-init.js` | Inline in `<head>` — sync theme before paint (same as single-file) |
| `lang-init.js` | Inline in `<head>` — sync lang before paint |
| `styles.css` | `:root` / `[data-theme]` tokens, `prefers-reduced-motion`, print |
| `theme.js` | `chartThemeColors()`, `applyChartThemeDefaults()`, `themechange` listener |
| `i18n.js` | `I18N` dict or dynamic `fetch('./i18n/{lang}.json')`, `t()`, `applyLanguage()` |
| `charts.js` | Create/update/destroy Chart instances |
| `render.js` | `renderKpis`, `renderSummaryTable`, `renderStatusGrid` |
| `api.js` | `fetch` external API or `./data/*.json`; normalize response shape |
| `main.js` | `DOMContentLoaded` → load data → render → wire toggles |

### ES modules example

```javascript
// js/main.js
import { initToggles } from "./theme.js";
import { applyLanguage, loadI18n } from "./i18n.js";
import { buildCharts } from "./charts.js";
import { renderDashboard } from "./render.js";
import { loadDashboardData } from "./api.js";

document.addEventListener("DOMContentLoaded", async () => {
  await loadI18n();
  initToggles();
  try {
    const data = await loadDashboardData({ url: "./data/mock.json" });
    renderDashboard(data);
    buildCharts(data);
    applyLanguage();
    lucide.createIcons();
  } catch (err) {
    document.getElementById("error-banner")?.classList.remove("hidden");
  }
});
```

Chart.js and Lucide stay as **global UMD** scripts in `index.html`; app code uses `window.Chart` and `window.lucide`.

### Loading i18n from JSON

```javascript
// js/i18n.js
let I18N = {};

export async function loadI18n() {
  const lang = window.getDashboardLang();
  const res = await fetch(`./i18n/${lang}.json`);
  I18N = await res.json();
  window.t = (key) => I18N[key] ?? key;
}
```

On `langchange`, re-fetch or keep both `th.json` + `en.json` in memory.

### Static server (required for fetch / modules)

`file://` often blocks `fetch` and ES modules. Tell the user to serve the folder:

```bash
npx --yes serve example/operations-summary
# or
python -m http.server 8080
```

Add an HTML comment in `index.html` when `api.js` or JSON fetch is present.

### External API notes

- User must supply URL and auth headers if needed
- Implement: loading skeleton, error banner, retry button, empty state
- CORS must be enabled on the API or use a proxy the user provides
- Poll interval for live dashboards: 30–60s default; respect `document.visibilityState`

### Optional extra files

| Need | Add |
|------|-----|
| CSV export | `js/export.js` |
| Complex filters | `js/filters.js` |
| Many chart types | `js/charts/` directory |
| Shared utils | `js/utils.js` |

Do not add `package.json`, tests, or CI unless the user asks.

---

## Theme system (light / dark)

### HTML token table

| Token | Light | Dark |
|-------|-------|------|
| `--bg-main` | `#f5f7fb` | `#0f1117` |
| `--bg-card` | `#ffffff` | `#1a1d27` |
| `--bg-muted` | `#f9fafb` | `#252936` |
| `--border-subtle` | `#e5e7eb` | `#2d3344` |
| `--text-primary` | `#111827` | `#f3f4f6` |
| `--text-muted` | `#6b7280` | `#9ca3af` |
| `--primary` | `#da251c` | `#ef4444` |
| `--shadow-card` | `0 4px 24px rgba(0,0,0,0.06)` | `0 4px 24px rgba(0,0,0,0.35)` |
| `--chart-grid` | `rgba(0,0,0,0.06)` | `rgba(255,255,255,0.06)` |
| `--chart-text` | `#6b7280` | `#9ca3af` |

Define light tokens on `:root` / `[data-theme="light"]` and dark on `[data-theme="dark"]`. Set `color-scheme: light|dark` on the active theme block for native form controls.

### Chart.js theme listener

```javascript
const chartInstances = []; // push each new Chart(...) here

document.addEventListener("themechange", () => {
  applyChartThemeDefaults();
  chartInstances.forEach((c) => c.update());
});
```

### Status badge contrast (dark mode)

| Status | Light classes | Dark-friendly |
|--------|---------------|---------------|
| Success | `bg-green-100 text-green-800` | `bg-green-500/15 text-green-400` |
| Warning | `bg-amber-100 text-amber-800` | `bg-amber-500/15 text-amber-400` |
| Danger | `bg-red-100 text-red-800` | `bg-red-500/15 text-red-400` |
| Info | `bg-slate-100 text-slate-700` | `bg-slate-500/15 text-slate-300` |
| Idle | `bg-gray-100 text-gray-600` | `bg-gray-500/15 text-gray-400` |

Prefer semantic tokens over fixed `bg-green-100` when possible.

### Next.js / Corp Brain

- Wrap app with `ThemeProvider` (`next-themes`, `attribute="class"`, `defaultTheme="system"`, `enableSystem`)
- Page header: reuse shadcn `Button` variant `outline` + sun/moon icons from `lucide-react`
- Surfaces: `bg-background`, `bg-card`, `text-foreground`, `text-muted-foreground`, `border-border` — never page-level hardcoded `bg-white` / `bg-gray-900`
- Charts (Recharts / Chart.js in client components): read `hsl(var(--muted-foreground))` or CSS variables; subscribe to theme via `useTheme()` from `next-themes`

---

## i18n system (TH / EN)

### HTML pattern

| Item | Value |
|------|-------|
| Storage key | `dashboard-lang` |
| Values | `th` \| `en` |
| HTML attr | `<html lang="th">` or `lang="en"` |
| Event | `langchange` — detail `{ lang }` |
| Helper | `window.t("key")` reads `I18N[activeLang][key]` |

### What must translate

- Page title, header, section headings, KPI labels, trend footnotes
- Chart dataset labels, axis tooltips, donut legend
- Table column headers, status badges, filter options
- Footer, `aria-label` on theme/lang/period controls
- Mock task descriptions in summary rows (store per-locale in `I18N` or `MOCK[lang]`)

### What stays locale-neutral

- IDs (`AMR-01`), zone codes (`Zone A`), timestamps (`14:32`)
- Raw metrics (`1,842`, `87%`) — optionally format: `n.toLocaleString(lang === "th" ? "th-TH" : "en-US")`

### data-i18n markup (static strings)

```html
<h1 data-i18n="pageTitle"></h1>
<p><span data-i18n="lastUpdated"></span> <span id="last-updated">14:32</span></p>
```

```javascript
function applyI18nDom() {
  document.querySelectorAll("[data-i18n]").forEach((el) => {
    el.textContent = window.t(el.dataset.i18n);
  });
}
```

### MOCK with locale branches

```javascript
const DATA = {
  kpis: [
    { key: "kpiOnline", value: "24", trend: "+12%", trendKey: "vsYesterday" },
  ],
  summaryRows: [
    { id: "AMR-01", zone: "Zone A", status: "running", time: "14:31",
      task: { th: "ขนส่ง A-12 → B-04", en: "Transport A-12 → B-04" } },
  ],
};
```

### langchange + charts

```javascript
document.addEventListener("langchange", () => {
  applyI18nDom();
  buildCharts(); // labels from window.t()
});
```

### Next.js / Corp Brain

- Use existing project i18n (`next-intl`, JSON message files under `messages/th.json` + `messages/en.json`)
- `LanguageToggle` in summary page header — same visual as HTML segmented control
- Chart components: pass translated labels as props; re-render when locale changes via `useLocale()`
- Do not hardcode Thai or English in JSX — use `t("dashboard.kpi.online")` or equivalent

---

## Color system (generic reference)

### Primary

```css
--primary: #5b8def;
--primary-dark: #4f46e5;
--primary-light: #8fb4ff;
```

Corp Brain mapping: `--primary` / `--brand-primary`, `--primary-foreground`.

### Accent / semantic

```css
--success: #22c55e;
--warning: #f59e0b;
--danger: #ef4444;
--info: #06b6d4;
```

Corp Brain mapping: use Tailwind semantic classes and `destructive` for danger states.

### Background

Light:

```css
--bg-main: #f5f7fb;
--bg-card: #ffffff;
--bg-soft: #f9fafb;
```

Dark (HTML `data-theme="dark"`):

```css
--bg-main: #0f1117;
--bg-card: #1a1d27;
--bg-soft: #252936;
```

Corp Brain mapping: `bg-background`, `bg-card`, `bg-muted` (automatic per `.dark` class).

### Text

```css
--text-primary: #111827;
--text-secondary: #6b7280;
--text-muted: #9ca3af;
```

Corp Brain mapping: `text-foreground`, `text-muted-foreground`.

---

## Typography

Preferred fonts: Inter, SF Pro Display, Poppins (Corp Brain uses project `--font-sans`).

| Role | Size / Weight |
|------|---------------|
| Page Title | 32px / Bold |
| Section Title | 20px / SemiBold |
| Card Title | 16px / Medium |
| Body | 14px / Regular |
| Caption | 12px / Medium |

Tailwind examples: `text-3xl font-bold`, `text-xl font-semibold`, `text-base font-medium`, `text-sm`, `text-xs font-medium`.

---

## Card system

- Card background via `var(--bg-card)` (HTML) or `bg-card` (Next.js) — **not** fixed white
- Radius ~20px (`rounded-2xl` / `rounded-card`), soft shadow via `var(--shadow-card)` or `shadow-sm`, padding 20–32px
- Border: `border-[var(--border-subtle)]` or `border-border`

### Design token object (adapt to CSS variables)

```js
export const theme = {
  radius: {
    card: "20px",
    button: "16px",
  },
  colors: {
    primary: "#5B8DEF",
    success: "#22C55E",
    danger: "#EF4444",
  },
  shadows: {
    card: "0 8px 24px rgba(99,102,241,0.08)",
  },
};
```

In Corp Brain, derive from `globals.css` instead of duplicating hex values.

---

## Chart system

Charts must look premium, minimal, modern, spacious. Never clutter.

### Bar charts

- Rounded corners, gradient or primary fills, soft grid lines, spacing between bars

### Line charts

- Smooth curve, subtle glow, modern tooltip, animated transitions (respect `prefers-reduced-motion`)

### Donut charts

- Thick clean ring, centered info, soft colors using `--chart-1` through `--chart-5`

---

## Table system

- Remove harsh borders; use `border-border` sparingly
- Hover highlight on rows
- Spacious rows, clean typography
- Status badges as pills (see SKILL.md)

---

## Iconography

- **HTML:** Lucide UMD — `<i data-lucide="layout-dashboard"></i>` then `lucide.createIcons()`
- **Next.js:** `lucide-react`, line icons, consistent stroke width
- Avoid filled icons and mixed icon packs

---

## Responsive system

| Breakpoint | Behavior |
|------------|----------|
| Desktop | Full-width `max-w-7xl`, KPI 4-col, chart 2+1 grid |
| Tablet | KPI 2-col, charts stack or 2-col |
| Mobile | Single column; KPI 1–2 col; tables horizontal scroll if needed |

No sidebar at any breakpoint. Summary content stacks vertically.

---

## Animation system

Use only:

- Smooth hover, soft transitions, slight lift effects, chart animation

Avoid:

- Flashy motion, heavy parallax, distracting animation

Corp Brain: use `motion` / Animate UI sparingly; honor reduced motion.

---

## Tech stack reference

**Standalone HTML (default output):**

```text
Single .html file
Tailwind Play CDN, Chart.js CDN, Lucide UMD CDN, Google Fonts CDN
Vanilla JS (optional Alpine.js CDN)
```

**Corp Brain integration (when user asks for src/):**

```text
Next.js 16, React 19, TypeScript, Tailwind CSS 4,
shadcn/ui, motion, lucide-react
```

Add React/chart npm packages only when integrating into the repo, not for HTML prototypes.

---

## AI consistency booster

When attaching a reference image:

```text
Follow the exact same visual language, spacing rhythm, hierarchy,
card structure, and single-page summary composition from the reference —
full-width data layout, no sidebar, KPI-first.
```
