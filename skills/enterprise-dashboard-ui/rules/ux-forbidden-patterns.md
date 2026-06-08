---
title: Forbidden Patterns
impact: MEDIUM
impactDescription: Avoid anti-patterns that break enterprise dashboard UX
tags: ux, anti-patterns, quality
---

## Forbidden Patterns

Act as Senior Product Designer + Staff Frontend Engineer. Target feel: **Professional · Modern · Clean · Premium · Data-focused**.

**Never:**

- Sidebar, drawer menu, or left navigation column
- App-shell chrome (profile, global search, notifications) on summary pages
- Overcrowded layouts, thick borders, neon/cyberpunk, heavy glassmorphism
- Overuse gradients or random accent colors
- Multiple font families or outdated admin templates
- Hardcoded `#fff` / `#111` when theme tokens exist
- Monolithic files >400 lines — split to multi-file folder

**Always:**

- Surface key metrics immediately (KPI row above the fold)
- Reduce cognitive load; grouping over navigation
- Read-only summary first; minimal actions
- Respect `prefers-reduced-motion`
- `truncate` + `min-w-0` on flex text; `shrink-0` on interactive elements

Design DNA: Stripe metrics, Vercel analytics, Supabase overviews — scannable in seconds.
