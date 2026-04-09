# Folder Notes Reference

## Canonical Convention

This skill uses the **Outside-Folder** convention for folder notes.

```text
Projects.md
Projects/
  Alpha.md
  Beta.md
  _assets/
```

- `Projects.md` is the folder note.
- `Projects/` contains the notes, subfolders, and shared assets for that topic.
- Treat the folder note as the overview, scope, and index for the adjacent folder.

## Creation Rules

When creating a new folder-backed topic:

1. Create the folder.
2. Create the adjacent folder note with the same basename.
3. Put child notes inside the folder, not beside it.
4. Keep the folder note outside the folder.

Example layout:

```text
Research.md
Research/
  Literature Review.md
  Interview Notes.md
```

## Interpretation Rules

When a folder note already exists:

- Read it before moving, renaming, or creating notes in the folder.
- Use it to infer the folder's purpose, naming style, and inclusion criteria.
- Preserve the existing folder-note pair instead of converting to an inside-folder or index-file layout.
- Update the folder note after organizing the folder so it remains accurate.

## Linking Folder Notes

Outside-folder folder notes are ordinary notes for linking purposes.

```markdown
[[Projects]]
[[Projects#Roadmap]]
[Projects](./Projects.md)
```

- Prefer `[[Projects]]` for internal vault links.
- Use standard Markdown links only when the output must be portable outside Obsidian.

## Embedding Folder Notes

```markdown
![[Projects]]
![[Projects#Roadmap]]
```

## Organizing Files By Folder Note

When asked to organize notes based on folder notes:

1. Find the adjacent folder note, such as `Projects.md` for `Projects/`.
2. Read the folder note before deciding where files belong.
3. Place related notes inside `Projects/`.
4. Keep shared folder-note assets inside `Projects/_assets/`.
5. Update `Projects.md` to reflect the new structure, links, or summary.

## Recommended Folder Note Content

Folder notes should usually include:

- a short description of the folder's purpose
- links to key child notes
- any naming or organization rules that apply inside the folder
- optional status, tags, or scope notes in frontmatter

Example folder note:

```markdown
---
title: Projects
tags:
  - hub
  - project
---

# Projects

This folder contains active project work and project-specific notes.

## Key Notes

- [[Projects/Alpha]]
- [[Projects/Beta]]

## Scope

- Keep one note per project inside `Projects/`.
- Store shared diagrams in `Projects/_assets/`.
```
