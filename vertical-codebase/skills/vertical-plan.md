---
description: Create a step-by-step transformation plan from horizontal to vertical codebase
user_invocable: true
---

# /vertical-plan - Transformation Plan

Generate a detailed, actionable plan for restructuring a horizontal codebase into vertical architecture. Run `/vertical-audit` first to identify patterns.

## Usage

- `/vertical-plan` - Generate plan based on current `src/` structure
- `/vertical-plan <path>` - Generate plan for a specific directory

## Planning Steps

### Phase 1: Analyze Current State

1. Map the full directory structure under `src/`.
2. Identify all domain-related file clusters by scanning:
   - Filename prefixes across type directories
   - Import relationships between files
   - Route/page definitions
3. Identify truly shared code (used by 3+ unrelated domains).

### Phase 2: Define Target Verticals

For each proposed vertical, determine:

- **Name**: Clear domain-based name (`profile`, `dashboard`, `billing`)
- **Source files**: List every file that belongs in this vertical
- **Internal structure**: How files will be organized within the vertical
- **Public API**: What this vertical exports for others to use
- **Dependencies**: Which other verticals it imports from

### Phase 3: Generate Move Plan

Create a table of every file move:

```markdown
## Move Plan

### Vertical: `profile`

| Current Path | New Path | Type |
|-------------|----------|------|
| src/components/ProfileCard.tsx | src/profile/components/ProfileCard.tsx | component |
| src/components/ProfileAvatar.tsx | src/profile/components/ProfileAvatar.tsx | component |
| src/hooks/useProfile.ts | src/profile/hooks/useProfile.ts | hook |
| src/utils/profileHelpers.ts | src/profile/utils/profileHelpers.ts | util |
| src/types/profile.ts | src/profile/types/profile.ts | type |
| (new) | src/profile/index.ts | barrel |
```

### Phase 4: Import Update Plan

For each moved file:
1. List all files that import it (importers)
2. Show the old import path and new import path
3. Flag circular dependencies or problematic patterns

### Phase 5: Barrel File Definitions

For each vertical, define the `index.ts`:

```typescript
// src/profile/index.ts
export { ProfileCard } from './components/ProfileCard'
export { ProfileAvatar } from './components/ProfileAvatar'
export { useProfile } from './hooks/useProfile'
export type { ProfileData, ProfileSettings } from './types/profile'
```

### Phase 6: Shared Code Plan

- List files that don't belong to any specific vertical
- Propose: `design-system/` for domain-agnostic UI, `shared/` or `lib/` for utilities
- Each shared module should have focused responsibility

### Phase 7: Execution Order

Recommend a safe order of execution:

```markdown
## Execution Order

1. Create target directories (non-breaking)
2. Move shared/design-system code first (fewest dependencies)
3. Move verticals in dependency order (leaf verticals first)
4. Update all imports
5. Add barrel files
6. Remove empty old directories
7. Add ESLint boundary rules
8. Verify: run tests, type-check, lint
```

### Phase 8: Risk Assessment

Flag potential issues:
- **High risk**: Files imported by 20+ other files
- **Circular dependencies**: Cross-vertical circular imports
- **Framework constraints**: Files that must stay in specific locations (Next.js pages, etc.)
- **Test impacts**: Test files that need path updates
- **CI/CD impacts**: Build configs referencing old paths

## Guidelines

- Never propose moving framework-required files (Next.js app/, pages/, etc.)
- Preserve test co-location (tests should move with their source files)
- Identify a safe, incremental migration path - don't require a big bang refactor
- Consider git blame preservation (use `git mv` for moves)
- Plan should be executable by `/vertical-refactor` skill
