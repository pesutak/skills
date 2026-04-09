---
description: Create and manage tasks/TODOs in .notes/tasks/
user_invocable: true
---

# /task - Task Management

Manage tasks stored in `.notes/tasks/`.

## Usage

- `/task` - Show active tasks grouped by priority
- `/task add <title>` or `/task <title>` - Create a new task
- `/task done <slug>` - Mark task as done
- `/task list` - List all tasks (including done)
- `/task list --status active` - Filter by status
- `/task list --priority high` - Filter by priority
- `/task list --project <name>` - Filter by project
- `/task list --tag <tag>` - Filter by tag
- `/task edit <slug>` - Edit a task
- `/task archive` - Archive all done tasks

## Creating a Task

1. Check `.notes/tasks/` exists. If not, suggest `/notes-init`.
2. Filename: `YYYY-MM-DD-slug.md`
3. Frontmatter:

```yaml
---
title: "Task title"
type: task
tags: []
created: YYYY-MM-DD
updated: YYYY-MM-DD
status: active
priority: medium
project: ""
---
```

4. Ask for priority if not specified. Default is `medium`.
5. Ask for project association if relevant.
6. Content below frontmatter can include details, subtasks (as markdown checklists), context.

## Showing Active Tasks (default `/task`)

- Find all files in `.notes/tasks/`
- Filter to `status: active`
- Group by priority (high -> medium -> low)
- Show: title, tags, project, created date
- Format as a clean summary

## Marking Done

- Find task by slug (partial match OK)
- Set `status: done` and `updated` to today
- Show confirmation

## Archiving

- `/task archive` sets `status: archived` on all `done` tasks
- Update the `updated` field

## Guidelines

- Active tasks should always be quickly accessible (default command shows them)
- Keep task titles short and actionable (start with a verb)
- Use markdown checklists `- [ ]` for subtasks within a task file
