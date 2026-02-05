# Skills Repository

Personal collection of reusable Claude Code skills.

## Structure

```
skills/
└── <skill-name>/
    └── SKILL.md
```

Each skill follows the standard Claude skill format: a `SKILL.md` with YAML frontmatter (`name`, `description`) and Markdown instructions, plus optional `scripts/`, `references/`, and `assets/` directories.

## Adding a New Skill

Use the `skill-creator` skill for guidance on creating new skills. Key principles:
- Keep SKILL.md concise (< 500 lines) - Claude is already smart, only add what it doesn't know
- Use progressive disclosure: metadata always loaded, body on trigger, resources on demand
- Match freedom level to task fragility (high freedom for flexible tasks, low for fragile ones)
