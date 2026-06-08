---
title: Delivery Checklist
impact: MEDIUM
impactDescription: Verify dashboard before handoff
tags: checklist, qa, delivery
---

## Delivery Checklist

Verify before delivering any dashboard.

**Layout & structure**

- [ ] No sidebar or app navigation
- [ ] Header, KPI row, charts, summary table present
- [ ] Responsive: KPI stacks on mobile; charts full width
- [ ] Semantic HTML (`header`, `main`/`section`); `lang` set

**Theme**

- [ ] Light and dark work; toggle persists (`dashboard-theme`)
- [ ] Surfaces use CSS variables; readable in both modes
- [ ] Charts update on `themechange`

**i18n**

- [ ] TH and EN work; toggle persists (`dashboard-lang`)
- [ ] Charts/tables/badges re-render on `langchange`
- [ ] All strings from `I18N` — no orphaned copy

**Technical**

- [ ] CDN loads (network required)
- [ ] No broken imports; multi-file uses static server if `fetch` needed
- [ ] Mock data renders; numbers readable at a glance

**Output priorities:** Consistency → Readability → Hierarchy → Spacing → UX clarity → Premium feel → Responsiveness → Reusability
