---
name: commitor
description: Generate commit messages combining GitMoji, Conventional Commits, and Keep a Changelog. Use when the user wants to prepare a commit message, write a commit title, generate a conventional commit, add an emoji prefix, or describe staged changes. By default, only outputs the formatted message — executes git commit only when the user explicitly asks to commit. Also handles merge commit summarization: use "merge to <branch>" to analyze the full branch diff.
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

## Merge Mode

**Trigger**: user says "merge to `<branch>`" or "merge commit to `<branch>`".

The current branch is the source; `<branch>` is the merge target (e.g., `main`).

**Workflow**:

```bash
# All commits on current branch since divergence
git log <branch>..HEAD --format="---COMMIT---%n%H%n%s%n%b" --reverse

# Cumulative diff stat for context
git diff <branch>...HEAD --stat
```

Then:
- Read all individual commits to understand the full scope
- Pick the highest-significance type (`feat` > `fix` > `patch` > rest)
- Synthesize a **single** commit message — group changes by theme, not one bullet per commit

## Output Format

Standard commit:
```
<emoji> <type>(<scope>): <description> (<TASK-XXX>)

- First key change
- Second key change
- ...
```

Breaking change (append `!`, add footer):
```
<emoji> <type>(<scope>)!: <description>

- Key change

BREAKING CHANGE: <what breaks and how to migrate>
```

Scope and task number are optional. Only include them when relevant.

## Rules

- English by default; match the language of the existing changelog or commit history if evident
- Title in lowercase, imperative mood ("add" not "added")
- Title < 72 characters (including emoji and type prefix)
- One type/emoji per commit
- Description as concise bullet points (no trailing periods)
- Use Unicode emojis (not `:shortcodes:`)
- Scope is optional — use to specify the area of the commit (e.g., auth, panel, api)
- Task number is optional — use project prefix format (e.g., NTT-123, BH-42)
- Breaking changes: append `!` after type/scope and add a `BREAKING CHANGE:` footer describing what breaks and how to migrate

## Commit Types

| Emoji | Type       | Description              |
| ----- | ---------- | ------------------------ |
| ✨    | feat       | New feature              |
| 🔨    | patch      | Minor improvement or multi-area change |
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
| 🔒    | security   | Security fix or hardening |
| 🏗️    | build      | Build system or tooling  |
| 🚧    | wip        | Work in progress         |

## Changelog Mapping

Reference for mapping commit types to [Keep a Changelog](https://keepachangelog.com/) categories:

| Changelog Category | Commit Types               |
| ------------------ | -------------------------- |
| Added              | feat                       |
| Changed            | patch, style, perf, data   |
| Fixed              | fix, security              |
| Removed            | remove                     |
| Breaking           | any type with `!`          |
| Technical          | docs, refactor, test, ai, config             |

> `wip` and `build` commits are excluded from changelogs — they don't represent releasable changes.

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

```
🔒 security(api): sanitize user input to prevent SQL injection

- Add parameterized queries to user search endpoint
- Escape special characters in filter inputs
```

```
🏗️ build: migrate bundler from webpack to vite

- Replace webpack config with vite.config.ts
- Update npm scripts for dev and build
```

```
🚧 wip(checkout): draft multi-step payment flow

- Add step components (address, payment, confirmation)
- Payment API integration not yet connected
```
