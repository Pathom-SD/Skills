# Enterprise Dashboard UI

Structured skill for creating enterprise summary dashboards — optimized for agents and LLMs.

## Structure

- `SKILL.md` — Agent entry point (concise index)
- `rules/` — Individual rule files (one per topic)
  - `_sections.md` — Section metadata
  - `_template.md` — Template for new rules
- `references/reference.md` — Detailed design system (CDN, tokens, charts)
- `AGENTS.md` — Full compiled guide
- `metadata.json` — Document metadata

## Install

```bash
npx skills add https://github.com/Pathom-SD/Skills --skill enterprise-dashboard-ui
```

## Creating a New Rule

1. Copy `rules/_template.md` to `rules/area-description.md`
2. Use prefix: `output-`, `layout-`, `theme-`, `i18n-`, `component-`, `chart-`, `ux-`
3. Include incorrect vs correct examples
4. Update `SKILL.md` quick reference if adding a new rule

## Acknowledgments

Structure inspired by [vercel-labs/agent-skills](https://github.com/vercel-labs/agent-skills/tree/main/skills/react-best-practices).
