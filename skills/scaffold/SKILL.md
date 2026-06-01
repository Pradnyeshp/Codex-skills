---
name: scaffold
description: Scaffold a new component, module, or feature with boilerplate, tests, and wiring that match the project's existing structure and conventions
---

# Scaffold

Create the starting files for a new component, module, or feature that fit the codebase like they were always there.

## Steps

1. **Clarify the target** — what is being scaffolded (component, route, service, model, CLI command, etc.) and its name/purpose. Ask if missing
2. **Study an existing peer** — find the closest similar element already in the repo and read it to learn the file layout, naming, imports, exports, and test placement
3. **Plan the files** — list each file you'll create (implementation, types, tests, styles, index/registration) and where it goes, then confirm if non-trivial
4. **Generate the boilerplate** — create the files following the peer's patterns: same structure, naming case, import style, and conventions. Include a minimal working implementation, not just empty stubs
5. **Wire it up** — register the new element where the project expects it (barrel/index exports, router, DI container, plugin list, etc.)
6. **Add a starter test** matching the project's test framework and layout
7. **Verify** — run the build/typecheck and the new test to confirm it's wired correctly

## Rules

- Mirror existing conventions exactly — directory structure, file naming, casing, imports, and exports. Do not invent a new pattern
- Generate working, minimal boilerplate — runnable and typed, not placeholder gibberish
- Don't over-build: scaffold the skeleton and a clear path to extend, not a speculative full feature
- Always include the wiring/registration step so the new code is actually reachable
- List every file created and confirm the project still builds
