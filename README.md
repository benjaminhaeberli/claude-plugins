# bh-skills

Personal Claude Code plugin — a collection of Claude skills to enhance your codebase.

## Available Skills

### General (framework-agnostic)

| Skill                                              | Description                                             |
| -------------------------------------------------- | ------------------------------------------------------- |
| [skill-creator](skills/skill-creator/)             | Guide for creating effective skills                     |
| [ecodesign-review](skills/ecodesign-review/)       | Audit eco-design & digital sobriety of a web app        |
| [gdpr-review](skills/gdpr-review/)                 | Audit GDPR/RGPD/nLPD compliance of a web app            |
| [seo-review](skills/seo-review/)                   | Audit technical SEO of a web app                        |
| [accessibility-review](skills/accessibility-review/) | Audit WCAG/RGAA/eCH-0059 accessibility of a web app  |

### Stack-specific (PHP / Laravel / Kirby)

| Skill                                                    | Description                                         | Technologies                                                    |
| -------------------------------------------------------- | --------------------------------------------------- | --------------------------------------------------------------- |
| [php-security-review](skills/php-security-review/)       | Audit OWASP security of a PHP web app               | PHP, Laravel, Kirby, Livewire, Blade, Vite, Tailwind CSS, SQL   |
| [php-performance-review](skills/php-performance-review/) | Audit performance & optimization of a PHP web app   | PHP, Laravel, Kirby, Livewire, Blade, Vite, Tailwind CSS, SQL   |

## Installation

### As a plugin (recommended)

**1. Add the marketplace** (once per machine):

```
/plugin marketplace add https://github.com/benjaminhaeberli/bh-skills.git
```

**2. Install the plugin**:

```
/plugin install bh-skills@benjaminhaeberli-bh-skills
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
    "/path/to/skills/skills/ecodesign-review",
    "/path/to/skills/skills/gdpr-review",
    "/path/to/skills/skills/seo-review",
    "/path/to/skills/skills/accessibility-review",
    "/path/to/skills/skills/php-security-review",
    "/path/to/skills/skills/php-performance-review",
    "/path/to/skills/skills/skill-creator"
  ]
}
```

## Structure

```
.claude-plugin/
├── marketplace.json          # Marketplace catalog
└── plugin.json               # Plugin manifest
skills/
├── skill-creator/
│   └── SKILL.md
├── ecodesign-review/
│   ├── SKILL.md
│   └── references/
│       └── checklist.md
├── gdpr-review/
│   ├── SKILL.md
│   └── references/
│       └── checklist.md
├── seo-review/
│   ├── SKILL.md
│   └── references/
│       └── checklist.md
├── accessibility-review/
│   ├── SKILL.md
│   └── references/
│       └── checklist.md
├── php-security-review/
│   ├── SKILL.md
│   └── references/
│       └── checklist.md
└── php-performance-review/
    ├── SKILL.md
    └── references/
        └── checklist.md
```
