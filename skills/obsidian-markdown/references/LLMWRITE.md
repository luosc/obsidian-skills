# LLMwrite Block Processing

An `LLMwrite` fenced code block is a placeholder instructing the LLM to generate and insert Markdown content in place of the block itself.

## Detection

A code block whose language identifier is `LLMwrite`:

````markdown
```LLMwrite
Instructions for what content to generate here.
```
````

## Processing Rules

1. Extract the instructional text from inside the block.
2. Generate new Markdown content based **only** on those instructions, following all Obsidian Flavored Markdown conventions.
3. Replace the **entire** original block — opening fence (`` ```LLMwrite ``), all instruction lines, and closing fence (`` ``` ``) — with the generated content.
4. Do **not** wrap the generated content in a code block unless the instructions explicitly request one (e.g. "generate a Python snippet").
5. Ensure the replacement integrates seamlessly with surrounding content (correct blank lines, heading levels, etc.).

## Example

**Input note:**

````markdown
# Meeting Agenda

## Item 1: Review Previous Action Items

```LLMwrite
List the action items from the meeting on 2023-10-25.
Format as a Markdown task list.
- Action Item 1: John to follow up on X.
- Action Item 2: Jane to draft Y.
```

## Item 2: New Business
````

**Output note (after processing):**

```markdown
# Meeting Agenda

## Item 1: Review Previous Action Items

- [ ] John to follow up on X.
- [ ] Jane to draft Y.

## Item 2: New Business
```

## Example — Code Block Requested

**Input:**

````markdown
```LLMwrite
Generate a Python function that returns the nth Fibonacci number. Wrap it in a python code block.
```
````

**Output:**

````markdown
```python
def fibonacci(n: int) -> int:
    if n <= 1:
        return n
    a, b = 0, 1
    for _ in range(2, n + 1):
        a, b = b, a + b
    return b
```
````
