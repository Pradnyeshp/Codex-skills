---
name: changelog
description: Generate a changelog entry from recent git commits for release notes
---

# Changelog Generator

Generate a changelog entry from recent git commits.

## Steps

1. Run `git log --oneline --no-merges -30` to see recent commits
2. If a CHANGELOG.md exists, read it to match its existing style
3. Group commits by type and generate the changelog entry

## Output Format

```
## [<version>] - <YYYY-MM-DD>

### Added
- <new features>

### Changed
- <modifications to existing features>

### Fixed
- <bug fixes>

### Removed
- <removed features>
```

## Rules

- Group commits into **Added**, **Changed**, **Fixed**, **Removed** based on commit type (`feat` -> Added, `fix` -> Fixed, `refactor`/`chore` -> Changed)
- Omit empty sections
- Rewrite commit subjects into user-friendly language — no commit hashes, no jargon
- Use today's date
- Output raw markdown, no wrapping fences
