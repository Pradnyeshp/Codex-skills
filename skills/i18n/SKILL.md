---
name: i18n
description: Extract hardcoded user-facing strings and wire up internationalization and localization using the project's i18n library
---

# Internationalization (i18n)

Make user-facing text translatable without changing behavior, following the project's existing i18n setup.

## Steps

1. **Detect the i18n setup** — find the library and catalogs in use (`i18next`/`react-intl`, `gettext`/`.po`, Rails `config/locales/*.yml`, Django `makemessages`, ICU MessageFormat). If none, add the ecosystem-standard one rather than a custom scheme
2. **Find hardcoded strings** — locate user-facing literals in views, components, templates, and messages; skip logs, internal identifiers, and developer-only text
3. **Extract with stable keys** — replace literals with translation lookups using clear, namespaced keys; add the source-language entry to the catalog as the default
4. **Handle plurals and interpolation** — use the library's plural/select forms and named placeholders instead of string concatenation, so grammar and word order can vary by locale
5. **Cover formatting** — route dates, numbers, currency, and lists through locale-aware formatters, not hardcoded formats
6. **Externalize the catalog** — keep translations in resource files; ensure missing keys fall back to the source language rather than showing a raw key
7. **Verify** — confirm the UI renders unchanged in the default locale and that extraction tooling picks up every new key; run existing tests

## Rules

- Never build sentences by **concatenation** — use full messages with named placeholders and the library's plural/select syntax
- Use **stable, namespaced keys**; don't key by the English text (it breaks when copy changes)
- Translate only **user-facing** text — leave logs, error codes, config, and internal IDs alone
- Route dates/numbers/currency through **locale-aware formatters**, never hardcoded formats or symbols
- Provide a sensible **fallback locale**; a missing translation should degrade gracefully, not crash or show a raw key
- Keep the source-language behavior **identical** — i18n is a refactor, not a copy change
- Match the project's existing catalog format and extraction workflow rather than introducing a parallel system
- Watch for layout impact (text expansion, RTL) when relevant, but don't redesign UI here
