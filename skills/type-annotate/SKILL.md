---
name: type-annotate
description: Add or improve static type annotations (Python hints, TypeScript types, Go/Rust signatures) and verify with the type checker
---

# Type Annotate

Add precise static types to untyped or loosely typed code, verified by the type checker.

## Steps

1. **Identify the scope** — the file, module, or functions to annotate. Ask if unclear
2. **Detect the type system and checker**:
   - Python: type hints + `mypy` or `pyright` (check `pyproject.toml`/`mypy.ini`)
   - TypeScript: `tsc --noEmit`; tighten `any`, add explicit return types
   - Go/Rust: the compiler enforces types — focus on precise signatures and generics
3. **Infer types from usage** — read how each function is called and what it returns; derive the most accurate types rather than the loosest
4. **Annotate** — add parameter types, return types, and key variable/field types. Use precise types (unions, generics, literals, optionals) over `any`/`interface{}`/`Object`
5. **Run the type checker** and resolve the errors it surfaces — annotations often expose real type mismatches
6. **Verify** — confirm the checker passes clean and the tests still pass

## Rules

- Prefer the **most precise** type the code actually supports; avoid `any`/`unknown`/`object` escape hatches unless genuinely warranted
- Annotations must not change runtime behavior
- Match the project's existing typing style (e.g. `Optional[X]` vs `X | None`, named types vs inline)
- When annotating reveals a real bug or inconsistency, surface it rather than masking it with a loose type
- Don't add redundant annotations the compiler already infers locally, unless the project convention requires them (e.g. explicit public return types)
- Report any type errors you couldn't resolve and why
