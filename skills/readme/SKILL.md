---
name: readme
description: Generate or update a clear, accurate project README that helps a newcomer understand, install, and use the project
---

# Readme

Produce a README that reflects what the project actually does and how to use it — not a generic template.

## Steps

1. **Understand the project** — read the manifests, entry points, and existing docs to determine what the project is, who it's for, and what problem it solves
2. **Find the real usage** — identify the actual install, configure, run, and test commands from scripts/Makefile/CI, and verify they're current rather than copying stale instructions
3. **Capture key features** — list the main capabilities from the code and public API, not aspirational ones
4. **Check for an existing README** — preserve accurate sections, badges, and license info; rewrite only what's outdated or missing
5. **Write the README** in a logical order a newcomer can follow top to bottom
6. **Verify** that every command and code snippet works, and that links and paths resolve

## Output Format

```text
# Project Name

One-sentence description of what it does.

## Features
- ...

## Install
<copy-pasteable steps>

## Usage
<minimal working example>

## Configuration
<key env vars / config options>

## Development
<build, test, lint commands>

## License
<license>
```

## Rules

- Be accurate over comprehensive — every claim, command, and snippet must match the current code
- Lead with what the project does and a minimal working example; defer deep details to later sections
- Use the project's real commands; verify them rather than guessing
- Keep code snippets short, runnable, and copy-pasteable
- Don't invent features, badges, or roadmap items that aren't real
- Preserve existing accurate content and license; don't silently drop sections
- Match the project's tone and existing documentation style
