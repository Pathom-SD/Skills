---
title: Thai and English i18n
impact: HIGH
impactDescription: Bilingual dashboards with persisted language preference
tags: i18n, thai, english, localization
---

## Thai and English i18n

**Every HTML dashboard must support TH and EN** with a segmented header toggle.

**Incorrect (hardcoded single language):**

```html
<h1>สรุปปฏิบัติการ</h1>
<span class="badge">กำลังทำงาน</span>
<!-- chart labels baked in Thai only -->
```

**Correct (central I18N + langchange):**

```javascript
window.I18N = {
  th: { pageTitle: "สรุปปฏิบัติการ", statusRunning: "กำลังทำงาน" },
  en: { pageTitle: "Operations Summary", statusRunning: "Running" },
};

window.t = (key) => I18N[getDashboardLang()]?.[key] ?? key;

document.addEventListener("langchange", () => {
  applyI18nDom();
  renderKpis();
  buildCharts();
});
```

| Rule | Value |
|------|-------|
| Storage | `dashboard-lang` → `th` \| `en` |
| Default | `navigator.language` starts with `th` → `th`, else `en` |
| Toggle | Segmented `TH \| EN` next to theme toggle |
| HTML | Set `<html lang="th|en">`; update `<title>` on switch |
| Dynamic UI | Re-render charts, tables, badges on `langchange` |

Use stable enum keys (`running`, `warning`, `error`, `idle`) mapped through `I18N`.

Full patterns: `references/reference.md#i18n-system-th--en`
