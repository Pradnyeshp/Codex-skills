# Contributing

Thanks for your interest in adding to the Codex skills catalog.

## Adding a skill

1. Create a folder under `skills/<skill-name>/` with a single `SKILL.md`.
2. Start the file with YAML frontmatter — `name` and a one-line `description` of when to use it.
3. Write the body as clear, step-by-step guidance, ending with a concise **Rules** section.
4. Cross-link related skills by name (e.g. pair with `observability`).
5. Add the skill to the catalog table and usage list in [README.md](README.md).

## Conventions

- One skill per folder; keep `SKILL.md` focused and actionable.
- Match the tone and structure of existing skills.
- Use two commits: `feat(skills): add <name> skill ...` then `docs: add <name> skill to catalog`.

See [ROADMAP.md](ROADMAP.md) for skills we'd like to add next.
