---
name: feature-flag
description: Gate functionality behind feature flags for safe rollout, using the project's flagging system, with a plan to remove the flag later
---

# Feature Flag

Ship changes safely behind a flag so they can be rolled out, tested, and turned off without a redeploy — then cleaned up once stable.

## Steps

1. **Find the flagging mechanism** — detect the project's system (LaunchDarkly, Unleash, Flagsmith, a config table, or a simple env/config toggle). If none exists, add the simplest one that fits rather than a heavy dependency
2. **Define the flag** — pick a clear, namespaced name and a safe default (usually **off**); document its purpose and owner
3. **Gate at the right seam** — wrap the new behavior at a single, well-defined decision point; keep the old path intact and reachable so the flag can fall back instantly
4. **Keep both paths working** — ensure the code compiles and tests pass with the flag both on and off; avoid half-applied states
5. **Target rollout** — support enabling for a percentage, environment, or user/segment if the system allows, so it can be dialed up gradually
6. **Test both states** — add or run tests for flag-on and flag-off behavior; verify the default path is safe
7. **Plan removal** — record a cleanup task (and target date/owner) to delete the flag and the dead branch once the feature is fully rolled out — flags are temporary by design

## Rules

- Default new flags to **off/safe**; an unknown or failed flag lookup must fall back to the safe path, never crash
- Keep both branches functional and tested — the old path must remain a real, working fallback
- Gate at a **single clear seam**, not scattered `if (flag)` checks throughout the code
- Name flags clearly and namespaced; record purpose, owner, and intended removal
- Don't entangle multiple unrelated changes in one flag — one flag, one concern
- Treat flags as **temporary**: always leave a cleanup plan so they don't accumulate as permanent dead branches
- Never gate security or correctness fixes behind an off-by-default flag that leaves users exposed
- Don't read flags in hot loops without caching if the lookup is expensive
