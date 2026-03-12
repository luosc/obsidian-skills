# Obsidian Tasks Plugin — Emoji Format Reference

The Tasks plugin parses task lines **backwards from the end of the line** for emoji metadata. Any unrecognised text after the last recognised emoji signifier stops the backward scan, so the order of elements is critical.

## Required Element Order (single line)

```
- [STATUS] #tag1 #tag2 Task description EMOJI_METADATA ^block-link
```

1. **Status** — `- [ ]` todo · `- [x]` done · `- [-]` cancelled
2. **Tags** (optional) — all tags space-separated, placed *before* the description
3. **Task description** — free text
4. **Emoji metadata block** — all emoji signifiers space-separated, placed *after* the description
5. **Block link** (optional) — e.g. `^blockid`, placed last

## Emoji Signifiers

### Dates (all must be `YYYY-MM-DD`)

| Signifier | Meaning | Example |
|-----------|---------|---------|
| `➕` | Created date | `➕ 2024-01-15` |
| `🛫` | Start date | `🛫 2024-01-16` |
| `⏳` | Scheduled date | `⏳ 2024-01-17` |
| `📅` | Due date | `📅 2024-01-20` |
| `✅` | Done date (`[x]` only) | `✅ 2024-01-21` |
| `❌` | Cancelled date (`[-]` only) | `❌ 2024-01-21` |

### Priority

| Signifier | Priority |
|-----------|----------|
| `🔺` | Highest |
| `⏫` | High |
| `🔼` | Medium |
| *(none)* | Normal |
| `🔽` | Low |
| `⏬️` | Lowest |

### Recurrence

```
🔁 every <interval> [on <days/dates>] [when done]
```

Examples:
- `🔁 every day`
- `🔁 every week on Monday`
- `🔁 every month on the last Friday when done`
- `🔁 every quarter`

> Recurring tasks **must** have at least one of `📅`, `⏳`, or `🛫` to function correctly.

### Task Identity & Dependencies

| Signifier | Meaning | Allowed characters |
|-----------|---------|-------------------|
| `🆔 id` | Unique task ID | Letters, numbers, `_`, `-` |
| `⛔ id1,id2` | Depends on these task IDs (comma-separated) | Same as above |

### On Completion

```
🏁 keep    (default — task is kept after completion)
🏁 delete  (task line is removed after completion)
```

> Avoid `🏁 delete` on tasks that have nested sub-items — it can corrupt the list structure.

## Recommended Metadata Order

For readability and consistent parsing, use this order within the emoji block:

```
➕ created  🛫 start  ⏳ scheduled  📅 due  ✅/❌ done/cancelled  PRIORITY  🔁 recurrence  🆔 id  ⛔ depends  🏁 action
```

## Output Format Rule

The generated task **must** be a single line inside a `markdown` fenced code block. No other text outside the block.

```markdown
- [ ] #tag Task description 📅 2024-01-20 ⏫
```

## Formatting Warnings

- **Single line only** — task metadata cannot span multiple lines.
- **Strict date format** — always `YYYY-MM-DD`. Convert natural language ("next Friday", "tomorrow") before output.
- **No interstitial text** — do not insert any descriptive text between an emoji signifier and its value, or between emoji pairs.
- **No `📅 today`** — the plugin does not resolve the word "today" in task strings; use the actual date.

## Examples

**High-priority task with due date, recurrence, and dependency:**

```markdown
- [ ] #finance #reporting Review the Q3 financial report 📅 2024-10-28 ⏫ 🔁 every quarter on the last Friday ⛔ q2_review_complete
```

**Low-priority scheduled task with tags:**

```markdown
- [ ] #client #meeting Schedule a follow-up meeting with the client ⏳ 2024-05-28 🔽
```

**Completed task with done date and ID:**

```markdown
- [x] Server maintenance completed ✅ 2024-05-23 🆔 server_maint_003
```

**Task with start date and delete-on-completion:**

```markdown
- [ ] #project_phoenix Draft initial project proposal 🛫 2024-06-03 🏁 delete
```

**Cancelled task:**

```markdown
- [-] Old feature implementation ❌ 2024-04-01
```
