# Notes Management Skills

A set of Claude Code skills for managing personal notes, tasks, knowledge base, and quick hints - all stored as plain markdown files with YAML frontmatter.

## What's Included

| Skill | Command | Description |
|-------|---------|-------------|
| **notes-init** | `/notes-init` | Initialize `.notes/` directory in your project |
| **note** | `/note` | Create, list, edit personal notes |
| **task** | `/task` | Task/TODO management with priorities |
| **kb** | `/kb` | Knowledge base - reference material by topic |
| **hint** | `/hint` | Quick tips and tricks |
| **notes-search** | `/notes-search` | Search across all entry types |

## How It Works

- Notes are stored as **markdown files** in `.notes/` directory
- Each file has **YAML frontmatter** with metadata (title, tags, type, status, dates)
- Organization via **directories** (by type) and **tags** (cross-cutting)
- Everything is **grep-friendly** - no database, no special tooling needed
- Files are **version-controllable** with git

### Storage Structure

```
.notes/
  notes/              # Personal notes, ideas
  tasks/              # Tasks and TODOs
  kb/                 # Knowledge base entries
  hints/              # Quick tips and tricks
  projects/           # Project-specific notes
    my-project/
```

### Frontmatter Example

```yaml
---
title: "Docker multi-stage builds"
type: kb
tags: [docker, ci, optimization]
created: 2026-04-09
updated: 2026-04-09
status: active
related: ["dockerfile-best-practices"]
---
```

## Installation

### Option 1: Copy skills to your project (recommended)

```bash
# From your target project directory:
mkdir -p .claude/skills

# Copy all notes management skills
cp /path/to/skills/notes-management/skills/*.md .claude/skills/

# Copy the conventions file to your project root or .claude/
cp /path/to/skills/notes-management/CLAUDE.md .claude/notes-system.md
```

Then add to your project's `CLAUDE.md`:

```markdown
See .claude/notes-system.md for notes management conventions.
```

### Option 2: Reference from settings.json

Add to your Claude Code `settings.json`:

```json
{
  "skills": {
    "notes-management": "/path/to/skills/notes-management/skills/"
  }
}
```

### Option 3: Quick start (single project)

```bash
# Clone the skills repo
git clone <repo-url> ~/skills

# Symlink the skills directory
ln -s ~/skills/notes-management/skills/ .claude/skills/notes-management
```

## Quick Start

After installation:

1. Run `/notes-init` to create the `.notes/` directory structure
2. Run `/note My first note` to create a note
3. Run `/task Set up CI pipeline` to create a task
4. Run `/kb Docker cheatsheet` to add a KB entry
5. Run `/hint` to see a random tip
6. Run `/notes-search docker` to search across everything

## Design Principles

- **Plain text**: Everything is markdown. Readable without tools.
- **No dependencies**: Works with just Claude Code's built-in file tools.
- **Portable**: Copy files between machines, projects, or Claude instances.
- **Git-friendly**: Version control your knowledge if you want.
- **Searchable**: Frontmatter + markdown = grepable structure.
- **Composable**: Use individual skills or all together.
