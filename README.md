# bh-skills

Personal Claude Code plugin — a collection of reusable skills.

## Available Skills

| Skill                                          | Description                                      |
| ---------------------------------------------- | ------------------------------------------------ |
| [skill-creator](skills/skill-creator/)         | Guide for creating effective skills              |
| [ecodesign-analyze](skills/ecodesign-analyze/) | Audit eco-design & digital sobriety of a web app |

## Installation

### As a plugin (recommended)

This repo works as a private GitHub repository — no need to make it public. Authentication is handled by your existing git config (SSH keys or HTTPS token).

**1. Add the marketplace** (once per machine):

```
/plugin marketplace add git@github.com:YOUR_USER/skills.git
```

**2. Install the plugin**:

```
/plugin install bh-skills@YOUR_USER-skills
```

Auto-updates can be enabled via `/plugin` > Marketplaces > Enable auto-update.

### Local testing

```bash
claude --plugin-dir /path/to/skills
```

### Manual (per-project, without plugin system)

Add individual skills to your project's `.claude/settings.json`:

```json
{
  "skills": [
    "/path/to/skills/skills/ecodesign-analyze",
    "/path/to/skills/skills/skill-creator"
  ]
}
```

## Structure

```
.claude-plugin/
└── plugin.json           # Plugin manifest
skills/
├── skill-creator/
│   └── SKILL.md
└── ecodesign-analyze/
    ├── SKILL.md
    └── references/
        └── checklist.md
```
