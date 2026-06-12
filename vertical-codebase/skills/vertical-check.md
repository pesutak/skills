---
description: Validate vertical codebase boundaries and import rules
user_invocable: true
---

# /vertical-check - Boundary Validation

Check that the codebase follows vertical architecture rules: no cross-vertical deep imports, proper barrel files, clean boundaries.

## Usage

- `/vertical-check` - Full validation of `src/`
- `/vertical-check <vertical>` - Check a specific vertical
- `/vertical-check --fix` - Auto-fix simple violations (update imports to use barrels)

## Checks Performed

### 1. Directory Structure Check

For each vertical directory in `src/`:
- Has `index.ts` barrel file? (REQUIRED)
- Barrel file exports a public API? (REQUIRED)
- No stale exports in barrel (exported items still exist)?

Report:
```
OK  src/profile/index.ts - exports 4 items
OK  src/dashboard/index.ts - exports 7 items
ERR src/billing/ - MISSING index.ts (no public API defined)
ERR src/auth/index.ts - exports useAuth which no longer exists
```

### 2. Import Boundary Check

Scan ALL source files for import statements. For each import, verify:

**Rule: No deep cross-vertical imports**
```typescript
// VIOLATION: deep import into another vertical
import { profileHelper } from '../profile/utils/helpers'
import { useProfileState } from '@/profile/hooks/useProfileState'

// CORRECT: import from barrel
import { profileHelper, useProfileState } from '@/profile'
import { ProfileCard } from '../profile'
```

**Rule: Shared code must not import from verticals**
```typescript
// VIOLATION: shared importing from a vertical
// file: src/shared/utils/format.ts
import { DashboardConfig } from '@/dashboard/types'

// CORRECT: shared must be domain-agnostic
```

**Rule: No circular vertical dependencies**
- Build a dependency graph: which verticals import from which
- Detect cycles: A -> B -> C -> A
- Report all cycles found

Report format:
```
## Import Boundary Violations

### Deep Imports (7 violations)

| File | Violation | Should Be |
|------|-----------|-----------|
| src/dashboard/Widget.tsx | @/profile/hooks/useProfile | @/profile |
| src/pages/Settings.tsx | ../auth/utils/validateToken | @/auth |

### Circular Dependencies (1 cycle)

dashboard -> profile -> dashboard
```

### 3. Orphan Detection

Find files that don't belong to any vertical:
- Files still in top-level type directories (`src/components/`, `src/hooks/`, etc.)
- Files in `src/` root that should be in a vertical
- Utility files not associated with any domain

### 4. Vertical Health Score

For each vertical, calculate:
- **Cohesion** (high = good): % of internal imports vs external
- **Coupling** (low = good): number of external verticals it depends on
- **API surface** (small = good): number of exports in barrel file vs total files
- **Completeness**: does it have its own types, tests, etc.?

```
## Vertical Health

| Vertical | Cohesion | Coupling | API Surface | Score |
|----------|----------|----------|-------------|-------|
| profile  | 85%      | 2 deps   | 4/12 files  | 9/10  |
| dashboard| 60%      | 5 deps   | 8/20 files  | 6/10  |
| auth     | 90%      | 1 dep    | 3/8 files   | 10/10 |
```

### 5. ESLint Config Suggestion

If no boundary enforcement is configured, suggest adding it:

**For eslint-plugin-boundaries:**
```javascript
// eslint.config.js
import boundaries from 'eslint-plugin-boundaries'

export default [
  {
    plugins: { boundaries },
    settings: {
      'boundaries/elements': [
        { type: 'shared', pattern: 'src/shared/*' },
        { type: 'feature', pattern: 'src/*', capture: ['feature'] },
        { type: 'app', pattern: 'src/app/*' },
      ],
    },
    rules: {
      'boundaries/element-types': [2, {
        default: 'disallow',
        rules: [
          { from: 'feature', allow: ['shared'] },
          { from: 'feature', allow: [['feature', { feature: '${from.feature}' }]] },
          { from: 'app', allow: ['shared', 'feature'] },
        ],
      }],
      'boundaries/entry-point': [2, {
        default: 'disallow',
        rules: [{ target: 'feature', allow: 'index.ts' }],
      }],
    },
  },
]
```

## Guidelines

- Scan efficiently - use Grep for import patterns rather than reading every file
- Respect framework conventions (Next.js, Remix, etc.)
- Don't flag imports from `node_modules` or absolute package imports
- Consider path aliases (`@/`, `~/`) when checking imports
- If `--fix` is used, only fix simple cases (update import path), never move files
