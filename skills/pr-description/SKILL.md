---
name: pr-description
description: Generate a pull request title and description from the current branch diff
---

# PR Description Generator

Generate a pull request title and description from the changes on the current branch compared to main.

## Steps

1. Run `git branch --show-current` to get the current branch name
2. Run `git log --oneline main..HEAD` to see all commits on this branch
3. Run `git diff main...HEAD --stat` to see the summary of changes
4. If the branch has no commits ahead of main, tell the user
5. Analyze and generate the PR description

## Output Format

```
## Title
<imperative mood, under 70 chars>

## Summary
- <bullet 1: what changed and why>
- <bullet 2: what changed and why>

## Test plan
- [ ] <how to verify this works>
- [ ] <edge cases to check>
```

## Rules

- Title is imperative mood, under 70 characters, no period
- Summary bullets focus on *why*, not *what* — the diff shows what
- Group related commits into a single bullet
- Include a test plan with concrete verification steps
- Output raw markdown, no wrapping fences
