---
name: notes-categorizer
description: >-
  Rules for categorizing notes into planning, programming, and biology directories.
  Includes keyword matching, file formatting, and special commands like TODO archival.
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

### Command: Archive TODO (Weekly)

Archives completed todo items from all markdown files in `knowledge/planning/` into weekly archive files.

**Workflow:**
1. **Scan Files**: Scan all `.md` files in `knowledge/planning/`
2. **Identify Completed Todos**: Items starting with `- [x]` or `- [X]` within past week
3. **Get Current Week/Month**: Use system time for `YYYY-MM` folder and `YYYY-MM-DD` file
4. **Create Monthly Folder**: Create `knowledge/planning/YYYY-MM/` if needed
5. **Create Archive File**: Create `completed-todos-YYYY-MM-DD-weekly.md`
6. **Migrate Todos**: Preserve original structure with file headings
7. **Remove from Source**: Delete migrated items from original files
8. **Log Changes**: Append to `knowledge/logs/changelog.md`

**Invocation**:
- Trigger: `归档TODO`
- Options:
  - `--week=YYYY-MM-DD` (default: current week)
  - `--dry-run` (preview only)

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
