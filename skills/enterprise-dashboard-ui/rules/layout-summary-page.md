---
title: Summary Page Layout
impact: HIGH
impactDescription: Data-first single-page structure without app chrome
tags: layout, kpi, charts, no-sidebar
---

## Summary Page Layout

**No sidebar. No app shell. One scrollable page.**

**Incorrect:**

```html
<aside class="w-64"><!-- nav menu --></aside>
<main>...</main>
```

**Correct (top-down hierarchy):**

```text
Page Header → KPI row (4–6) → Primary charts (2/3 + 1/3) → Breakdown → Summary table
```

```html
<div class="mx-auto max-w-7xl px-4 py-6 sm:px-6 lg:px-8">
  <header>...</header>
  <section class="grid gap-4 sm:grid-cols-2 xl:grid-cols-4"><!-- KPI --></section>
  <section class="grid gap-6 lg:grid-cols-3">
    <div class="lg:col-span-2"><!-- primary chart --></div>
    <div><!-- donut --></div>
  </section>
  <section><!-- summary table --></section>
</div>
```

Page generation order:

1. Header (title, period, last updated)
2. KPI row
3. Primary analytics
4. Secondary breakdown
5. Summary table / status grid (5–10 rows max)
6. Optional footer (data source hint)

Industrial/AMR mode: fleet online, tasks completed, errors, utilization + status grid + throughput chart.

Reference: `rules/layout-header.md`
