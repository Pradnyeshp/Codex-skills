---
name: accessibility
description: Audit UI code for accessibility (a11y) issues against WCAG and suggest concrete fixes for semantics, keyboard, contrast, and ARIA
---

# Accessibility

Review front-end code for accessibility barriers and recommend concrete, standards-based fixes.

## Steps

1. **Identify the scope** — the component, page, or template to audit. Ask if not specified
2. **Check semantics** — are native elements used correctly (`button`, `a`, `nav`, `main`, headings in order, `label` tied to inputs)? Flag `div`/`span` standing in for interactive elements
3. **Check keyboard access** — every interactive element must be focusable and operable by keyboard, with a visible focus indicator and a logical tab order; no keyboard traps
4. **Check names and roles** — images need meaningful `alt` (or empty `alt` if decorative); icon-only controls need accessible names; use ARIA only to fill real gaps, never to replace native semantics
5. **Check forms** — labels, error messaging, required-field indication, and association of errors with fields
6. **Check color & contrast** — text meets WCAG AA contrast (4.5:1 body, 3:1 large); information isn't conveyed by color alone
7. **Report findings** with severity, the WCAG criterion, and a concrete fix for each

## Output Format

```text
## Accessibility Audit

### Blocker — keyboard
- <file:line> Custom dropdown not reachable by Tab (WCAG 2.1.1)
  Fix: make the trigger a <button>, manage focus, support Arrow/Esc keys

### Serious — names
- <file:line> Icon button has no accessible name (WCAG 4.1.2)
  Fix: add aria-label="Close" (or visually-hidden text)
```

## Rules

- Prefer **native semantic HTML** over ARIA; only add ARIA to fill genuine gaps, and never break native semantics with it
- Cite the relevant WCAG success criterion so fixes are justifiable, not opinion
- Give a concrete, minimal fix for each finding — not just "this is inaccessible"
- Don't rely on color alone; ensure information has a non-color cue
- Note what you could and couldn't check statically (e.g. contrast needs rendered values; screen-reader behavior needs manual testing)
- Order findings by severity so the most blocking issues come first
