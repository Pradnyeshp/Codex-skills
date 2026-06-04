---
name: dead-code
description: Find and safely remove unused code — dead functions, unreachable branches, unused imports, exports, and dependencies
---

# Dead Code

Remove code that nothing uses, with enough verification to be confident it's truly dead.

## Steps

1. **Scope the search** — decide what to check: unused imports/variables, unreferenced private functions, unreachable branches, unused exports, and orphaned files or dependencies
2. **Use the tooling** — prefer the language's analyzers (linter unused-rules, `ts-prune`/`knip`, `vulture`, `deadcode`, compiler warnings, coverage reports) to find candidates
3. **Confirm it's actually dead** — search the whole repo for references, including dynamic/string-based usage, reflection, public API surface, config, CI scripts, and tests. Public/exported symbols may be used by external consumers
4. **Distinguish dead from not-yet-used** — feature-flagged, plugin, or entry-point code can look unreferenced but isn't; check before deleting
5. **Remove in small, reviewable steps** — delete the unused code and cascade to anything it solely depended on (now-unused imports, helpers, deps)
6. **Verify** — build, typecheck, lint, and run the test suite after each removal to confirm nothing broke
7. **Summarize** — list what was removed and why it was safe

## Rules

- Only remove code you've **confirmed unreferenced**, including dynamic and string-based usage — when unsure, leave it and flag it instead of guessing
- Be cautious with public API / exported symbols: they may have external consumers; confirm intent before removing
- Watch for code used only via reflection, DI, plugins, serialization, or config strings — static search won't always catch it
- Remove in small commits and run build + tests after each so a mistaken deletion is caught and easy to revert
- Don't change behavior — this is removal of unused code only, not refactoring of live code
- Cascade cleanly: drop imports, helpers, and dependencies that become unused, but verify each is truly orphaned
- Keep deletions reversible (version control) and clearly summarized so a reviewer can sanity-check each one
