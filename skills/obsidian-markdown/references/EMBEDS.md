# Embeds Reference

## Embed Notes

```markdown
![[Note Name]]
![[Note Name#Heading]]
![[Note Name#^block-id]]
```

Folder notes use the same embed syntax as any other note. With the outside-folder convention, the folder note `Projects.md` for `Projects/` can be embedded like this:

```markdown
![[Projects]]
![[Projects#Roadmap]]
```

## Embed Images

```markdown
![[image.png]]
![[image.png|640x480]]    Width x Height
![[image.png|300]]        Width only (maintains aspect ratio)
```

## External Images

Standard Markdown image syntax with optional Obsidian resize extension:

```markdown
![Alt text](https://example.com/image.png)
![Alt text|300](https://example.com/image.png)          Width only (aspect ratio preserved)
![Alt text|640x480](https://example.com/image.png)      Width × Height
```

The `|width` and `|widthxheight` resize syntax works for both external images `![]()` and internal embeds `![[]]`.

## Embed Audio

```markdown
![[audio.mp3]]
![[audio.ogg]]
```

## Embed PDF

```markdown
![[document.pdf]]
![[document.pdf#page=3]]
![[document.pdf#height=400]]
```

## Embed Lists

```markdown
![[Note#^list-id]]
```

Where the list has a block ID:

```markdown
- Item 1
- Item 2
- Item 3

^list-id
```

## Embed Search Results

````markdown
```query
tag:#project status:done
```
````
