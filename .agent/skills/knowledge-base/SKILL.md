---
name: knowledge-base
description: >-
  Personal knowledge base structure and organization rules. Defines directory layout,
  file naming conventions, content format, time recording, and changelog requirements.
---

# Personal Knowledge Base Structure

This document outlines the structure and rules for the personal knowledge base agent.

## Directory Structure

All knowledge base articles are stored under the `knowledge/` directory:

```
/knowledge/
  ├── projects/      # Project metadata (Agent Memory) - see project-organization skill
  ├── planning/      # Notes related to project planning, roadmaps, and goals.
  ├── programming/   # Notes related to software development, algorithms, and coding.
  │   ├── {project}/       # Project-specific subdirectories
  │   └── commands/        # Common commands and scripts (no project affiliation)
  ├── biology/       # Notes related to biology, bioinformatics, and research.
  │   └── {project}/       # Project-specific subdirectories
  ├── inbox/         # Inbox for unprocessed notes and ideas (user writes here via Typora).
  ├── archive/       # Immutable archive of original inbox files, organized by month (YYYY-MM/).
  ├── _memory/       # Agent memory: structured knowledge entries extracted from archive.
  │   ├── _index.md        # Knowledge index (Agent loads this first for retrieval)
  │   └── entries/         # Individual knowledge entry files
  ├── digests/       # Weekly review summaries (_YYYY-Www-digest.md).
  ├── logs/          # Logs for tracking changes and activities.
  ├── SOP/           # Standard Operating Procedures and company Loop documentation.
  ├── private/       # Private notes, credentials references, and personal information.
  └── travel/        # Travel plans and related notes.
```

## `_` Prefix Naming Convention

Files and directories prefixed with `_` are Agent-maintained metadata (indexes, logs, digests, memory store). Users should not manually edit these — Agent updates them automatically during operations.

Examples:
- `logs/_changelog.md` — Agent-appended operation log
- `projects/_index.md` — Agent-maintained project index
- `_memory/` — Agent memory directory (knowledge entries + index)
- `_memory/_index.md` — Agent memory index
- `digests/_YYYY-Www-digest.md` — Agent-generated weekly digest

Files **without** `_` prefix are user-editable content:
- `todolist.md`, `inbox/*.md`, `_memory/entries/*.md`, `planning/YYYY-MM/completed-todos-*.md`

## Subdirectory Organization Principle

To prevent directory clutter:

- **Project priority**: Create subdirectories by project (e.g., `programming/{project}/`)
- **Threshold rule**: Create subdirectory when same-category files exceed **5**
- **Naming**: Use lowercase English, named by project or topic
- **Related resources**: `.assets` folder should be in same directory as corresponding `.md` file
- **Archival**: When archiving from inbox, load project index (`knowledge/projects/_index.md`) first

## SOP Directory (Loop Pages)

The `SOP/` directory contains company Loop workspace documentation:
- Follow the company's Loop SOP guidelines in `SOP/Loop.md`
- Use tag-based naming: `[Doc]`, `[SOP]`, `[Log]`, `[Ref]`, `[Draft]`
- Structure: Module → Entity → Perspective
- Required elements: Abstract, TOC, Body, Metadata

## File Naming Convention

- File names should be in English, using lowercase letters
- Words should be separated by hyphens (`-`)
- The file extension should be `.md`
- Example: `my-research-notes.md`

## Content Format

- All notes must be in Markdown format
- Each note should start with a level 1 heading (`#`) containing the title
- Use metadata at the top of the file (YAML frontmatter) to store tags, creation date, etc.

## Content Consistency

- When adding content to a file, maintain format and style consistency with existing content

## Time Recording Requirements

When recording timestamps or dates in notes:
- **Always use the `date` command to get the current system time**
- Use the format: `YYYY-MM-DD` for dates
- Use the format: `YYYY-MM-DD HH:MM:SS` for timestamps
- Run `date` command before creating or updating notes to ensure accurate time recording

Example:
```markdown
---
creation_date: YYYY-MM-DD
last_modified: YYYY-MM-DD HH:MM:SS
tags: [example, tag]
---

# Title of the Note

This is the content of the note.
```

## Changelog and Logging Requirements

- **All file operations (creation, modification, deletion) must be logged**
- A detailed entry must be added to the changelog at `knowledge/logs/_changelog.md`
- Each changelog entry must include:
  - A timestamp (`YYYY-MM-DD HH:MM:SS`)
  - The type of operation (e.g., `[ADD]`, `[UPDATE]`, `[DELETE]`)
  - The full path to the affected file
  - A brief description of the change in Chinese
- Refer to the content-handling skill for detailed logging format and language specifications
