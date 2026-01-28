---
name: project-organization
description: >-
  Project-based file organization and rule loading guidance. Defines project memory system,
  file structure by project, and mandatory rule loading workflow for archival tasks.
---

# Project Organization Rules

## Project Memory System

Project metadata is stored in the `knowledge/projects/` directory as Agent "memory".

### Memory Loading

When executing archival tasks, **must first read**:
```
knowledge/projects/_index.md
```

This index file contains:
- Active project list
- Project keyword mapping (for automatic file attribution)
- Project-related directory structure

## Project-Based File Structure

```
knowledge/
├── projects/                    # Project metadata (Agent Memory)
│   ├── _index.md               # Project index (must read)
│   ├── {project-a}.md          # Project A details
│   └── {project-b}.md          # Project B details
├── programming/
│   ├── {project-a}/            # Project A - code related
│   ├── {project-b}/            # Project B - code related
│   └── commands/               # Common commands (no project affiliation)
├── biology/
│   ├── {project-a}/            # Project A - biology related
│   └── {project-b}/            # Project B - biology related
└── planning/
    └── (organized by date/project TODO)
```

## Rule Loading Guidance (On-Demand Loading)

### Mandatory Rule Loading

When executing the following operations, **must proactively use Read tool to load** corresponding rule files:

| Operation Type | Must Load Rules |
|----------------|-----------------|
| Archive inbox / organize files | `knowledge/projects/_index.md` (project index) |
| Create/edit planning files | notes-categorizer skill (planning section) |
| Create/edit programming files | notes-categorizer skill (programming section) |
| Create/edit biology files | notes-categorizer skill (biology section) |
| Handle sensitive information | private-info skill |
| Update changelog | changelog skill |

### Why Proactive Loading?

Cursor's rule loading mechanism:
- `alwaysApply: true` → Always loaded
- `alwaysApply: false` + `globs` → **Only loaded when matching files are opened**

Therefore, when processing inbox files, target directory rules won't auto-load; Agent must proactively read them.

## Inbox Archival Workflow

1. **Load project index**: `Read: knowledge/projects/_index.md`
2. **Analyze file content**: Identify topics and keywords
3. **Match project**: Determine project affiliation based on keyword mapping
4. **Load target rules**: Read corresponding skill based on target directory
5. **Determine target path**: `{category}/{project}/` or `{category}/`
6. **Execute archival**: Move file, update frontmatter, log to changelog
7. **Update project index**: Update `_index.md` if new project created

## Subdirectory Organization

To prevent directory clutter:
- **Threshold**: Create subdirectory when same-category files exceed 5
- **Naming**: Lowercase English, named by project/topic
- **Resource files**: `.assets` folder in same directory as corresponding `.md` file
