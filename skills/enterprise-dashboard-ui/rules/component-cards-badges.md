---
title: Cards, Badges, and Buttons
impact: MEDIUM
impactDescription: Consistent enterprise component vocabulary
tags: components, kpi, badges, cards
---

## Cards, Badges, and Buttons

**Incorrect (inconsistent card styles):**

```html
<div class="bg-white rounded-lg shadow p-3 border-2 border-black">
<div class="rounded-md bg-slate-100 p-2">
```

**Correct (HTML mode):**

```html
<article class="rounded-card border border-[var(--border-subtle)] bg-[var(--bg-card)] p-6 shadow-[var(--shadow-card)]">
  <span class="text-sm text-[var(--text-muted)]">Units Online</span>
  <p class="text-3xl font-bold">24</p>
</article>
```

| Card type | Purpose |
|-----------|---------|
| KPI Card | Label, large number, trend %, icon |
| Chart Card | One chart + short title |
| Breakdown Card | Donut/bar + legend (top 3–5) |
| Summary Table | 5–10 rows, status badge |
| Status Grid | Compact health tiles |

Status badges (HTML): pill with semantic colors — success green, warning amber, danger red, idle gray. Dark mode: `bg-green-500/15 text-green-400` pattern.

Buttons: primary `bg-primary text-white rounded-xl`; secondary uses card/muted tokens. No gradient buttons unless asked.

Naming (Next.js): `KpiCard`, `ChartCard`, `SummaryTable`, `StatusBadge`.
