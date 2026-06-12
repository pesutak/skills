---
description: Audit a codebase for horizontal structure patterns and identify vertical groupings
user_invocable: true
---

# /vertical-audit - Codebase Structure Audit

Analyze the current codebase structure to identify horizontal (type-based) organization patterns and propose vertical (domain-based) groupings.

## Usage

- `/vertical-audit` - Full audit of `src/` directory
- `/vertical-audit <path>` - Audit a specific directory

## Audit Steps

### Phase 1: Map Current Structure

1. Use Glob to map the full directory tree under `src/` (or the given path).
2. Identify all top-level directories and categorize them:
   - **Type-based** (horizontal): `components/`, `hooks/`, `utils/`, `types/`, `constants/`, `helpers/`, `services/`, `models/`
   - **Domain-based** (vertical): feature-named directories like `dashboard/`, `profile/`, `auth/`
   - **Mixed**: directories that contain both patterns
   - **Shared/infra**: `design-system/`, `shared/`, `lib/`, `common/`

### Phase 2: Detect Horizontal Patterns

Scan for these anti-patterns and report each one found:

1. **Top-level type directories** - `src/components/`, `src/hooks/`, `src/utils/`, `src/types/`
   - Count files in each, list them
2. **Scattered domain code** - Related files split by type:
   - Search for filename patterns: e.g., files containing "profile" scattered across `components/profile*`, `hooks/useProfile*`, `utils/profile*`, `types/profile*`
   - Group these by domain name
3. **Grab-bag directories** - `utils/` or `helpers/` with 10+ unrelated files
4. **Filename prefix grouping** - Multiple files in same directory sharing a prefix (e.g., `profileCard.tsx`, `profileAvatar.tsx`)
5. **No public API boundaries** - Directories without `index.ts` barrel files
6. **Unrestricted imports** - Check for cross-cutting imports between unrelated areas (sample 10-20 files, trace import paths)

### Phase 3: Identify Natural Verticals

1. **Route-based**: Scan route/page definitions to find logical feature boundaries
2. **Prefix-based**: Group files by common name prefixes across type directories
3. **Import-based**: Trace import graphs to find clusters of tightly coupled files
4. **Team-based**: Check CODEOWNERS if it exists

### Phase 4: Score & Report

Generate a structured report:

```markdown
## Vertical Codebase Audit

### Structure Score: X/10
(10 = fully vertical, 1 = fully horizontal)

### Current Organization
- Type-based directories: N (list them)
- Domain-based directories: N (list them)
- Total files in src/: N

### Horizontal Patterns Found

#### 1. Type-Based Top-Level Directories
- `src/components/` - 45 files
- `src/hooks/` - 23 files
- `src/utils/` - 31 files
...

#### 2. Scattered Domain Code
- **profile**: components/profileCard.tsx, hooks/useProfile.ts, utils/profileHelpers.ts, types/profile.ts
- **dashboard**: components/dashboardWidget.tsx, hooks/useDashboard.ts, ...
...

#### 3. Import Boundary Violations
- No barrel files (index.ts) found in N directories
- Deep cross-imports detected: [examples]

### Proposed Verticals

| Vertical | Source Files | From Directories |
|----------|-------------|-----------------|
| `profile/` | 8 files | components, hooks, utils, types |
| `dashboard/` | 12 files | components, hooks, utils |
| `auth/` | 6 files | components, hooks, services |
| `design-system/` | 15 files | components (shared UI) |

### Recommended Next Step
Run `/vertical-plan` to generate a detailed transformation plan.
```

## Guidelines

- Be thorough but don't read every file - use Glob patterns and Grep to scan efficiently
- Focus on `src/` unless the project uses a different source root
- If the codebase is already mostly vertical, say so and suggest improvements
- For monorepos, audit each package separately
- Consider framework conventions (Next.js `app/` dir, Remix routes, etc.) when identifying verticals
