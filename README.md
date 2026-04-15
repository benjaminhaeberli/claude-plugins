# 🤖 Claude Plugins

**A collection of Claude plugins to enhance your codebase**

## Plugins

### General

| Skill                                                | Description                                         |
| ---------------------------------------------------- | --------------------------------------------------- |
| [skill-creator](plugins/general/skills/skill-creator/)      | Guide for creating effective skills                  |
| [documentator](plugins/general/skills/documentator/)        | Generate or update structured documentation in Markdown |

### Git

| Skill                                                | Description                                         |
| ---------------------------------------------------- | --------------------------------------------------- |
| [commitor](plugins/git/skills/commitor/)                    | Generate commit messages (GitMoji + Conventional Commits + Keep a Changelog) |
| [changelogator](plugins/git/skills/changelogator/)          | Generate a changelog from git commits (Keep a Changelog + Semver) |
| [releasor](plugins/git/skills/releasor/)                    | Orchestrate a full release: changelog, CHANGELOG.md, Git tag, and GitHub publication |

### Audit

| Skill                                                      | Description                                         |
| ---------------------------------------------------------- | --------------------------------------------------- |
| [ecodesign-audit](plugins/audit/skills/ecodesign-audit/)          | Audit eco-design & digital sobriety of a web app    |
| [gdpr-audit](plugins/audit/skills/gdpr-audit/)                    | Audit GDPR/RGPD/nLPD compliance of a web app        |
| [seo-audit](plugins/audit/skills/seo-audit/)                      | Audit technical SEO of a web app                     |
| [accessibility-audit](plugins/audit/skills/accessibility-audit/)  | Audit WCAG/RGAA/eCH-0059 accessibility of a web app |

### PHP

| Skill                                                            | Description                                       | Technologies                                                  |
| ---------------------------------------------------------------- | ------------------------------------------------- | ------------------------------------------------------------- |
| [php-security-audit](plugins/php/skills/php-security-audit/)            | Audit OWASP security of a PHP web app             | PHP, Laravel, Kirby, Livewire, Blade, Vite, Tailwind CSS, SQL |
| [php-performance-audit](plugins/php/skills/php-performance-audit/)      | Audit performance & optimization of a PHP web app | PHP, Laravel, Kirby, Livewire, Blade, Vite, Tailwind CSS, SQL |

## Installation

### As plugins (recommended)

**1. Add the marketplace** (once per machine):

```
/plugin marketplace add https://github.com/benjaminhaeberli/claude-plugins.git
```

**2. Install the plugins**:

```
/plugin install general@benjaminhaeberli
/plugin install git@benjaminhaeberli
/plugin install audit@benjaminhaeberli
/plugin install php@benjaminhaeberli
```

Auto-updates can be enabled via `/plugin` > Marketplaces > Enable auto-update.

> **Tip**: To hide Claude's `Co-Authored-By` attribution in commits and PRs, configure the [attribution settings](https://code.claude.com/docs/en/settings#attribution-settings) in `~/.claude/settings.json`. The `attribution` setting (with `commit` and `pr` keys set to empty strings) takes precedence over the deprecated `includeCoAuthoredBy` setting.

### Local testing

```bash
claude --plugin-dir /path/to/plugins
```

### Manual (per-project, without plugin system)

Add individual skills to your project's `.claude/settings.json`:

```json
{
  "skills": [
    "/path/to/claude-plugins/plugins/general/skills/documentator",
    "/path/to/claude-plugins/plugins/general/skills/skill-creator",
    "/path/to/claude-plugins/plugins/git/skills/commitor",
    "/path/to/claude-plugins/plugins/git/skills/changelogator",
    "/path/to/claude-plugins/plugins/git/skills/releasor",
    "/path/to/claude-plugins/plugins/audit/skills/ecodesign-audit",
    "/path/to/claude-plugins/plugins/audit/skills/gdpr-audit",
    "/path/to/claude-plugins/plugins/audit/skills/seo-audit",
    "/path/to/claude-plugins/plugins/audit/skills/accessibility-audit",
    "/path/to/claude-plugins/plugins/php/skills/php-security-audit",
    "/path/to/claude-plugins/plugins/php/skills/php-performance-audit"
  ]
}
```

## License

This project is licensed under [CC-BY-SA-4.0](https://creativecommons.org/licenses/by-sa/4.0/).

The `skill-creator` skill contains content derived from Anthropic's Claude Code documentation, originally licensed under [Apache License 2.0](https://www.apache.org/licenses/LICENSE-2.0). In accordance with Apache 2.0 terms, proper attribution is maintained. The derivative work is distributed under CC-BY-SA-4.0.

## Structure

```
.claude-plugin/
└── marketplace.json                    # Marketplace catalog
plugins/
├── general/                            # general plugin
│   ├── .claude-plugin/
│   │   └── plugin.json
│   └── skills/
│       ├── skill-creator/
│       │   └── SKILL.md
│       └── documentator/
│           └── SKILL.md
├── git/                                # git plugin
│   ├── .claude-plugin/
│   │   └── plugin.json
│   └── skills/
│       ├── commitor/
│       │   └── SKILL.md
│       ├── changelogator/
│       │   └── SKILL.md
│       └── releasor/
│           └── SKILL.md
├── audit/                              # audit plugin
│   ├── .claude-plugin/
│   │   └── plugin.json
│   └── skills/
│       ├── ecodesign-audit/
│       │   ├── SKILL.md
│       │   └── references/
│       │       └── checklist.md
│       ├── gdpr-audit/
│       │   ├── SKILL.md
│       │   └── references/
│       │       └── checklist.md
│       ├── seo-audit/
│       │   ├── SKILL.md
│       │   └── references/
│       │       └── checklist.md
│       └── accessibility-audit/
│           ├── SKILL.md
│           └── references/
│               └── checklist.md
└── php/                                # php plugin
    ├── .claude-plugin/
    │   └── plugin.json
    └── skills/
        ├── php-security-audit/
        │   ├── SKILL.md
        │   └── references/
        │       └── checklist.md
        └── php-performance-audit/
            ├── SKILL.md
            └── references/
                └── checklist.md
```
