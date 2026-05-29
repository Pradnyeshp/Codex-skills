---
name: explain
description: Explain unfamiliar code or a codebase area in plain language to help someone get oriented
---

# Explain

Produce a clear, plain-language walkthrough of code to help someone understand it quickly.

## Steps

1. Identify the target: the file, function, or directory the user names. If they point at a whole area, run a quick scan to map the key files
2. Read the target and enough surrounding context (callers, imports, types) to understand how it fits in
3. Explain it top-down: purpose first, then how it works, then the details that matter

## Output Format

```text
## What it does
<one or two sentences: the purpose and where it fits>

## How it works
1. <step-by-step flow of the main path, in order of execution>
2. ...

## Key pieces
- `name` — <role of this function/type/variable>

## Things to watch out for
- <gotchas, side effects, assumptions, or non-obvious behavior>
```

## Rules

- Lead with **why** it exists and **what** problem it solves, before line-by-line mechanics
- Use plain language; define jargon and domain terms the first time they appear
- Match the depth to the request — a quick summary for "what is this", a deeper trace for "how does this work"
- Cite real file and line references so the reader can follow along
- Call out side effects, external dependencies, and surprising control flow
- Do not invent behavior — if something is unclear, say so rather than guessing
