---
name: obsidian-markdown
description: Create and edit Obsidian Flavored Markdown with wikilinks, folder notes, embeds, callouts, properties, Tasks plugin emoji format tasks, image insertion, and LLMwrite block processing. Use when working with .md files in Obsidian, or when the user mentions wikilinks, folder notes, callouts, frontmatter, tags, embeds, tasks, or Obsidian notes.
---

# Obsidian Flavored Markdown Skill

Create and edit valid Obsidian Flavored Markdown. Obsidian extends CommonMark and GFM with wikilinks, folder-note workflows, embeds, callouts, properties, comments, and other syntax. This skill covers only Obsidian-specific extensions -- standard Markdown (headings, bold, italic, lists, quotes, code blocks, tables) is assumed knowledge.

## Git Sync (Important)

After completing any changes to an Obsidian vault:

1. Check whether the vault directory contains a `.git` folder (i.e. is a git repository).
2. If yes, **ask the user** whether they want to commit and sync the changes before doing anything.
3. Only proceed with git operations if the user explicitly approves.
4. When approved, the default workflow is:
   ```bash
   git pull --rebase
   git add -A
   git commit -m "openclaw {YYYY-MM-DD HH:MM}: {descriptive summary of changes}"
   git push
   ```
5. Never commit or push automatically without user confirmation.

## Output Formatting Rules

- All Markdown examples or generated note content **MUST** be enclosed in a fenced code block with the `markdown` language identifier.
- Template variables (placeholders for the user or another LLM to fill in) use curly braces: `{variable_name}`. This avoids ambiguity with wikilink `[[ ]]` and standard link `[ ]( )` syntax.
- Always insert an empty line before any table so Obsidian renders it correctly.

## Link Handling

| Situation | Format to use |
|-----------|--------------|
| Linking to a note within the same vault | `[[Note Name]]` (wikilink — Obsidian tracks renames) |
| Linking to an external URL | `[Text](https://example.com)` |
| Linking to a note from LLM output meant to be portable | `[Text](path/to/Note.md)` (standard Markdown, URL-encode spaces as `%20`) |

**Understanding wikilinks in input:** `[[Note]]`, `[[Note|Alias]]`, `[[Note#Heading]]`, `[[Note#^blockID]]` — all are valid and must be understood.

## Folder Notes

This skill treats a folder note as the note that represents and scopes an adjacent folder. The canonical convention is **Outside-Folder**:

```text
Projects.md
Projects/
  Alpha.md
  Beta.md
```

- Treat `Projects.md` and `Projects/` as a paired folder-note unit when they share the same basename and parent directory.
- Preserve the outside-folder convention. Do not create `Projects/Projects.md` or `Projects/_about_.md` unless the vault already uses a different convention and the user explicitly wants to keep it.
- When organizing files into a folder, read the folder note first and use it to infer the folder's purpose, naming style, and placement rules. See [FOLDER_NOTES.md](references/FOLDER_NOTES.md).
- When creating a new topical folder, create or update both the folder note and the folder contents:
  - `Projects.md` is the overview/index note.
  - `Projects/` holds the child notes and assets.
- After moving or creating child notes, update the folder note so it stays the overview, scope, and link hub for the folder.
- Link to an outside-folder folder note with `[[Projects]]`. Use an exact path only when disambiguation is needed.

## Workflow: Creating an Obsidian Note or Folder Note

1. **Add frontmatter** with properties (title, tags, aliases) at the top of the file. See [PROPERTIES.md](references/PROPERTIES.md) for all property types.
2. **Create or recognize folder notes** with the outside-folder convention when the note represents a folder or when the task is to organize files by folder. See [FOLDER_NOTES.md](references/FOLDER_NOTES.md).
3. **Write content** using standard Markdown for structure, plus Obsidian-specific syntax below.
4. **Link related notes** using wikilinks (`[[Note]]`) for internal vault connections, or standard Markdown links for external URLs.
5. **If organizing notes under a folder**, read the folder note first and update it after adding, moving, or renaming child notes.
6. **Embed content** from other notes, images, or PDFs using the `![[embed]]` syntax. See [EMBEDS.md](references/EMBEDS.md) for all embed types.
7. **Add callouts** for highlighted information using `> [!type]` syntax. See [CALLOUTS.md](references/CALLOUTS.md) for all callout types.
8. **Handle tasks** using standard task lists or the Tasks plugin Emoji Format. See [TASKS.md](references/TASKS.md) for full emoji format rules.
9. **Process LLMwrite blocks** by replacing them with generated Markdown content. See [LLMWRITE.md](references/LLMWRITE.md) for rules.
10. **Insert images** following the asset-folder workflow. See [IMAGES.md](references/IMAGES.md) for the full process.
11. **Verify** the note renders correctly in Obsidian's reading view.

## Internal Links (Wikilinks)

```markdown
[[Note Name]]                          Link to note
[[Note Name|Display Text]]             Custom display text
[[Note Name#Heading]]                  Link to heading
[[Note Name#^block-id]]                Link to block
[[#Heading in same note]]              Same-note heading link
[[Projects]]                           Link to outside-folder folder note
```

