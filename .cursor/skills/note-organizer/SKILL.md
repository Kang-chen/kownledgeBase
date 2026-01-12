---
name: NoteOrganizer
description: Organizes notes into the personal knowledge base based on predefined rules. Use when the user wants to save, categorize, or organize notes and ideas.
---

# NoteOrganizer Skill

This skill is responsible for automatically categorizing and storing notes in the personal knowledge base.

## When to Use

- Use this skill when the user wants to save a new note or idea
- Use this skill when content needs to be organized into the knowledge base
- Use this skill when the user provides content prefixed with `[[[`
- This skill is helpful for maintaining a structured personal knowledge base

## Instructions

### Workflow

1. **Analyze Input**: Analyze the user's input to identify the main topic and relevant keywords.

2. **Consult Rules**: Read the rule files located in `.cursor/rules/` to determine the appropriate category and directory for the note. The rules are:
   - `knowledge_base_structure.mdc`: General structure and formatting.
   - `planning_notes.mdc`: For notes about planning, roadmaps, etc.
   - `programming_notes.mdc`: For notes about coding, algorithms, etc.
   - `biology_notes.mdc`: For notes about biology, bioinformatics, etc.
   - For SOP/Loop pages, use the `loop-page-creator` skill instead.

3. **Determine Target Path**: Based on the matching rule, determine the target directory (e.g., `knowledge/planning/`).

4. **Generate File Name**: Create a descriptive, hyphen-separated filename from the note's title (e.g., `my-new-research-plan.md`).

5. **Format Content**:
   - Add a YAML frontmatter block with `creation_date` and `tags`.
   - Ensure the content is in valid Markdown format.

6. **Save File**: Use the `edit_file` tool to create a new file with the formatted content in the determined target path.

7. **Update Changelog**: Add an entry to `knowledge/logs/changelog.md` with timestamp, operation type, and description in Chinese.

8. **Confirm**: Report the successful creation and location of the new note to the user.

## Directory Structure

```
/knowledge/
├── planning/      # Notes related to project planning, roadmaps, and goals.
├── programming/   # Notes related to software development, algorithms, and coding.
├── biology/       # Notes related to biology, bioinformatics, and research.
├── inbox/         # Inbox for unprocessed notes and ideas.
├── logs/          # Logs for tracking changes and activities.
├── SOP/           # Standard Operating Procedures and company Loop documentation.
├── private/       # Private notes, credentials references, and personal information.
└── travel/        # Travel plans and related notes.
```

**Note**: For SOP/Loop pages, use the `loop-page-creator` skill which follows company Loop standards.

## File Naming Convention

- File names should be in English, using lowercase letters
- Words should be separated by hyphens (`-`)
- The file extension should be `.md`
- Example: `my-research-notes.md`
