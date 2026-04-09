---
description: Search across all notes, tasks, KB entries, and hints
user_invocable: true
---

# /notes-search - Cross-type Search

Search across everything in `.notes/` - notes, tasks, KB entries, and hints.

## Usage

- `/notes-search <query>` - Full-text search across all entries
- `/notes-search --tag <tag>` - Find all entries with a specific tag
- `/notes-search --type <type>` - Filter by type (note, task, kb, hint)
- `/notes-search --project <name>` - Find all entries for a project
- `/notes-search --status <status>` - Filter by status
- `/notes-search --recent [days]` - Show entries from last N days (default: 7)

## Search Strategy

### Full-text Search
1. Use Grep to search file contents in `.notes/` for the query pattern
2. Also search frontmatter (titles, tags) for matches
3. Show results grouped by type with: title, type, tags, file path
4. Sort by relevance (title match > tag match > content match)

### Tag Search
1. Use Grep to find `tags:` lines in frontmatter containing the tag
2. Read matching files to extract title and metadata
3. Group results by type

### Project Search
1. Search for `project: "name"` in frontmatter
2. Also check `.notes/projects/<name>/` directory
3. Show all related entries

### Recent Entries
1. Use Glob to find all `.md` files in `.notes/`
2. Filter by `created` or `updated` date in frontmatter
3. Sort newest first

## Output Format

Display results as a clean table or list:

```
## Search Results for "docker"

### KB (2 matches)
- docker-compose-tips      [docker, devops]     Updated: 2026-04-01
- dockerfile-best-practices [docker, ci]        Updated: 2026-03-15

### Hints (1 match)
- docker-prune             [docker, cleanup]    Created: 2026-03-20

### Tasks (1 match)
- 2026-04-05-migrate-to-docker  [docker, infra]  Status: active
```

## Guidelines

- Always search both content and frontmatter
- For short queries (1-2 words), also try partial/fuzzy matches on slugs and titles
- Show file paths so user can follow up with `/note view`, `/kb view`, etc.
- If no results found, suggest alternative search terms or browsing by tag
