# Skills

A collection of Claude Code skill packs for various workflows.

## Available Skill Packs

### [Notes Management](./notes-management/)

Personal knowledge management system - notes, tasks, knowledge base, and hints. All stored as plain markdown with YAML frontmatter.

**Skills**: `/notes-init`, `/note`, `/task`, `/kb`, `/hint`, `/notes-search`

## Installation

Each skill pack has its own installation instructions in its README. The general approach:

1. Copy the skill `.md` files to your project's `.claude/skills/` directory
2. Copy the conventions `CLAUDE.md` to your project
3. Start using the slash commands

## Structure

```
skills/
  <skill-pack-name>/
    README.md          # Documentation & installation guide
    CLAUDE.md          # System conventions (for target project)
    skills/            # Skill files (slash commands)
      *.md
```