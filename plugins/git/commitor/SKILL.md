---
name: commitor
description: Generate commit messages combining GitMoji, Conventional Commits, and Keep a Changelog. Use when the user wants to prepare a commit message, write a commit title, generate a conventional commit, add an emoji prefix, or describe staged changes. By default, only outputs the formatted message — executes git commit only when the user explicitly asks to commit.
---

# Commitor

Generate commit messages combining GitMoji + Conventional Commits + Keep a Changelog.

**By default**, output the formatted title and description in chat for the user to review — do **not** run `git commit`. Only execute the commit if the user explicitly asks (e.g., "use commitor and commit", "commit with commitor").

## Workflow

1. Run `git diff --cached` (or `git diff` if nothing is staged) to analyze changes
2. Pick the single most appropriate commit type from the table below
3. Determine the scope if relevant (e.g., auth, panel, exports)
4. Write a short title in imperative mood and a bullet-point description
5. **Default**: output the result in chat for review
6. **If the user asked to commit**: run `git commit` with the generated message

## Output Format

```
<emoji> <type>(<scope>): <description> (<TASK-XXX>)

- First key change
- Second key change
- ...
```

Scope and task number are optional. Only include them when relevant.

## Rules

- English only
- Title in lowercase, imperative mood ("add" not "added")
- Title < 72 characters (including emoji and type prefix)
- One type/emoji per commit
- Description as concise bullet points (no trailing periods)
- Use Unicode emojis (not `:shortcodes:`)
- Scope is optional — use to specify the area of the commit (e.g., auth, panel, api)
- Task number is optional — use project prefix format (e.g., NTT-123, BH-42)

## Commit Types

| Emoji | Type       | Description              |
| ----- | ---------- | ------------------------ |
| ✨    | feat       | New feature              |
| 🔨    | patch      | Minor improvement        |
| 🐛    | fix        | Bug fix                  |
| 📝    | docs       | Documentation            |
| 💄    | style      | UI / cosmetic change     |
| ♻️    | refactor   | Code refactoring         |
| ⚡️    | perf       | Performance improvement  |
| ✅    | test       | Tests                    |
| 🗃️    | data       | Database, DTO, migration |
| 🔥    | remove     | Remove code or files     |
| 🤖    | ai         | AI-related changes       |
| ⚙️    | config     | Configuration            |

## Changelog Mapping

Reference for mapping commit types to [Keep a Changelog](https://keepachangelog.com/) categories:

| Changelog Category | Commit Types               |
| ------------------ | -------------------------- |
| Added              | feat                       |
| Changed            | patch, style, perf, data   |
| Fixed              | fix                        |
| Removed            | remove                     |
| Technical          | docs, refactor, test, ai, config |

## Examples

```
✨ feat(auth): add OAuth2 login with Google (NTT-128)

- Implement OAuth2 flow with PKCE
- Add Google provider configuration
- Store refresh tokens in encrypted session
```

```
🐛 fix(exports): prevent duplicate rows in CSV export

- Add DISTINCT clause to export query
- Filter soft-deleted records before export
```

```
⚙️ config: update CI pipeline for Node 22

- Bump node version in GitHub Actions workflow
- Update lockfile for compatibility
```
