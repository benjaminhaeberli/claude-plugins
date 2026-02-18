# 🤖 Claude Plugins

**A collection of Claude plugins to enhance your codebase**

## Structure

```
.claude-plugin/marketplace.json          # Marketplace catalog
skills/general/<skill-name>/SKILL.md     # General skills (docs, skill creation)
skills/git/<skill-name>/SKILL.md         # Git workflow skills
skills/audit/<skill-name>/SKILL.md       # Web audit skills
skills/php/<skill-name>/SKILL.md         # PHP-specific skills
```

Each skill follows the standard format: `SKILL.md` with YAML frontmatter (`name`, `description`) and Markdown instructions, plus optional `scripts/`, `references/`, and `assets/` directories.

## Adding a New Skill

Use the `skill-creator` skill for guidance. Key principles:

- Keep SKILL.md concise (< 500 lines)
- Use progressive disclosure: metadata always loaded, body on trigger, resources on demand
- Match freedom level to task fragility
- **Always update `marketplace.json` and `README.md`** when adding or modifying a skill (table, structure tree, and manual install path)
