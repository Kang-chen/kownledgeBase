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

**Three-Phase Execution** (Phase 0 and Phase 1 are preview-only; Phase 2 mutates files only after user confirmation):

#### Phase 0 — Format & Enrich Preview

Before archiving anything, present a review packet in the conversation instead of editing `todolist.md` or creating new planning files.

1. Read `knowledge/planning/todolist.md` and preserve the original task text.
2. Normalize the preview into the standard sections: `紧急任务`, `进行中任务`, `待规划任务`, plus any detected loose wiki-link headings.
3. Identify archivable top-level items:
   - `[x]` — completed tasks
   - `[~]` — cancelled tasks
   - Struck-through completed tasks, if present
4. Enrich each candidate with:
   - source section
   - related wiki links or file references
   - sub-task counts (`done/total`)
   - suggested archive reason
   - follow-up action, if the item changes ongoing priorities
5. Identify non-archivable but messy items:
   - active parents with completed sub-tasks
   - task headings with no checkbox
   - stale `进行中任务` older than two weeks when dates are available
   - tasks that should become project hubs or inbox chunks instead of completed TODO archive
6. Present the review packet to the user and wait for confirmation before Phase 1.
7. Do not create persistent dry-run documents by default. Only save a review packet if the user explicitly asks for a file.
8. If the user asks to save the review packet, write it in Chinese and place it in an agent-maintained review area, not beside durable planning notes.
9. Do not remove, rewrite, or reorder tasks in `todolist.md` during Phase 0.

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

5. Include the Phase 0 enrichment fields when presenting the preview table.
6. **Wait for user confirmation before proceeding to Phase 2**

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
9. If this is part of inbox/weekly archival, include only the TODO items that changed ongoing priorities in the final archive signal report. Do not list routine completed tasks unless they affect next actions.

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
