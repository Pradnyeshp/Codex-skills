---
name: commit-message
description: Generate a conventional commit message from staged git changes
---

# Commit Message Generator

Generate a commit message for the currently staged git changes following the **Conventional Commits** specification.

## Steps

1. Run `git diff --cached --stat` to see which files are staged
2. Run `git diff --cached` to see the full diff
3. If nothing is staged, tell the user to stage files first with `git add`
4. Analyze the changes and generate a commit message

## Commit Message Format

```
<type>(<scope>): <subject>

<body>
```

## Rules

- **type** must be one of: `feat`, `fix`, `refactor`, `chore`, `docs`, `test`, `style`, `perf`, `ci`, `build`
- **scope** is the primary area affected (e.g., `auth`, `api`, `ui`) — auto-detect from changed file paths
- **subject** is imperative mood, lowercase, no period, under 50 characters
- **body** is optional — include only when the "what" isn't obvious from the subject. Wrap at 72 characters. Explain *why*, not *what*.
- If changes span multiple logical units, suggest splitting into separate commits and show each proposed message

## Output

Print ONLY the commit message — no explanation, no markdown fences.
