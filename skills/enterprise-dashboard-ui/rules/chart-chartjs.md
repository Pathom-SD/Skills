---
title: Chart.js Patterns
impact: MEDIUM
impactDescription: Premium minimal charts that follow theme
tags: chartjs, charts, data-viz
---

## Chart.js Patterns

Charts must look premium, minimal, and follow active theme colors.

**Incorrect (hardcoded chart colors, no theme listener):**

```javascript
Chart.defaults.color = "#6b7280";
new Chart(ctx, {
  data: { datasets: [{ borderColor: "#da251c", backgroundColor: "#fff" }] }
});
// never updates on themechange
```

**Correct (read CSS variables + listen):**

```javascript
function chartThemeColors() {
  const s = getComputedStyle(document.documentElement);
  return {
    text: s.getPropertyValue("--chart-text").trim(),
    grid: s.getPropertyValue("--chart-grid").trim(),
    primary: s.getPropertyValue("--primary").trim(),
  };
}

document.addEventListener("themechange", () => {
  applyChartThemeDefaults();
  chartInstances.forEach((c) => c.update());
});
```

Defaults: Inter font, smooth line (`tension: 0.4`), doughnut `cutout: "72%"`, bar `borderRadius: 8`, `responsive: true, maintainAspectRatio: false` in fixed-height containers (`h-64`, `h-72`).

On `langchange`, rebuild chart labels from `window.t()`.

Full defaults: `references/reference.md#chartjs-defaults`
