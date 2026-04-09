---
description: Manage knowledge base entries in .notes/kb/
user_invocable: true
---

# /kb - Knowledge Base

Manage reference knowledge stored in `.notes/kb/`. KB entries are long-lived reference material organized by topic.

## Usage

- `/kb` - List all KB entries grouped by tags
- `/kb add <title>` or `/kb <title>` - Create a new entry
- `/kb view <slug>` - View an entry
- `/kb edit <slug>` - Edit an entry
- `/kb list` - List all entries
- `/kb list --tag <tag>` - Filter by tag
- `/kb tags` - Show all tags with entry counts
- `/kb archive <slug>` - Archive outdated entry

## Creating a KB Entry

1. Check `.notes/kb/` exists. If not, suggest `/notes-init`.
2. Filename: `slug.md` (no date prefix - KB entries are reference material, not time-bound).
3. Frontmatter:

```yaml
---
title: "Entry title"
type: kb
tags: [category-tag, specific-tag]
created: YYYY-MM-DD
updated: YYYY-MM-DD
status: active
related: []
---
```

4. Tags are crucial for KB entries - they serve as the primary organization method. Always suggest relevant tags from existing entries.
5. Use `related` to link to other KB entries by slug.
6. Content should be well-structured with headings, code blocks, and examples.

## Listing Entries

- Default view: group by the most common tags (showing each entry under its primary tag)
- Show: title, tags, last updated
- Sort alphabetically within groups

## Content Guidelines for KB Entries

- Focus on ONE topic per entry
- Use clear headings: `## Problem`, `## Solution`, `## Examples`, `## References`
- Include code examples where applicable
- Keep content current - update rather than create duplicates
- Use `related` frontmatter to cross-reference related entries

## Guidelines

- Before creating a new entry, search existing KB for duplicates/overlap
- Suggest merging if a similar entry exists
- KB entries should be written as reference material (timeless, not diary-style)
