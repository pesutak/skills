---
description: Create, list, edit, and view personal notes in .notes/
user_invocable: true
---

# /note - Personal Notes Management

Manage personal notes stored in `.notes/notes/`.

## Usage

- `/note` - List recent notes (last 10)
- `/note add <title>` or `/note <title>` - Create a new note
- `/note list` - List all notes
- `/note list --tag <tag>` - Filter by tag
- `/note view <slug>` - View a note
- `/note edit <slug>` - Edit a note
- `/note archive <slug>` - Archive a note
- `/note tags` - List all used tags

## Creating a Note

1. Check `.notes/notes/` exists. If not, suggest running `/notes-init` first.
2. Generate filename: `YYYY-MM-DD-slug.md` (use today's date, derive slug from title).
3. Ask the user for content if not provided. For quick notes, the user may provide everything in one line.
4. Create the file with proper frontmatter:

```yaml
---
title: "The title"
type: note
tags: []
created: YYYY-MM-DD
updated: YYYY-MM-DD
status: active
---
```

5. Before finalizing tags, scan existing notes for commonly used tags and suggest relevant ones.
6. Write the content below the frontmatter.

## Listing Notes

- Use Glob to find files in `.notes/notes/`
- Read frontmatter to show title, tags, date, status
- Sort by date (newest first)
- Support filtering by tag (scan frontmatter for matching tags)

## Viewing a Note

- Find the file by slug (partial match is OK - match against filenames)
- Display the full content

## Editing a Note

- Find and read the existing note
- Apply the requested changes
- Update the `updated` field in frontmatter

## Archiving

- Set `status: archived` in frontmatter
- Do NOT delete the file

## Guidelines

- If `.notes/` doesn't exist, suggest `/notes-init` instead of creating it ad-hoc
- Always preserve existing frontmatter fields when editing
- When suggesting tags, check existing notes for consistency
