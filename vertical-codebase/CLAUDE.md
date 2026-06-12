# Vertical Codebase - Principles & Rules

Based on [The Vertical Codebase](https://tkdodo.eu/blog/the-vertical-codebase) by TkDodo.

## Core Principle

> Code that changes together should live together.

Organize code by **what it does** (business domain / functionality), not by **what it technically is** (components, hooks, utils, types).

## What's Wrong with Horizontal Structure

Horizontal (type-based) structure groups code by technical layer:

```
src/
  components/    # 200+ unrelated components
  hooks/         # all hooks lumped together
  utils/         # grab-bag of utilities
  types/         # all type definitions
  constants/     # all constants
```

Problems:
- **Poor discoverability** - related code scattered across directories
- **No clear boundaries** - any component can import any util
- **Low cohesion** - files live together only because they share a technical type
- **Scale issues** - directories grow unbounded with unrelated files
- **Hard to reason about ownership** - who owns `utils/analytics`?

## Vertical Structure

Group code by functionality/domain into self-contained verticals:

```
src/
  widgets/
    components/
    hooks/
    utils/
    types/
    index.ts          # public API (barrel file)
  profiling/
    components/
    hooks/
    utils/
    index.ts
  dashboard/
    components/
    hooks/
    widgets/          # can nest sub-verticals
    index.ts
  design-system/      # shared, domain-agnostic code
    button/
    modal/
    index.ts
```

## Rules

### 1. Group by Domain
- Each vertical contains ALL code for that feature: components, hooks, types, utils, constants
- A vertical represents a business capability, not a technical layer
- Routes/pages are a good starting point for identifying verticals

### 2. Co-locate Everything
- Props live with their component (not in a separate types/ file)
- Data fetching logic lives with the component that uses it
- Tests live next to the code they test
- Styles live next to the component they style

### 3. Explicit Public API
- Each vertical exports only what other verticals need via `index.ts`
- Internal files are private - other verticals MUST NOT deep-import them
- Think of each vertical as a mini-package

### 4. Import Rules
```
ALLOWED:
  vertical -> own internal files
  vertical -> shared/design-system
  vertical -> another vertical's public API (index.ts)
  app/pages -> any vertical's public API

FORBIDDEN:
  vertical -> another vertical's internal files (deep imports)
  shared -> any vertical (shared must be domain-agnostic)
```

### 5. Shared Code
- Code used by multiple verticals should become its own vertical
- Domain-agnostic shared code -> `design-system/` or `shared/`
- Don't use `shared/` as a dumping ground
- Each shared module should have clear, focused responsibility

### 6. Scaling
- Start with routes/pages as verticals
- When a sub-feature grows or is used elsewhere, promote it to its own vertical
- Align verticals with team ownership and CODEOWNERS

## Detection Patterns (Horizontal Smell)

These patterns indicate horizontal organization that should be refactored:

1. **Top-level type directories**: `src/components/`, `src/hooks/`, `src/utils/`, `src/types/`
2. **Scattered domain code**: `components/profile.tsx` + `hooks/useProfile.ts` + `utils/profileHelpers.ts`
3. **Grab-bag directories**: `utils/` with 50+ unrelated files
4. **No barrel files**: No `index.ts` to define public API per feature
5. **Cross-cutting imports**: Everything imports from everywhere with no restrictions
6. **Filename prefixes as grouping**: `profileCard.tsx`, `profileAvatar.tsx`, `profileSettings.tsx` all in `components/`

## Tooling for Enforcement

- **eslint-plugin-boundaries** - enforce which verticals can depend on what
- **Nx** - project dependency rules out of the box
- **pnpm workspaces** - `package.json` `exports` field defines public interfaces
- **import/no-restricted-paths** - ESLint rule to disable cross-feature deep imports
