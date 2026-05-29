---
name: doc-gen
description: Add docstrings and inline documentation to functions, matching the language's conventions and existing style
---

# Doc Generator

Add documentation comments to code, following the language's idiomatic format.

## Steps

1. Identify the target: the file/function the user names, or undocumented public functions in the file they point at
2. Detect the documentation convention:
   - Read 1–2 existing documented functions to match the project's style
   - Otherwise use the language idiom: JSDoc/TSDoc (JS/TS), Google or NumPy docstrings (Python), GoDoc comments (Go), rustdoc `///` (Rust), Javadoc (Java)
3. Read each function's signature and body to understand what it actually does
4. Write accurate documentation for each target

## What Each Docstring Covers

- A one-line summary of what the function does
- Parameters: name, type (if not in signature), and meaning
- Return value: what it represents
- Errors/exceptions raised, when relevant
- A short usage example only when behavior is non-obvious

## Rules

- Document **public** APIs first; skip trivial getters/setters and self-explanatory one-liners
- Describe behavior accurately from the code — never document intent the code does not implement
- Match the existing convention and format exactly; do not introduce a new doc style
- Explain *why* and *what*, not a restatement of the code line by line
- Do not change any logic — documentation only
- Keep wording concise and free of filler
