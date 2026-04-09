---
description: Manage quick tips and tricks in .notes/hints/
user_invocable: true
---

# /hint - Tips & Tricks

Manage quick, actionable hints stored in `.notes/hints/`. Hints are short, focused tips - the kind of thing you'd put on a sticky note.

## Usage

- `/hint` - Show a random hint (or recent hints if few exist)
- `/hint add <title>` or `/hint <title>` - Add a new hint
- `/hint list` - List all hints
- `/hint list --tag <tag>` - Filter by tag
- `/hint view <slug>` - View a hint
- `/hint edit <slug>` - Edit a hint
- `/hint tags` - Show all hint tags

## Creating a Hint

1. Check `.notes/hints/` exists. If not, suggest `/notes-init`.
2. Filename: `slug.md` (no date prefix).
3. Frontmatter:

```yaml
---
title: "Short descriptive title"
type: hint
tags: [tool-or-topic]
created: YYYY-MM-DD
updated: YYYY-MM-DD
---
```

4. Content should be brief - ideally under 10 lines.
5. Format: straight to the point. Command, snippet, or one-paragraph explanation.

## Hint Content Format

Keep hints short and scannable:

```markdown
## Command/Tip

\`\`\`bash
the-command --with-flags
\`\`\`

**Why**: Brief explanation of when/why to use this.
```

Or for non-command hints:

```markdown
Brief explanation of the trick or technique.

**Example**: concrete example if helpful.
```

## Default `/hint` Behavior

When user just runs `/hint` with no arguments:
- If there are hints, pick one at random and display it (like "tip of the day")
- Show total hint count so user knows there are more

## Guidelines

- Hints should be atomic - one tip per file
- If a hint grows beyond ~15 lines, it probably belongs in `/kb` instead
- Tag hints by the tool/technology they relate to: `git`, `docker`, `vim`, `bash`, etc.
