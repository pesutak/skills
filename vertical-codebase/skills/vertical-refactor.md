---
description: Execute vertical codebase transformation - move files and update imports
user_invocable: true
---

# /vertical-refactor - Execute Transformation

Execute the actual transformation of a horizontal codebase to vertical structure. Should be run AFTER `/vertical-audit` and `/vertical-plan`.

## Usage

- `/vertical-refactor` - Execute full plan interactively (confirms each vertical)
- `/vertical-refactor <vertical>` - Transform a single vertical (e.g., `profile`)
- `/vertical-refactor --dry-run` - Show what would change without making changes

## IMPORTANT: Safety Rules

1. **Always ask for confirmation** before starting the refactor
2. **Work one vertical at a time** - complete it fully before moving to the next
3. **Use `git mv`** for file moves to preserve git history
4. **Run tests after each vertical** to catch breakage early
5. **Commit after each vertical** so changes can be reverted individually
6. **Never move framework-required files** (Next.js pages, app directory, etc.)

## Execution Steps (Per Vertical)

### Step 1: Create Directory Structure

```bash
mkdir -p src/<vertical>/components
mkdir -p src/<vertical>/hooks
mkdir -p src/<vertical>/utils
mkdir -p src/<vertical>/types
```

Only create subdirectories that will have files. If a vertical has only 3-4 files total, a flat structure is fine:

```
src/profile/
  ProfileCard.tsx
  useProfile.ts
  types.ts
  index.ts
```

### Step 2: Move Files

For each file in the plan:
1. `git mv <old-path> <new-path>`
2. Track the move for import updates

### Step 3: Update Imports in Moved Files

For each moved file, update its internal imports:
- Relative imports to other files within the same vertical - update relative paths
- Imports from other verticals - update to use barrel imports if available
- Imports from shared/design-system - keep as-is or update path

### Step 4: Update External Imports

Find all files outside the vertical that import from moved files:
1. Use Grep to search for old import paths
2. Update each import to use the new path (prefer barrel import from `index.ts`)

### Step 5: Create Barrel File

Create `src/<vertical>/index.ts` exporting the public API:

```typescript
// Only export what other verticals actually need
export { ProfileCard } from './components/ProfileCard'
export { useProfile } from './hooks/useProfile'
export type { ProfileData } from './types'
```

### Step 6: Verify

1. Run TypeScript type-check (`npx tsc --noEmit` or project-specific command)
2. Run tests (`npm test` or project-specific command)
3. Run linter (`npm run lint` or project-specific command)
4. If anything fails, fix it before proceeding

### Step 7: Commit

Create a focused commit for this vertical:

```
refactor: move <vertical> to vertical structure

Move all <vertical>-related code from scattered type-based directories
into a cohesive src/<vertical>/ vertical with explicit public API.
```

## Handling Edge Cases

### File used by multiple verticals
- If clearly owned by one vertical - move there, others import via barrel
- If truly shared - move to `shared/` or `design-system/`

### Circular imports detected
- Stop and report the circular dependency
- Suggest extracting shared dependency into its own module
- Ask user how to proceed

### Test files
- Move test files alongside their source
- Update test imports accordingly

### CSS/Style files
- Move with their component
- Update any path references in the styles

### Type-only files
- If types are used only within the vertical - move inside the vertical
- If types define shared contracts - keep in `shared/types/` or create a types vertical

## Guidelines

- Always prefer small, verifiable steps over large batch operations
- If the codebase has no tests, warn the user about increased risk
- Keep detailed log of every file moved for rollback purposes
- If any step fails, stop and ask the user before continuing
- After all verticals are done, clean up empty directories
