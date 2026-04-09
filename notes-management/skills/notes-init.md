---
description: Initialize the .notes/ directory structure in the current project
user_invocable: true
---

# /notes-init - Initialize Notes System

When the user runs `/notes-init`, set up the notes management system in the current project.

## Steps

1. Create the directory structure:
   ```
   .notes/
     notes/
     tasks/
     kb/
     hints/
     projects/
   ```

2. Create `.notes/.gitkeep` files in each empty subdirectory so git tracks them.

3. Create `.notes/README.md` with a brief overview of the directory structure and frontmatter format for quick reference.

4. Check if `.gitignore` exists. If it does, ask the user if they want to add `.notes/` to gitignore (some users prefer to version-control their notes, others don't). Do NOT modify `.gitignore` without asking.

5. Report what was created and suggest the user tries `/note`, `/task`, `/kb`, or `/hint` to start adding content.

## Important

- Do NOT overwrite existing `.notes/` directory or any files within it.
- If `.notes/` already exists, report that it's already initialized and show its current state.
