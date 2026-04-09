# Properties (Frontmatter) Reference

Properties use YAML frontmatter at the start of a note:

```yaml
---
title: My Note Title
date: 2024-01-15
tags:
  - project
  - important
aliases:
  - My Note
  - Alternative Name
cssclasses:
  - custom-class
status: in-progress
rating: 4.5
completed: false
due: 2024-02-01T14:30:00
---
```

## Property Types

| Type | Example |
|------|---------|
| Text | `title: My Title` |
| Number | `rating: 4.5` |
| Checkbox | `completed: true` |
| Date | `date: 2024-01-15` |
| Date & Time | `due: 2024-01-15T14:30:00` |
| List | `tags: [one, two]` or YAML list |
| Links | `related: "[[Other Note]]"` (quotes required) |

## Default Properties

- `tags` - Note tags (searchable, shown in graph view)
- `aliases` - Alternative names for the note (used in link suggestions)
- `cssclasses` - CSS classes applied to the note in reading/editing view

## Tags

```markdown
#tag
#nested/tag
#tag-with-dashes
#tag_with_underscores
```

Tags can contain: letters (any language), numbers (not first character), underscores `_`, hyphens `-`, forward slashes `/` (for nesting).

In frontmatter:

```yaml
---
tags:
  - tag1
  - nested/tag2
---
```

## Internal Links in Properties

Text and list properties that contain internal links **must** have the value enclosed in double quotes. Obsidian adds these automatically in the GUI, but they must be added manually when editing raw Markdown or using templates.

```yaml
---
link: "[[Other Note]]"
linklist:
  - "[[Note A]]"
  - "[[Note B]]"
---
```

The same quoting rule applies to folder notes that use the outside-folder convention:

```yaml
---
hub: "[[Projects]]"
related:
  - "[[Projects/Alpha]]"
  - "[[Projects/Beta]]"
---
```

## Folder Notes

For an outside-folder folder note such as `Projects.md` paired with `Projects/`, the note is still a normal Markdown file for frontmatter purposes.

```yaml
---
title: Projects
aliases:
  - Project Hub
tags:
  - hub
  - project
---
```

- Keep the filename aligned with the folder basename so the folder-note pair stays obvious.
- Use `aliases` when you want alternative display names without changing the folder-note filename.

## JSON Properties (Input Only)

Obsidian can parse properties written as a JSON object inside the frontmatter delimiters:

```markdown
---
{ "tags": ["journal"], "publish": false }
---
```

**Understand** JSON properties when they appear in input. **Never generate** JSON properties — always output YAML. Obsidian converts JSON to YAML on save.
