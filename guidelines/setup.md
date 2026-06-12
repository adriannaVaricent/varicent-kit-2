# Varicent ICM Make Kit v2 — Guidelines Setup

## Purpose

These guidelines teach **Figma Make** to generate Varicent-faithful React prototypes using the same `@varicent/varicent-ui-*` packages as `icm-ui`. Kit v2 adds corrected component names, a 30-component quick reference, verified icon registry, and battle-tested component patterns from real Make sessions.

## Required reading (in order)

1. `guidelines/setup.md` (this file)
2. `guidelines/Guidelines.md` — tokens, typography, spacing, 30 components, AI generation rules
3. `guidelines/icon-registry.md` — semantic action → verified icon export
4. `guidelines/component-patterns.md` — per-component usage rules (append-only)
5. `guidelines/component-notes.md` — session-learned fixes with Don't/Do examples

## How to use in Make

1. Attach **`@make-kits/varicent-kit-2`** to your Make file.
2. List `@varicent/varicent-ui-*` packages as **direct dependencies** in the Make project's `package.json` (not only transitive via the kit).
3. In **kit-strict** convergence prompts, name components explicitly: e.g. "Use `ObjectTable`, `FilterForm`, `Dialog`, `Button`, `Tag` from the kit only."
4. Pair guidelines with realistic ICM fixture data and required states (empty, loading, error, permission-limited).

## Direct dependencies (required in Make project)

The Make project's `package.json` must list these as direct dependencies so Vite can resolve them:

```
@make-kits/varicent-kit-2
@varicent/varicent-ui-core
@varicent/varicent-ui-data-grid
@varicent/varicent-ui-date
@varicent/varicent-ui-filter-form
@varicent/varicent-ui-gen-ai
@varicent/varicent-ui-icons
@varicent/varicent-ui-illustrations
@varicent/varicent-ui-select
@varicent/varicent-ui-tokens
@varicent/varicent-ui-tree
```

## Import conventions

- Import from the package listed in `Guidelines.md` Section 5 — not from deep file paths.
- Use **semantic props** (`intent`, `priority`, `type`, `disabled`, `loading`) instead of custom CSS overrides.
- Prefer **composition**: `FormGroup` + `Input`/`Select`, `Dialog` + `Button`, `ObjectTable` + `NonIdealState` for empty states.
- Icons: `@varicent/varicent-ui-icons` — look up intent in `icon-registry.md` before importing.

## Global anti-patterns

### Never use these libraries

- shadcn/ui, `@radix-ui/*`, `lucide-react`, `@mui/*`, `sonner`, `recharts`
- `@carbon/icons-react` — use `@varicent/varicent-ui-icons` instead
- Do not create or use `src/app/components/ui/` scaffold folders

### On runtime errors

Fix props and data keys — **do not** replace the DS component with custom UI.

### Button fills (Figma plugin + Make)

Never set `.fills`, `.strokes`, or `.effects` on Button instances or children to override color — use `intent` and `priority` props only.

## pnpm symlink note

This project uses pnpm, which symlinks packages. Do **not** use `find`, `glob`, or `file_search` to discover guideline files — they silently fail on symlinks. Read files by exact path, use `ls`, or `find -L`.

## Folder layout

```
guidelines/
  setup.md
  Guidelines.md
  icon-registry.md
  component-patterns.md
  component-notes.md
docs/make/
  CONTRIBUTING.md
```

## Contributing new rules

Designers can say **"add rule:"** in Make chat to append verified entries. See `docs/make/CONTRIBUTING.md` for the full workflow.
