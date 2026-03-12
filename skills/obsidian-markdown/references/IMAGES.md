# Image Insertion Workflow

When inserting images into a note `{note-name}.md`, follow this three-step process.

## Step 1 — Create the Asset Folder

Create a folder with the **same name as the note, without the `.md` extension**, in the same directory as the note. This keeps assets co-located and avoids cluttering the vault root.

```bash
mkdir "{note-name}"
```

If the folder already exists, skip this step.

## Step 2 — Copy Images into the Asset Folder

**Single image:**

```bash
cp "{source-path}/{image-file}" "./{note-name}/"
```

**All images from a folder:**

```bash
cp "{image-folder}"/* "./{note-name}/"
```

If it is unclear which images from a folder are relevant, ask the user to confirm before copying.

## Step 3 — Insert Markdown Citations

For each copied image, add a citation to the note. Use a path **relative to the note file**. URL-encode spaces in folder or file names (`My Note` → `My%20Note`).

```markdown
![{Descriptive alt text}](./{note-name}/{image-file})
```

### With an Italic Caption

Place the caption in italics on the line immediately before the image, then add a blank line after the image:

```markdown
_{Caption describing the image}_
![{Descriptive alt text}](./{note-name}/{image-file})

```

### With a Specific Display Width

Append `|{width}` or `|{width}x{height}` to the alt text (Obsidian-specific resize syntax):

```markdown
![{Alt text}|600](./{note-name}/{image-file})
![{Alt text}|640x480](./{note-name}/{image-file})
```

## Full Example

Note file: `Research Notes.md`  
Images to insert: `diagram.png`, `chart.svg`

```bash
mkdir "Research Notes"
cp "/tmp/exports/diagram.png" "/tmp/exports/chart.svg" "./Research Notes/"
```

Markdown to insert into `Research Notes.md`:

```markdown
_Figure 1: System architecture overview._
![System architecture diagram](./Research%20Notes/diagram.png)

_Figure 2: Performance results by quarter._
![Performance chart](./Research%20Notes/chart.svg)

```
