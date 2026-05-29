---
name: dockerize
description: Generate a production-ready Dockerfile and .dockerignore for the current project
---

# Dockerize

Generate a Dockerfile and `.dockerignore` tailored to the project.

## Steps

1. Detect the stack and entry point:
   - Manifest files: `package.json`, `pyproject.toml`/`requirements.txt`, `go.mod`, `Cargo.toml`, `pom.xml`/`build.gradle`, etc.
   - Build/start scripts, the listening port, and runtime version
2. If a `Dockerfile` already exists, read it and propose improvements instead of overwriting blindly
3. Generate a multi-stage Dockerfile and a matching `.dockerignore`

## Dockerfile Requirements

- **Multi-stage build** — separate build/dependency stage from the slim runtime stage
- Pin a specific, slim base image (e.g. `node:22-slim`, `python:3.12-slim`) — avoid `latest`
- Install dependencies in a cached layer before copying source, to maximize layer caching
- Run as a non-root user
- `EXPOSE` the correct port and set a sensible `CMD`/`ENTRYPOINT`
- Add a `HEALTHCHECK` when the app serves traffic
- Keep the final image minimal — no build tools or dev dependencies in the runtime stage

## .dockerignore Requirements

- Exclude VCS, dependencies, build artifacts, env files, and local tooling
  (e.g. `.git`, `node_modules`, `__pycache__`, `dist`, `build`, `target`, `.env*`, `*.log`)

## Rules

- Tailor versions to what the project actually uses — do not guess if the manifest states a version
- Explain any non-obvious choice in a one-line comment in the Dockerfile
- After generating, suggest the `docker build` command to test it
