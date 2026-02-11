# 🤖 Claude Plugins

**A collection of Claude plugins to enhance your codebase**

## Plugins

### General (framework-agnostic)

| Skill                                                        | Description                                         |
| ------------------------------------------------------------ | --------------------------------------------------- |
| [skill-creator](skills/general/skill-creator/)               | Guide for creating effective skills                          |
| [ecodesign-review](skills/general/ecodesign-review/)         | Audit eco-design & digital sobriety of a web app             |
| [gdpr-review](skills/general/gdpr-review/)                   | Audit GDPR/RGPD/nLPD compliance of a web app                |
| [seo-review](skills/general/seo-review/)                     | Audit technical SEO of a web app                             |
| [accessibility-review](skills/general/accessibility-review/) | Audit WCAG/RGAA/eCH-0059 accessibility of a web app         |
| [changelogator](skills/general/changelogator/)               | Generate a changelog from git commits (Keep a Changelog + Semver)            |
| [commitor](skills/general/commitor/)                         | Generate commit messages (GitMoji + Conventional Commits + Keep a Changelog) |
| [documentator](skills/general/documentator/)                 | Generate or update structured documentation in Markdown      |

### Stack-specific (PHP / Laravel / Kirby)

| Skill                                                        | Description                                       | Technologies                                                  |
| ------------------------------------------------------------ | ------------------------------------------------- | ------------------------------------------------------------- |
| [php-security-review](skills/php/php-security-review/)       | Audit OWASP security of a PHP web app             | PHP, Laravel, Kirby, Livewire, Blade, Vite, Tailwind CSS, SQL |
| [php-performance-review](skills/php/php-performance-review/) | Audit performance & optimization of a PHP web app | PHP, Laravel, Kirby, Livewire, Blade, Vite, Tailwind CSS, SQL |

## Installation

### As plugins (recommended)

**1. Add the marketplace** (once per machine):

```
/plugin marketplace add https://github.com/benjaminhaeberli/claude-plugins.git
```

**2. Install the plugins**:

```
/plugin install general-skills@benjaminhaeberli
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
    "/path/to/claude-plugins/skills/general/ecodesign-review",
    "/path/to/claude-plugins/skills/general/gdpr-review",
    "/path/to/claude-plugins/skills/general/seo-review",
    "/path/to/claude-plugins/skills/general/accessibility-review",
    "/path/to/claude-plugins/skills/general/changelogator",
    "/path/to/claude-plugins/skills/general/commitor",
    "/path/to/claude-plugins/skills/general/documentator",
    "/path/to/claude-plugins/skills/general/skill-creator",
    "/path/to/claude-plugins/skills/php/php-security-review",
    "/path/to/claude-plugins/skills/php/php-performance-review"
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
│   ├── ecodesign-review/
│   │   ├── SKILL.md
│   │   └── references/
│   │       └── checklist.md
│   ├── gdpr-review/
│   │   ├── SKILL.md
│   │   └── references/
│   │       └── checklist.md
│   ├── seo-review/
│   │   ├── SKILL.md
│   │   └── references/
│   │       └── checklist.md
│   ├── accessibility-review/
│   │   ├── SKILL.md
│   │   └── references/
│   │       └── checklist.md
│   ├── changelogator/
│   │   └── SKILL.md
│   ├── commitor/
│   │   └── SKILL.md
│   └── documentator/
│       └── SKILL.md
└── php/                          # php-skills plugin
    ├── php-security-review/
    │   ├── SKILL.md
    │   └── references/
    │       └── checklist.md
    └── php-performance-review/
        ├── SKILL.md
        └── references/
            └── checklist.md
```
