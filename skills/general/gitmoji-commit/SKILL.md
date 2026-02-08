---
name: gitmoji-commit
description: Generate git commit messages following the GitMoji convention. Use when the user wants to prepare a commit message, write a commit title, or describe staged changes with appropriate emoji prefixes. Does NOT execute git commands — only outputs the formatted message for the user to commit manually. Reference gitmoji.dev for emoji guide.
---

# GitMoji Commit

Generate commit messages with GitMoji convention. Output the formatted title and description for the user to commit manually — never run `git commit`.

## Workflow

1. Run `git diff --cached` (or `git diff` if nothing is staged) to analyze changes
2. Pick the single most appropriate GitMoji for the change
3. Write a short title in imperative mood
4. Write a bullet-point description summarizing the key changes
5. Output the result in chat — do **not** run any git commit command

## Output Format

```
:emoji: Short title in imperative mood

- First key change
- Second key change
- …
```

### Rules

- One emoji per commit message
- Title < 72 characters, imperative mood ("add" not "added"), lowercase
- Description as concise bullet points (no trailing periods)
- Use English
- Reference issue numbers when relevant (e.g., `fix #42`)

## GitMoji Reference

| Emoji | Code | Usage |
| --- | --- | --- |
| :sparkles: | `:sparkles:` | New feature |
| :bug: | `:bug:` | Bug fix |
| :art: | `:art:` | Code style/format |
| :recycle: | `:recycle:` | Refactoring |
| :zap: | `:zap:` | Performance |
| :memo: | `:memo:` | Documentation |
| :lipstick: | `:lipstick:` | UI/Style |
| :wrench: | `:wrench:` | Configuration |
| :heavy_plus_sign: | `:heavy_plus_sign:` | Add dependency |
| :heavy_minus_sign: | `:heavy_minus_sign:` | Remove dependency |
| :arrow_up: | `:arrow_up:` | Upgrade dependency |
| :arrow_down: | `:arrow_down:` | Downgrade dependency |
| :fire: | `:fire:` | Remove code/files |
| :truck: | `:truck:` | Move/rename files |
| :construction: | `:construction:` | Work in progress |
| :white_check_mark: | `:white_check_mark:` | Add tests |
| :green_heart: | `:green_heart:` | Fix CI |
| :lock: | `:lock:` | Security fix |
| :rotating_light: | `:rotating_light:` | Fix linter warnings |
| :globe_with_meridians: | `:globe_with_meridians:` | Internationalization |
| :pencil2: | `:pencil2:` | Fix typo |
| :rewind: | `:rewind:` | Revert changes |
| :twisted_rightwards_arrows: | `:twisted_rightwards_arrows:` | Merge branches |
| :package: | `:package:` | Build/package |
| :alien: | `:alien:` | External API changes |
| :see_no_evil: | `:see_no_evil:` | Update .gitignore |
