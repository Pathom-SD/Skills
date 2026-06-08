---
title: Light and Dark Themes
impact: HIGH
impactDescription: Every dashboard ships with both themes and persistence
tags: theme, dark-mode, css-variables
---

## Light and Dark Themes

**Every HTML dashboard must include light and dark modes** with a header toggle.

**Incorrect (hardcoded surfaces):**

```html
<body class="bg-gray-50">
  <div class="bg-white border border-gray-100 shadow-md">
```

**Correct (CSS variables + data-theme):**

```html
<script>
  (function initTheme() {
    const key = "dashboard-theme";
    const root = document.documentElement;
    const saved = localStorage.getItem(key);
    const prefersDark = matchMedia("(prefers-color-scheme: dark)").matches;
    const theme = saved === "light" || saved === "dark" ? saved : prefersDark ? "dark" : "light";
    root.setAttribute("data-theme", theme);
    root.classList.toggle("dark", theme === "dark");
    window.setDashboardTheme = (next) => {
      root.setAttribute("data-theme", next);
      root.classList.toggle("dark", next === "dark");
      localStorage.setItem(key, next);
      document.dispatchEvent(new CustomEvent("themechange", { detail: { theme: next } }));
    };
  })();
</script>
```

| Rule | Value |
|------|-------|
| Storage | `dashboard-theme` → `light` \| `dark` |
| Toggle | Sun/moon button, top-right, `aria-label` |
| Charts | On `themechange`: re-read `--chart-grid` / `--chart-text`, call `chart.update()` |

Card surfaces: `bg-[var(--bg-card)] border-[var(--border-subtle)] shadow-[var(--shadow-card)]`

Token table: `references/reference.md#theme-system-light--dark`
