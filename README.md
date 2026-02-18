# 🤖 Claude Plugins

**A collection of Claude plugins to enhance your codebase**

## Plugins

### General

| Skill                                              | Description                                         |
| -------------------------------------------------- | --------------------------------------------------- |
| [skill-creator](skills/general/skill-creator/)     | Guide for creating effective skills                  |
| [documentator](skills/general/documentator/)       | Generate or update structured documentation in Markdown |

### Git

| Skill                                              | Description                                         |
| -------------------------------------------------- | --------------------------------------------------- |
| [commitor](skills/git/commitor/)                   | Generate commit messages (GitMoji + Conventional Commits + Keep a Changelog) |
| [changelogator](skills/git/changelogator/)         | Generate a changelog from git commits (Keep a Changelog + Semver) |

### Audit

| Skill                                                    | Description                                         |
| -------------------------------------------------------- | --------------------------------------------------- |
| [ecodesign-audit](skills/audit/ecodesign-audit/)         | Audit eco-design & digital sobriety of a web app    |
| [gdpr-audit](skills/audit/gdpr-audit/)                   | Audit GDPR/RGPD/nLPD compliance of a web app        |
| [seo-audit](skills/audit/seo-audit/)                     | Audit technical SEO of a web app                     |
| [accessibility-audit](skills/audit/accessibility-audit/) | Audit WCAG/RGAA/eCH-0059 accessibility of a web app |

### PHP

| Skill                                                          | Description                                       | Technologies                                                  |
| -------------------------------------------------------------- | ------------------------------------------------- | ------------------------------------------------------------- |
| [php-security-audit](skills/php/php-security-audit/)           | Audit OWASP security of a PHP web app             | PHP, Laravel, Kirby, Livewire, Blade, Vite, Tailwind CSS, SQL |
| [php-performance-audit](skills/php/php-performance-audit/)     | Audit performance & optimization of a PHP web app | PHP, Laravel, Kirby, Livewire, Blade, Vite, Tailwind CSS, SQL |

## Installation

### As plugins (recommended)

**1. Add the marketplace** (once per machine):

```
/plugin marketplace add https://github.com/benjaminhaeberli/claude-plugins.git
```

**2. Install the plugins**:

```
/plugin install general-skills@benjaminhaeberli
/plugin install git-skills@benjaminhaeberli
/plugin install audit-skills@benjaminhaeberli
/plugin install php-skills@benjaminhaeberli
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
    "/path/to/claude-plugins/skills/general/documentator",
    "/path/to/claude-plugins/skills/general/skill-creator",
    "/path/to/claude-plugins/skills/git/commitor",
    "/path/to/claude-plugins/skills/git/changelogator",
    "/path/to/claude-plugins/skills/audit/ecodesign-audit",
    "/path/to/claude-plugins/skills/audit/gdpr-audit",
    "/path/to/claude-plugins/skills/audit/seo-audit",
    "/path/to/claude-plugins/skills/audit/accessibility-audit",
    "/path/to/claude-plugins/skills/php/php-security-audit",
    "/path/to/claude-plugins/skills/php/php-performance-audit"
  ]
}
```

## License

This project is licensed under [CC-BY-SA-4.0](https://creativecommons.org/licenses/by-sa/4.0/).

The `skill-creator` skill contains content derived from Anthropic's Claude Code documentation, originally licensed under [Apache License 2.0](https://www.apache.org/licenses/LICENSE-2.0). In accordance with Apache 2.0 terms, proper attribution is maintained. The derivative work is distributed under CC-BY-SA-4.0.

## Structure

```
.claude-plugin/
└── marketplace.json              # Marketplace catalog
skills/
├── general/                      # general-skills plugin
│   ├── skill-creator/
│   │   └── SKILL.md
│   └── documentator/
│       └── SKILL.md
├── git/                          # git-skills plugin
│   ├── commitor/
│   │   └── SKILL.md
│   └── changelogator/
│       └── SKILL.md
├── audit/                        # audit-skills plugin
│   ├── ecodesign-audit/
│   │   ├── SKILL.md
│   │   └── references/
│   │       └── checklist.md
│   ├── gdpr-audit/
│   │   ├── SKILL.md
│   │   └── references/
│   │       └── checklist.md
│   ├── seo-audit/
│   │   ├── SKILL.md
│   │   └── references/
│   │       └── checklist.md
│   └── accessibility-audit/
│       ├── SKILL.md
│       └── references/
│           └── checklist.md
└── php/                          # php-skills plugin
    ├── php-security-audit/
    │   ├── SKILL.md
    │   └── references/
    │       └── checklist.md
    └── php-performance-audit/
        ├── SKILL.md
        └── references/
            └── checklist.md
```
