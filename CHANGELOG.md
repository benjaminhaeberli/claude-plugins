# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/) and this project adheres to [Semantic Versioning](https://semver.org/).

## v1.5.5 - 2026-04-15

Move all skills into a `skills/` subdirectory per plugin to match the expected Claude Code plugin structure and fix skill auto-discovery in the VS Code extension.

**Contributors:** @benjaminhaeberli
**GitHub compare:** https://github.com/benjaminhaeberli/claude-plugins/compare/v1.5.4...v1.5.5

### 🔨 Changed
- Move all skill directories into `skills/` subdirectory for each plugin (`general`, `git`, `audit`, `php`) — required by Claude Code's auto-discovery mechanism
- Update `README.md` and `CLAUDE.md` to reflect the new structure

## v1.5.4 - 2026-04-15

Remove explicit `skills` path from all `plugin.json` manifests to rely on auto-discovery, fixing skill loading after the Claude Code plugin API update.

**Contributors:** @benjaminhaeberli
**GitHub compare:** https://github.com/benjaminhaeberli/claude-plugins/compare/v1.5.3...v1.5.4

### 🐛 Fixed
- Remove `"skills": "./"` from all four `plugin.json` manifests — path was resolved relative to `.claude-plugin/`, making skill auto-discovery fail

## v1.5.3 - 2026-04-15

Fix compatibility with the updated Claude Code plugin API, which now requires a `version` field in every `plugin.json` manifest.

**Contributors:** @benjaminhaeberli
**GitHub compare:** https://github.com/benjaminhaeberli/claude-plugins/compare/v1.5.2...v1.5.3

### 🐛 Fixed

- Add required `version` field to all four `plugin.json` manifests (`general`, `git`, `audit`, `php`)

## v1.5.2 - 2026-03-04

This release refines the git skill suite: the `build` commit type is now included in changelogs under Technical, the GitHub compare URL is surfaced in all release notes, and contributors are displayed inline rather than in a separate section. The outdated `releasor` task doc has been removed.

**Contributors:** @benjaminhaeberli
**GitHub compare:** https://github.com/benjaminhaeberli/claude-plugins/compare/v1.5.1...v1.5.2

### 🔨 Changed

- **changelogator, commitor, releasor**: `build` commit type moved from excluded to `Technical` category
- **changelogator**: GitHub compare URL now included in generated changelog output
- **releasor**: contributors displayed inline after summary paragraph instead of a separate `## Contributors` section
- **releasor**: release notes format unified — same structure for `CHANGELOG.md` and GitHub release body
- **releasor**: added edge case handling for `--no-ff` merge commits (silent deduplication)
- **releasor**: localization extended with `Contributors` and `GitHub compare` French translations

### 🔥 Removed

- Deleted `docs/tasks/260212_releasor.md` — task completed and no longer relevant

## v1.5.1 - 2026-03-04

This release enhances the `commitor` and `changelogator` skills with new commit types, smarter conventions, and improved language handling. The `commitor` skill gains `security`, `build`, and `wip` types, breaking change support, and a new merge mode for synthesizing branch-wide commit messages. The `changelogator` gets aligned category mapping, French localization, and auto-generated release summaries.

### 🔨 Changed

- **commitor**: Add `security` 🔒, `build` 🏗️, and `wip` 🚧 commit types
- **commitor**: Add breaking change format — `!` suffix and `BREAKING CHANGE:` footer
- **commitor**: Clarify `patch` type as multi-area fallback
- **commitor**: Add merge mode — use "merge to `<branch>`" to synthesize a commit from the full branch diff
- **changelogator**: Align category mapping with `commitor` (`security` in Fixed, `Breaking` category, `wip`/`build` excluded)
- **changelogator**: Add French localization table for changelog category names
- **changelogator**: Add release summary paragraph to changelog output
- **changelogator**: Replace "English only" rule with language-adaptive behavior
- **commitor**: Replace "English only" rule with language-adaptive behavior

### ⚙️ Technical

- Add attribution settings tip in README with link to Claude Code docs
- Disable `Co-Authored-By` attribution in `.claude/settings.json`

## v1.5.0 - 2026-02-25

### ✨ Added

- `releasor` skill: orchestrate full release workflow — changelog generation, `CHANGELOG.md` update, Git tag creation, and GitHub publication

### 🔨 Changed

- Include Technical changelog category in output by default for both `releasor` and `changelogator`

### ⚙️ Technical

- Restructure `skills/` into `plugins/` directory with per-plugin `plugin.json` manifests enabling auto-discovery
- Split monolithic plugin into four dedicated plugins: `general`, `git`, `audit`, and `php`
- Rename `*-review` skill directories to `*-audit` across the audit plugin
- Plan and document `releasor` workflow and overall plugin architecture

## v1.4.0 - 2026-02-11

### ✨ Added

- `changelogator` skill: Generate a changelog from git commits following Keep a Changelog format

### 🔨 Changed

- Rename `gitmoji-commit` to `commitor` and enhance the commit template with scope, task number, and changelog mapping
- **changelogator**: Improve output rules with description rewriting and complete template with Technical category

## v1.3.0 - 2026-02-08

### ✨ Added

- `gitmoji-commit` skill: Generate conventional commit messages with GitMoji prefixes (#9)

## v1.2.0 - 2026-02-06

### ✨ Added

- `documentator` skill: Generate structured documentation from code or descriptions (#7, #8)

### ⚙️ Technical

- Add `.claude/settings.local.json` to `.gitignore` (#6)

## v1.1.0 - 2026-02-06

### ✨ Added

- Licensing details and standardized export procedures to audit skills (#5)

## v1.0.0 - 2026-02-06

### ✨ Added

- Initial plugin with `ecodesign`, `gdpr`, and `skill-creator` skills (#1)
- `marketplace.json` catalog with installation documentation (#4)
- CC BY-SA 4.0 license (#3)

### 🐛 Fixed

- README content and plugin configuration corrected for GitHub profile (#2)
