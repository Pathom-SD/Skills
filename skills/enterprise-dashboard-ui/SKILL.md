---
name: enterprise-dashboard-ui
description: >-
  Enterprise single-page summary dashboards — KPI cards, charts, tables.
  Default one HTML file; escalates to multi-file folder or Next.js when complex.
  Tailwind CDN, Chart.js, light/dark themes, TH/EN i18n, no sidebar.
  Use for analytics summaries, operations overviews, AMR/industrial boards, HTML prototypes.
license: MIT
metadata:
  author: skills
  version: "1.0.0"
---

# Enterprise Dashboard UI

Enterprise summary dashboard framework for AI agents. Contains rules across 7 categories for output format, layout, themes, i18n, components, charts, and delivery quality.

## When to Apply

Reference these guidelines when:

- Building or redesigning **single-page summary dashboards** (no sidebar)
- Creating KPI cards, charts, compact tables, status overview blocks
- Designing industrial/AMR/warehouse status screens
- Generating standalone HTML mockups or multi-file dashboard folders
- Integrating dashboard pages into Next.js / Corp Brain (only when asked)

## Rule Categories by Priority

| Priority | Category | Impact | Prefix |
|----------|----------|--------|--------|
| 1 | Output Format | CRITICAL | `output-` |
| 2 | Layout & Structure | HIGH | `layout-` |
| 3 | Theme System | HIGH | `theme-` |
| 4 | Internationalization | HIGH | `i18n-` |
| 5 | Components | MEDIUM | `component-` |
| 6 | Charts & Data | MEDIUM | `chart-` |
| 7 | UX & Delivery | MEDIUM | `ux-` |

## Quick Reference

### 1. Output Format (CRITICAL)

- `output-format-decision` - Single HTML vs multi-file folder vs Next.js
- `output-single-html` - One-file CDN stack, rules, skeleton
- `output-multi-file` - Folder layout, ES modules, external data
- `output-nextjs` - Corp Brain / shadcn integration

### 2. Layout & Structure (HIGH)

- `layout-summary-page` - Page zones, generation order, no sidebar
- `layout-header` - Compact header with toggles and optional filter

### 3. Theme System (HIGH)

- `theme-light-dark` - CSS variables, toggle, persistence, chart updates

### 4. Internationalization (HIGH)

- `i18n-th-en` - TH/EN toggle, I18N object, langchange re-render

### 5. Components (MEDIUM)

- `component-cards-badges` - KPI/chart/table cards, badges, buttons

### 6. Charts & Data (MEDIUM)

- `chart-chartjs` - Theme-aware Chart.js defaults and listeners

### 7. UX & Delivery (MEDIUM)

- `ux-forbidden-patterns` - Anti-patterns and design DNA
- `ux-delivery-checklist` - Pre-handoff verification

## How to Use

Read individual rule files for detailed explanations and code examples:

```
rules/output-format-decision.md
rules/theme-light-dark.md
rules/i18n-th-en.md
```

Each rule contains incorrect vs correct examples.

## Supporting Resources

| Resource | Purpose |
|----------|---------|
| `AGENTS.md` | Full compiled guide |
| `references/reference.md` | CDN stack, tokens, Chart.js, multi-file conventions |
| `examples/operations-summary.html` | Working AMR fleet dashboard sample |

## Quick Prompt Anchor

```text
Single-page summary dashboard, no sidebar.
Default: one HTML file; multi-file folder if >~400 lines, external data, or extra features.
CDN: Tailwind, Chart.js, Lucide, Inter. Light/dark + TH/EN toggles.
KPI row → charts → breakdown → summary table. Primary #da251c.
```
