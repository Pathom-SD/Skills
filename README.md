# Skills

A collection of [Agent Skills](https://agentskills.io/) for AI coding agents — modular instructions that extend agent capabilities for specialized tasks.

## Available Skills

### enterprise-dashboard-ui

Enterprise single-page summary dashboards — KPI cards, charts, summary tables. HTML prototypes with Tailwind CDN, Chart.js, light/dark themes, TH/EN i18n. Escalates to multi-file folder or Next.js when complexity grows.

**Use when:**

- Building analytics summaries, operations overviews, or executive dashboards
- Creating industrial/AMR/warehouse status boards
- Generating standalone HTML dashboard mockups (no build step)

## Installation

Install all skills from this repository:

```bash
npx skills add https://github.com/Pathom-SD/Skills
```

Install a specific skill:

```bash
npx skills add https://github.com/Pathom-SD/Skills --skill enterprise-dashboard-ui
```

List available skills without installing:

```bash
npx skills add https://github.com/Pathom-SD/Skills --list
```

Browse and discover more skills at [skills.sh](https://skills.sh/).

## Repository Structure

```text
skills/
└── enterprise-dashboard-ui/
    ├── SKILL.md           # Agent entry point (concise index)
    ├── AGENTS.md          # Full compiled guide
    ├── metadata.json      # Skill metadata
    ├── rules/             # Individual rules (one file per topic)
    ├── references/        # Detailed design system reference
    └── examples/          # Sample dashboard output
```

Each skill follows the [Vercel agent-skills](https://github.com/vercel-labs/agent-skills/tree/main/skills/react-best-practices) pattern: a short `SKILL.md` that links to focused rule files under `rules/`.

## Creating a New Skill

```bash
npx skills init my-new-skill
```

Then add the skill folder under `skills/` and register it in `skills.sh.json`.

## License

MIT
