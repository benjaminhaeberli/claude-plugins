---
name: releasor
description: Orchestrate a full release: changelog generation, CHANGELOG.md update, Git tag creation, and GitHub publication. Use when the user asks to prepare or publish a release, create a version tag, or mentions "release", "publish version", "tag", or "version bump". Completes the chain commitor → changelogator → releasor.
---

# Releasor

Orchestrate a complete release workflow: generate changelog → update `CHANGELOG.md` → commit + tag → push + publish.

**Default behavior**: generate and display everything for review. Only write files or run destructive commands after explicit user confirmation.

## Workflow

### Step 1 — Analyze current state

```bash
git tag --sort=-v:refname | head -1   # last tag
git log <tag>..HEAD --oneline          # commits since tag (no tag → full log)
```

Display a summary:
- Last tag (or "no tag yet")
- Number of commits since tag
- Types present (feat, fix, patch…)
- Suggested semver bump (see Semver Rules below)

If there are no commits since the last tag, stop and inform the user.

### Step 2 — Generate changelog

Parse commits with:

```bash
git log <tag>..HEAD --format="---COMMIT---%n%H%n%s%n%b" --reverse
```

Apply the full **changelogator** logic (commit format, category mapping, semver rules, output template, output rules).

**If `gh` is available**, enrich each entry with its associated PR number:

```bash
gh pr list --search "<SHA>" --state merged --json number,url --jq '.[0] | "(#\(.number))"'
```

Append `(#123)` to the relevant changelog entry when found.

Build the **GitHub compare URL**:

```bash
git remote get-url origin   # → https://github.com/owner/repo.git (strip .git)
# → https://github.com/owner/repo/compare/<prev-tag>...vX.Y.Z
```

**If `gh` is available**, resolve contributors:

```bash
git log <prev-tag>..HEAD --format="%H" | while read sha; do
  gh api /repos/{owner}/{repo}/commits/$sha --jq '.author.login' 2>/dev/null
done | sort -u
```

If `gh` unavailable: skip the Contributors line entirely.

Propose the changelog and suggested version in chat. Let the user adjust before continuing.

### Step 3 — Update CHANGELOG.md

**Wait for explicit user confirmation before writing.**

- If `CHANGELOG.md` exists: prepend the new entry below the title line
- If it does not exist: create it with a standard Keep a Changelog header, then append the entry

```markdown
# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/) and this project adheres to [Semantic Versioning](https://semver.org/).

## vX.Y.Z - YYYY-MM-DD
...
```

Do **not** overwrite existing entries.

### Step 4 — Commit and create Git tag

**Ask confirmation before running any git command.**

1. Commit `CHANGELOG.md` using the **commitor** format:

```
📝 docs(changelog): add vX.Y.Z release notes
```

Run: `git add CHANGELOG.md && git commit -m "📝 docs(changelog): add vX.Y.Z release notes"`

2. Create an annotated tag on that commit:

```bash
git tag -a vX.Y.Z -m "Release vX.Y.Z\n\n<changelog content>"
```

### Step 5 — Display release notes

Always display the complete release notes in chat, ready to copy-paste.

The release notes use the following format (same for CHANGELOG.md and GitHub release):

```markdown
## vX.Y.Z - YYYY-MM-DD

<summary paragraph>

**Contributors:** @username1, @username2
**GitHub compare:** https://github.com/{owner}/{repo}/compare/{prev-tag}...vX.Y.Z

### ✨ Added
- ...

### 🐛 Fixed
- ...
```

> `Contributors` line: only included when `gh` is available. Omit entirely if unavailable.
> `GitHub compare` line: always included when the remote is a GitHub URL.

Inform the user: "Tag created locally. Validate the release notes above before pushing."

### Step 6 — Push and publish

**Ask explicit confirmation before each action.**

```bash
git push origin HEAD         # push commit
git push origin vX.Y.Z      # push tag
```

If `gh` is available and the remote is GitHub, offer to publish the release:

```bash
gh release create vX.Y.Z \
  --title "vX.Y.Z" \
  --notes "<release notes>"
```

Confirm success and display the release URL if published.

---

## Semver Rules

| Condition                                    | Bump  |
| -------------------------------------------- | ----- |
| Any breaking change (`!` in type or `BREAKING CHANGE` in body) | Major |
| Any `feat` commit                            | Minor |
| Everything else (fix, patch, docs…)          | Patch |

If no tag exists, suggest `v0.1.0`.

## Commit Format (changelogator-compatible)

```
<emoji> <type>(<scope>): <description>
```

| Changelog Category | Emoji | Commit Types                          |
| ------------------ | ----- | ------------------------------------- |
| Breaking           | ⚠️    | any type with `!`                     |
| Added              | ✨    | feat                                  |
| Changed            | 🔨    | patch, style, perf, data              |
| Fixed              | 🐛    | fix, security                         |
| Removed            | 🔥    | remove                                |
| Technical          | ⚙️    | docs, refactor, test, ai, config, build |

> `wip` commits are excluded from changelogs — they don't represent releasable changes.

## Output Rules

- Rewrite descriptions — do not copy commit subjects verbatim
- Use backticks for code references (commands, file names, keys)
- Omit empty categories
- English by default; match existing changelog language if evident
- Skip merge commits

## gh Availability Matrix

| | Without `gh` | With `gh` |
|---|---|---|
| **Changelog** | Commits only | + PR numbers `(#123)` |
| **Contributors** | Not included | Inline after summary |
| **Release notes** | Displayed in chat | Displayed in chat |
| **Publication** | Manual (copy to GitHub UI) | Via `gh release create` |

## Edge Cases

- **No tag**: use all commits, suggest `v0.1.0`
- **No commits since tag**: stop and inform the user
- **`gh` not available**: skip Contributors line and PR enrichment silently, proceed without it
- **PR not found for a commit**: skip silently, no `(#...)` appended
- **`CHANGELOG.md` missing**: create it with standard header before prepending
- **`--no-ff` merges**: individual branch commits AND the merge commit both appear in `git log <tag>..HEAD`, causing duplicate changelog entries. Use `--ff-only` or squash merges for clean output. If duplicates are detected (same description on multiple commits), deduplicate silently.