Define a block ID by appending `^block-id` to any paragraph:

```markdown
This paragraph can be linked to. ^my-block-id
```

For lists and quotes, place the block ID on a separate line after the block:

```markdown
> A quote block

^quote-id
```

## Embeds

Prefix any wikilink with `!` to embed its content inline:

```markdown
![[Note Name]]                         Embed full note
![[Note Name#Heading]]                 Embed section
![[image.png]]                         Embed image
![[image.png|300]]                     Embed image with width
![[document.pdf#page=3]]               Embed PDF page
![[Projects]]                          Embed outside-folder folder note
```

See [EMBEDS.md](references/EMBEDS.md) for audio, video, search embeds, and external images.

## Callouts

```markdown
> [!note]
> Basic callout.

> [!warning] Custom Title
> Callout with a custom title.

> [!faq]- Collapsed by default
> Foldable callout (- collapsed, + expanded).
```

Common types: `note`, `tip`, `warning`, `info`, `example`, `quote`, `bug`, `danger`, `success`, `failure`, `question`, `abstract`, `todo`.

See [CALLOUTS.md](references/CALLOUTS.md) for the full list with aliases, nesting, and custom CSS callouts.

## Properties (Frontmatter)

```yaml
---
title: My Note
date: 2024-01-15
tags:
  - project
  - active
aliases:
  - Alternative Name
cssclasses:
  - custom-class
---
```

Default properties: `tags` (searchable labels), `aliases` (alternative note names for link suggestions), `cssclasses` (CSS classes for styling).

See [PROPERTIES.md](references/PROPERTIES.md) for all property types, tag syntax rules, and advanced usage.

## Tags

```markdown
#tag                    Inline tag
#nested/tag             Nested tag with hierarchy
```

Tags can contain letters, numbers (not first character), underscores, hyphens, and forward slashes. Tags can also be defined in frontmatter under the `tags` property.

## Comments

```markdown
This is visible %%but this is hidden%% text.

%%
This entire block is hidden in reading view.
%%
```

## Obsidian-Specific Formatting

```markdown
==Highlighted text==                   Highlight syntax
```

## Math (LaTeX)

```markdown
Inline: $e^{i\pi} + 1 = 0$

Block:
$$
\frac{a}{b} = c
$$
```

## Diagrams (Mermaid)

````markdown
```mermaid
graph TD
    A[Start] --> B{Decision}
    B -->|Yes| C[Do this]
    B -->|No| D[Do that]
```
````

To link Mermaid nodes to Obsidian notes, add `class NodeName internal-link;`.

## Footnotes

```markdown
Text with a footnote[^1].

[^1]: Footnote content.

Inline footnote.^[This is inline.]
```

## Tasks (Obsidian Tasks Plugin — Emoji Format)

When converting natural language to a task or managing tasks with the Tasks plugin, use the Emoji Format. Full rules in [TASKS.md](references/TASKS.md).

Quick reference — strict element order, all on one line:

```
- [STATUS] #tag1 #tag2 Task description ➕ created 🛫 start ⏳ scheduled 📅 due PRIORITY 🔁 recurrence 🆔 id ⛔ depends 🏁 action
```

- Status: `- [ ]` todo · `- [x]` done · `- [-]` cancelled
- All dates: `YYYY-MM-DD` — convert natural language dates before output
- Priority: `🔺` highest · `⏫` high · `🔼` medium · _(none)_ normal · `🔽` low · `⏬️` lowest
- `✅` done date only on `[x]` tasks; `❌` cancelled date only on `[-]` tasks
- Recurring tasks (`🔁`) need at least one of `📅`, `⏳`, or `🛫`
- Output: single task line inside a `markdown` code block, nothing else outside it

## LLMwrite Blocks

When a note contains an `LLMwrite` fenced code block, replace the **entire block** (opening fence, instructions, closing fence) with newly generated Markdown content. Do not wrap the output in any code block unless the instructions explicitly request it. See [LLMWRITE.md](references/LLMWRITE.md) for examples.

## Complete Example

````markdown
---
title: Project Alpha
date: 2024-01-15
tags:
  - project
  - active
status: in-progress
---

# Project Alpha

This project aims to [[improve workflow]] using modern techniques.

> [!important] Key Deadline
> The first milestone is due on ==January 30th==.

## Tasks

- [x] Initial planning
- [ ] Development phase
  - [ ] Backend implementation
  - [ ] Frontend design

## Notes

The algorithm uses $O(n \log n)$ sorting. See [[Algorithm Notes#Sorting]] for details.

![[Architecture Diagram.png|600]]

Reviewed in [[Meeting Notes 2024-01-10#Decisions]].
````

## References

- [Obsidian Flavored Markdown](https://help.obsidian.md/obsidian-flavored-markdown)
- [Internal links](https://help.obsidian.md/links)
- [Embed files](https://help.obsidian.md/embeds)
- [Callouts](https://help.obsidian.md/callouts)
- [Properties](https://help.obsidian.md/properties)
- [Folder Note Plugin](https://github.com/xpgo/obsidian-folder-note-plugin)
- [Tasks plugin docs](https://publish.obsidian.md/tasks/)
