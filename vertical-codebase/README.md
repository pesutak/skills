# Vertical Codebase Skills

A set of Claude Code skills for analyzing, planning, and transforming codebases from horizontal (type-based) to vertical (domain-based) architecture.

Based on [The Vertical Codebase](https://tkdodo.eu/blog/the-vertical-codebase) by TkDodo.

## The Problem

Most codebases start with horizontal organization:

```
src/
  components/     # 200+ unrelated components
  hooks/          # all hooks in one pile
  utils/          # grab-bag of utilities
  types/          # all type definitions
```

This groups code by **what it is**, not **what it does**. Related code gets scattered across directories, boundaries are unclear, and the codebase becomes harder to navigate as it grows.

## The Solution

Vertical architecture groups code by **domain/feature**:

```
src/
  profile/        # everything profile-related
    components/
    hooks/
    utils/
    types/
    index.ts      # public API
  dashboard/
    components/
    hooks/
    index.ts
  design-system/  # shared, domain-agnostic UI
```

## Skills

| Skill | Command | Description |
|-------|---------|-------------|
| **vertical-audit** | `/vertical-audit` | Analyze codebase structure, detect horizontal patterns, propose verticals |
| **vertical-plan** | `/vertical-plan` | Generate detailed transformation plan with file moves and import updates |
| **vertical-refactor** | `/vertical-refactor` | Execute the transformation safely, one vertical at a time |
| **vertical-check** | `/vertical-check` | Validate boundaries, detect import violations, score vertical health |

## Workflow

```
/vertical-audit     ->  Understand current state, get a score
        |
/vertical-plan      ->  Get a detailed, actionable migration plan
        |
/vertical-refactor  ->  Execute the plan safely, vertical by vertical
        |
/vertical-check     ->  Validate everything, set up enforcement
```

## Key Rules

1. **Group by domain** - Each vertical owns all its code: components, hooks, types, utils
2. **Co-locate** - Code that changes together lives together
3. **Explicit public API** - Each vertical has an `index.ts` barrel file
4. **No deep imports** - Cross-vertical imports go through barrel files only
5. **Shared = domain-agnostic** - `design-system/` and `shared/` must not import from verticals
6. **Incremental** - Migrate one vertical at a time, verify after each

## Installation

```bash
# From your project directory
mkdir -p .claude/skills
cp /path/to/skills/vertical-codebase/skills/*.md .claude/skills/
cp /path/to/skills/vertical-codebase/CLAUDE.md .claude/vertical-codebase.md
```

## Enforcement Tooling

After transformation, enforce boundaries with:
- **[eslint-plugin-boundaries](https://github.com/javierbrea/eslint-plugin-boundaries)** - Define allowed dependencies between verticals
- **[import/no-restricted-paths](https://github.com/import-js/eslint-plugin-import/blob/main/docs/rules/no-restricted-paths.md)** - Prevent cross-feature deep imports
- **[Nx](https://nx.dev/)** - Project dependency rules for monorepos

## References

- [The Vertical Codebase](https://tkdodo.eu/blog/the-vertical-codebase) - TkDodo
- [Bulletproof React](https://github.com/alan2207/bulletproof-react) - Project structure guide
- [Feature-Sliced Design](https://feature-sliced.design/) - Architectural methodology
