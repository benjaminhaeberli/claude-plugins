# bh-skills

Personal Claude Code plugin with reusable skills.

## Structure

```
.claude-plugin/marketplace.json          # Marketplace catalog
skills/general/<skill-name>/SKILL.md     # General skills (framework-agnostic)
skills/php/<skill-name>/SKILL.md         # PHP-specific skills
```

Each skill follows the standard format: `SKILL.md` with YAML frontmatter (`name`, `description`) and Markdown instructions, plus optional `scripts/`, `references/`, and `assets/` directories.

## Adding a New Skill

Use the `skill-creator` skill for guidance. Key principles:

- Keep SKILL.md concise (< 500 lines)
- Use progressive disclosure: metadata always loaded, body on trigger, resources on demand
- Match freedom level to task fragility
