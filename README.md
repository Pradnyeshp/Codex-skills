# Codex-Skills

Collection of helpful Codex CLI skills for software development.

## Skills

| Skill | Description |
| ----- | ----------- |
| [Commit Message](/skills/commit-message/SKILL.md) | Generate conventional commit messages from staged changes |
| [PR Description](/skills/pr-description/SKILL.md) | Generate PR title and description from branch diff |
| [Changelog](/skills/changelog/SKILL.md) | Generate changelog entries from recent commits |
| [Code Review](/skills/code-review/SKILL.md) | Review the branch diff for bugs, security issues, and quality problems with severity ratings |
| [Test Gen](/skills/test-gen/SKILL.md) | Generate unit tests for changed code, matching the project's framework and conventions |
| [Refactor](/skills/refactor/SKILL.md) | Refactor code for clarity without changing behavior, verified by existing tests |
| [Dockerize](/skills/dockerize/SKILL.md) | Generate a production-ready Dockerfile and .dockerignore for the project |
| [Debug](/skills/debug/SKILL.md) | Systematically diagnose and fix an error, stack trace, or failing test by root cause |
| [Explain](/skills/explain/SKILL.md) | Explain unfamiliar code or a codebase area in plain language for onboarding |
| [Doc Gen](/skills/doc-gen/SKILL.md) | Add docstrings and inline documentation matching the language's conventions |
| [Dependency Audit](/skills/dependency-audit/SKILL.md) | Audit dependencies for outdated and known-vulnerable packages and recommend updates |

## Installation

### Option 1: Copy to your project (project-level)

Copy the `skills/` folder into your project under `.agents/skills/`:

```bash
mkdir -p /path/to/your/project/.agents/skills
cp -r skills/* /path/to/your/project/.agents/skills/
```

### Option 2: Install globally (all projects)

Copy the skill folders to your global Codex config:

```bash
cp -r skills/* ~/.codex/skills/
```

## Usage

Codex automatically discovers skills at startup. Once installed, you can ask Codex to use them:

```text
> generate a commit message for my staged changes
> write a PR description for this branch
> generate changelog for version 2.1.0
> review my branch changes for bugs and security issues
> generate tests for the files I changed
> refactor this function without changing its behavior
> write a Dockerfile for this project
> debug this stack trace and fix the root cause
> explain how the auth module works
> add docstrings to this file
> audit my dependencies for vulnerabilities
```

Codex will find the matching skill and follow its instructions.

## How Codex Skills Work

Each skill is a `SKILL.md` file inside a named folder with YAML frontmatter (`name` and `description`) and markdown instructions.

```text
skills/
  commit-message/
    SKILL.md
  pr-description/
    SKILL.md
  changelog/
    SKILL.md
  code-review/
    SKILL.md
  test-gen/
    SKILL.md
  refactor/
    SKILL.md
  dockerize/
    SKILL.md
  debug/
    SKILL.md
  explain/
    SKILL.md
  doc-gen/
    SKILL.md
  dependency-audit/
    SKILL.md
```

Key points:

- **name**: Short identifier for the skill
- **description**: Tells Codex when to use this skill (matched against user intent)
- **Body**: Step-by-step markdown instructions Codex follows
- **Discovery**: Codex scans `.agents/skills/` (project) and `~/.codex/skills/` (global) at startup

See [Codex Skills docs](https://developers.openai.com/codex/skills) for the full reference.
