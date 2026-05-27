# Codex-Skills

Collection of helpful Codex CLI skills for software development.

## Skills

| Skill | Description |
|-------|-------------|
| [Commit Message](/skills/commit-message/SKILL.md) | Generate conventional commit messages from staged changes |
| [PR Description](/skills/pr-description/SKILL.md) | Generate PR title and description from branch diff |
| [Changelog](/skills/changelog/SKILL.md) | Generate changelog entries from recent commits |

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

```
> generate a commit message for my staged changes
> write a PR description for this branch
> generate changelog for version 2.1.0
```

Codex will find the matching skill and follow its instructions.

## How Codex Skills Work

Each skill is a `SKILL.md` file inside a named folder with YAML frontmatter (`name` and `description`) and markdown instructions.

```
skills/
  commit-message/
    SKILL.md
  pr-description/
    SKILL.md
  changelog/
    SKILL.md
```

Key points:

- **name**: Short identifier for the skill
- **description**: Tells Codex when to use this skill (matched against user intent)
- **Body**: Step-by-step markdown instructions Codex follows
- **Discovery**: Codex scans `.agents/skills/` (project) and `~/.codex/skills/` (global) at startup

See [Codex Skills docs](https://developers.openai.com/codex/skills) for the full reference.
