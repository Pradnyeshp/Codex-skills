---
name: release
description: Cut a new version release — determine the version bump, update version files and changelog, and create the tag and release notes
---

# Release

Prepare and cut a clean, versioned release.

## Steps

1. **Determine the new version** — review commits since the last tag (`git describe --tags --abbrev=0`, then `git log <last-tag>..HEAD`) and pick the SemVer bump:
   - **major** for breaking changes, **minor** for new features, **patch** for fixes only
2. **Confirm the version** with the user before changing anything
3. **Update version files** — bump the version in the project's manifest(s): `package.json`, `pyproject.toml`/`__version__`, `Cargo.toml`, `version.go`, etc. Keep them in sync
4. **Update the changelog** — add a dated section for the new version grouped by Added / Changed / Fixed / Removed, derived from the commit log
5. **Run the pre-release checks** — build, test, and lint must pass clean before tagging
6. **Commit** the version and changelog changes with a release message (e.g. `chore(release): v1.4.0`)
7. **Tag** — create an annotated tag (`git tag -a v1.4.0 -m "v1.4.0"`)
8. **Summarize** the release notes for the user; report the push/publish commands but do not run them unless asked

## Rules

- Follow **SemVer** strictly; when unsure between bumps, explain the tradeoff and let the user decide
- Never tag or release with failing tests, build, or lint
- Keep all version references in sync across manifests, lockfiles, and constants
- Write changelog entries for humans — concise, grouped, and meaningful, not a raw commit dump
- Do not `git push`, `npm publish`, or push tags without explicit confirmation — releasing is outward-facing and hard to reverse
- Report exactly what you changed, committed, and tagged
