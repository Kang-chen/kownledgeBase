---
name: notes-categorizer
description: >-
  Rules for categorizing notes into planning, programming, and biology directories.
  Includes keyword matching, file formatting, and special commands like TODO archival.
  Trigger on: 归档TODO, 周归档. Also handles [~] cancelled tasks.
---

# Notes Categorization Rules

This skill defines categorization rules for notes in the personal knowledge base, covering planning, programming, and biology domains.

## Planning Notes

**Target Directory**: `knowledge/planning/`

### Keywords
- "plan", "planning"
- "roadmap"
- "goal", "objective"
- "milestone"
- "strategy"

### Action
When these keywords are detected:
1. Generate a suitable filename based on the note's title
2. Add YAML frontmatter with `creation_date` and relevant `tags` (e.g., `planning`)
3. Save as Markdown file in `knowledge/planning/`

### Command: Archive TODO

Archives completed and cancelled todo items from `knowledge/planning/todolist.md` into weekly archive files.

**Two-Phase Execution** (both phases are mandatory):

#### Phase 1 — Scan & Preview

1. Read `knowledge/planning/todolist.md`
2. Identify archivable top-level items:
   - `[x]` — completed tasks (include all sub-tasks regardless of their status)
   - `[~]` — cancelled tasks (include all sub-tasks, mark as "已取消" in archive)
3. Never archive `[ ]` parent tasks, even if some sub-tasks are `[x]`
4. Present a preview table to the user:

```
| # | Status | Task | Sub-tasks |
|---|--------|------|-----------|
| 1 | ✅ 完成 | Task description | 3/3 |
| 2 | ❌ 取消 | Task description | 1/2 |
```

5. **Wait for user confirmation before proceeding to Phase 2**

#### Phase 2 — Execute Archive

1. Get current date via `date` command
2. Create directory `knowledge/planning/YYYY-MM/` if needed
3. Create or append to `knowledge/planning/YYYY-MM/completed-todos-YYYY-MM-DD-weekly.md`:

```yaml
---
creation_date: YYYY-MM-DD
tags: [planning, archive, completed-todos]
---
```

4. Write archived items preserving original markdown structure
5. For `[~]` items, prepend "**[已取消]**" to the task line
6. Remove archived items from source `todolist.md`
7. Append to `knowledge/logs/_changelog.md`
8. Present summary: N completed, M cancelled, archive file path

---

## Programming Notes

**Target Directory**: `knowledge/programming/`

### Keywords
- "code", "coding", "programming"
- "algorithm", "data structure"
- "Python", "JavaScript", "R", "SQL", "Java", "C++"
- "bug", "debug", "error"
- "API", "library", "framework"

### Action
When these keywords are detected:
1. Generate a suitable filename based on the note's title
2. Add YAML frontmatter with `creation_date` and relevant `tags` (e.g., `programming`, `python`)
3. Save as Markdown file in `knowledge/programming/`

### Code Formatting Rules

**All code must use code blocks**:
- Wrap all code in Markdown code blocks
- Always specify language type after opening backticks

**Correct**:
```python
def example_function(data):
    # This is an example function
    return len(data)
```

**Incorrect** (missing language specifier):
```
def example_function(data):
    return len(data)
```

---

## Biology Notes

**Target Directory**: `knowledge/biology/`

### Keywords
- "biology", "bioinformatics"
- "gene", "genome", "DNA", "RNA", "protein"
- "sequencing", "NGS"
- "analysis", "pipeline", "workflow"
- "research", "study", "experiment", "paper"

### Action
When these keywords are detected:
1. Generate a suitable filename based on the note's title
2. Add YAML frontmatter with `creation_date` and relevant `tags` (e.g., `biology`, `bioinformatics`)
3. Save as Markdown file in `knowledge/biology/`
