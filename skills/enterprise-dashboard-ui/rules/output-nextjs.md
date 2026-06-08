---
title: Next.js Integration
impact: HIGH
impactDescription: Production integration into Corp Brain app shell
tags: nextjs, shadcn, corp-brain
---

## Next.js Integration

Switch to Next.js **only when** the user wants code in `src/features/` or `src/app/`.

**Incorrect (copying HTML hex palette into Next.js):**

```tsx
<div className="bg-[#f5f7fb]">
  <div className="bg-white border border-gray-100 rounded-2xl">
```

**Correct (shadcn tokens from globals.css):**

```tsx
<div className="bg-card rounded-2xl border border-border shadow-sm p-6">
  <p className="text-foreground">...</p>
  <p className="text-muted-foreground">...</p>
```

| Rule | Do |
|------|-----|
| Colors | `bg-background`, `bg-card`, `text-foreground`, `border-border`, `bg-primary` |
| Brand | `--brand-primary` / `--primary` (red) — not blue/purple |
| Components | shadcn/ui; icons from `lucide-react` |
| Layout | Full-width summary inside `SidebarInset` — **no duplicate sidebar** |
| Theme | `next-themes` + `ThemeToggle` in header |
| Language | Project i18n (`next-intl`) + `LanguageToggle` in header |
| Stack | Next.js 16 App Router, React 19, TypeScript, Tailwind CSS 4 |

Place components under `src/features/<domain>/components/`.
