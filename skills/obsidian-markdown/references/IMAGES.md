# Image Insertion Workflow

When inserting images into a note, choose the asset folder based on whether the note is a regular note or an outside-folder folder note.

## Step 1 — Choose the Asset Folder

### Regular Note

For a regular note `{note-name}.md`, create a folder with the **same name as the note, without the `.md` extension**, in the same directory as the note.

```bash
mkdir "{note-name}"
```

### Outside-Folder Folder Note

For a folder note `{folder-name}.md` paired with `{folder-name}/`, store assets inside the adjacent folder under `_assets/`. This avoids colliding with the folder itself.

```bash
mkdir -p "{folder-name}/_assets"
```

If the destination folder already exists, skip creation.

## Step 2 — Copy Images into the Asset Folder

Use the chosen destination folder from Step 1.

**Single image:**

```bash
cp "{source-path}/{image-file}" "./{asset-folder}/"
```

**All images from a folder:**

```bash
cp "{image-folder}"/* "./{asset-folder}/"
```

If it is unclear which images from a folder are relevant, ask the user to confirm before copying.

## Step 3 — Insert Markdown Citations

For each copied image, add a citation using a path **relative to the note file**. URL-encode spaces in folder or file names (`My Note` → `My%20Note`).

### Regular Note

```markdown
![{Descriptive alt text}](./{note-name}/{image-file})
```

### Outside-Folder Folder Note

```markdown
![{Descriptive alt text}](./{folder-name}/_assets/{image-file})
```

### With an Italic Caption

Place the caption in italics on the line immediately before the image, then add a blank line after the image:

```markdown
_{Caption describing the image}_
![{Descriptive alt text}](./{asset-folder}/{image-file})

```

### With a Specific Display Width

Append `|{width}` or `|{width}x{height}` to the alt text (Obsidian-specific resize syntax):

```markdown
![{Alt text}|600](./{asset-folder}/{image-file})
![{Alt text}|640x480](./{asset-folder}/{image-file})
```

## Full Examples

Regular note: `Research Notes.md`  
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

Outside-folder folder note: `Projects.md` paired with `Projects/`

```bash
mkdir -p "Projects/_assets"
cp "/tmp/exports/roadmap.png" "./Projects/_assets/"
```

Markdown to insert into `Projects.md`:

```markdown
_Roadmap for the active projects folder._
![Projects roadmap](./Projects/_assets/roadmap.png)

```
