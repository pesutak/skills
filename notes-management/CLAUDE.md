# Notes Management System - Conventions

## Storage

All notes are stored in `.notes/` directory relative to the project root.

### Directory Structure

```
.notes/
  notes/              # Personal notes, ideas, thoughts
  tasks/              # Tasks and TODOs
  kb/                 # Knowledge base - reference material
  hints/              # Quick tips, tricks, shortcuts
  projects/           # Project-specific notes (subdirectory per project)
    <project-name>/
```

### File Naming

- Time-based entries (notes, tasks): `YYYY-MM-DD-slug.md`
- Reference entries (kb, hints): `slug.md`
- Slug: lowercase, hyphens, no spaces: `my-important-note.md`

### Frontmatter Schema

Every file MUST have YAML frontmatter:

```yaml
---
title: "Entry title"
type: note | task | kb | hint
tags: [tag1, tag2]
created: YYYY-MM-DD
updated: YYYY-MM-DD
status: active | done | archived     # for tasks; notes use active/archived
priority: low | medium | high        # optional, primarily for tasks
project: "project-name"             # optional, links to a project
related: ["other-slug"]             # optional, cross-references
---
```

### Tagging Rules

- Lowercase, hyphenated: `machine-learning`, `dev-ops`, `quick-fix`
- Reuse existing tags - before creating a new tag, check what already exists
- Combine broad + specific: `[python, async, performance]`
- To find existing tags: scan frontmatter across `.notes/` files

### Content Guidelines

- Write in markdown
- Use headings for structure within longer notes
- Keep KB entries focused on one topic
- Hints should be short and actionable
- Tasks should have clear, actionable titles

### Search & Discovery

- All files are plain markdown - searchable via grep/glob
- Frontmatter enables structured queries (by tag, type, status, date, project)
- File naming enables chronological browsing
- `related` field enables cross-referencing between entries
