# 🤖 Claude Plugins

**A collection of Claude plugins to enhance your codebase**

## Structure

```
.claude-plugin/marketplace.json                           # Marketplace catalog
plugins/general/.claude-plugin/plugin.json                # General plugin manifest
plugins/general/skills/<skill-name>/SKILL.md              # General skills (docs, skill creation)
plugins/git/.claude-plugin/plugin.json                    # Git plugin manifest
plugins/git/skills/<skill-name>/SKILL.md                  # Git workflow skills
plugins/audit/.claude-plugin/plugin.json                  # Audit plugin manifest
plugins/audit/skills/<skill-name>/SKILL.md                # Audit skills
plugins/php/.claude-plugin/plugin.json                    # PHP plugin manifest
plugins/php/skills/<skill-name>/SKILL.md                  # PHP skills
```

Each skill follows the standard format: `SKILL.md` with YAML frontmatter (`name`, `description`) and Markdown instructions, plus optional `scripts/`, `references/`, and `assets/` directories.

## Adding a New Skill

Use the `skill-creator` skill for guidance. Key principles:

- Keep SKILL.md concise (< 500 lines)
- Use progressive disclosure: metadata always loaded, body on trigger, resources on demand
- Match freedom level to task fragility
- **`plugin.json` uses auto-discovery (no update needed)** — just drop a new skill directory with `SKILL.md` inside the relevant `skills/` folder
- **Always update `CLAUDE.md` and `README.md`** when adding or modifying a skill (table and structure tree)
