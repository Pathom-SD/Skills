---
title: Compact Page Header
impact: MEDIUM
impactDescription: Scannable context without app chrome
tags: header, toggle, filter
---

## Compact Page Header

Header helps read summary data only — keep ~80–120px total height.

**Incorrect (app shell chrome):**

```html
<header>
  <input type="search" placeholder="Search..." />
  <button><!-- notifications --></button>
  <img src="avatar.png" />
</header>
```

**Correct:**

```html
<header class="mb-8 flex flex-wrap items-start justify-between gap-4">
  <div>
    <p class="text-sm text-[var(--text-muted)]">Zone A · Today</p>
    <h1 class="text-3xl font-bold">Operations Summary</h1>
    <p class="text-sm text-[var(--text-muted)]">Last updated 14:32</p>
  </div>
  <div class="flex shrink-0 items-center gap-3">
    <select id="period-filter"><!-- optional --></select>
    <div role="group"><!-- TH | EN --></div>
    <button id="theme-toggle" aria-label="..."><!-- sun/moon --></button>
  </div>
</header>
```

Include: title, one-line context, time/period, **language toggle**, **theme toggle**, optional single filter.

Never include unless asked: sidebar, breadcrumbs, profile menu, notification bell, global search.
