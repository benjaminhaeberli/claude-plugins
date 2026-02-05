# Skills

Personal collection of reusable [Claude Code skills](https://code.claude.com/docs/en/skills).

## Available Skills

| Skill | Description |
|-------|-------------|
| [skill-creator](skills/skill-creator/) | Guide for creating effective skills |

## Usage

To use these skills with Claude Code, add them to your project's `.claude/settings.json`:

```json
{
  "skills": [
    "/path/to/skills/skills/skill-creator"
  ]
}
```

Or install a packaged `.skill` file directly in Claude Code.

## Structure

Each skill lives in `skills/<skill-name>/` and follows the [standard skill format](https://code.claude.com/docs/en/skills):

```
skills/<skill-name>/
├── SKILL.md              # Required: frontmatter + instructions
├── scripts/              # Optional: executable code
├── references/           # Optional: documentation loaded on demand
└── assets/               # Optional: files used in output
```
