---
name: note-organizer
description: >-
  Organizes notes into the personal knowledge base based on predefined rules.
  Use when the user wants to save, categorize, or organize notes and ideas,
  or when content is prefixed with `[[[`.
  Trigger on: 周回顾, weekly review (runs full weekly review workflow),
  or any inbox processing request.
  Proactive: On Fridays/weekends when last review was >5 days ago, suggest "本周还未进行周回顾".
---

# NoteOrganizer Skill

This skill is responsible for automatically categorizing and storing notes in the personal knowledge base.

## Workflow Steps

1. **Analyze Input**: Analyze the user's input to identify the main topic and relevant keywords.

2. **Load Project Memory**: Read project metadata files from `knowledge/projects/` to understand active projects and their file organization.
   - **Must read**: `knowledge/projects/_index.md` (project index)

3. **Consult Skills** (Must Read):
   > **Important**: Must use Read tool to proactively load these skill files, do not rely on auto-loading!

   **Required skills** (based on target directory):
   - `notes-categorizer` skill: Contains rules for planning, programming, biology notes
   - `project-organization` skill: For multi-file archival, contains project organization rules
   - `knowledge-base` skill: General structure and formatting
   - `content-handling` skill: For content integrity and logging language rules
   - For SOP/Loop pages, use the `loop-page-creator` skill instead

4. **Identify Project**: Match content keywords to active projects (see project-organization skill).

5. **Determine Target Path**: Based on the matching rule and project, determine the target directory:
   - With project affiliation: `knowledge/{category}/{project}/`
   - Without project affiliation: `knowledge/{category}/`

6. **Generate File Name**: Create a descriptive, hyphen-separated filename from the note's title (e.g., `my-new-research-plan.md`).

7. **Format Content**:
   - Add a YAML frontmatter block with `creation_date`, `tags`, and `project` (if applicable)
   - Ensure the content is in valid Markdown format

8. **Save File**: Use the `edit_file` tool to create a new file with the formatted content in the determined target path.

9. **Update Changelog**: Add entry to `knowledge/logs/_changelog.md` following the changelog skill format.

10. **Confirm**: Report the successful creation and location of the new note to the user.

## Directory Structure

```
/knowledge/
├── projects/      # Project metadata (Agent Memory)
├── planning/      # Notes related to project planning, roadmaps, and goals.
├── programming/   # Notes related to software development, algorithms, and coding.
│   ├── {project}/       # Project-specific subdirectories
│   └── commands/        # Common commands (no project affiliation)
├── biology/       # Notes related to biology, bioinformatics, and research.
│   └── {project}/       # Project-specific subdirectories
├── inbox/         # Inbox for unprocessed notes and ideas.
├── archive/       # Immutable archive of original inbox files, organized by month.
├── _memory/       # Agent memory: structured knowledge entries extracted from archive.
│   ├── _index.md  # Knowledge index (Agent loads this first)
│   └── entries/   # Individual knowledge entry files
├── digests/       # Weekly review summaries.
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

## Inbox Archival Workflow

1. **Load project index**: `Read: knowledge/projects/_index.md`
2. **Analyze file content**: Identify topics and keywords
3. **Match project**: Determine project affiliation based on keyword mapping
4. **Load target rules**: Read corresponding skill based on target directory
5. **Determine target path**: `{category}/{project}/` or `{category}/`
6. **Execute archival**: Move file, update frontmatter, record changelog
7. **Update project index**: If new project, update `_index.md`

## Weekly Review (周回顾)

Unified entry point for periodic knowledge base maintenance.

### Flow

1. **TODO Archive**: Load notes-categorizer skill, run Archive TODO two-phase flow
2. **Inbox Processing**: Run three-phase inbox workflow (below)
3. **Health Check**: Report counts per area + overdue alerts
4. **Digest**: Generate `knowledge/digests/_YYYY-Www-digest.md`, log to changelog

### Inbox Processing Workflow (Three Phases)

#### Phase 1 — Archive Original Files

Preserve original content integrity:
- Move each `inbox/*.md` file to `knowledge/archive/YYYY-MM/` with date prefix
- Move associated `.assets/` directories alongside their files
- Do not modify file content

#### Phase 2 — Extract Knowledge Entries

Create Agent-searchable memory from archived files:
- Read each archived original file
- Extract independent knowledge entries by type:
  - `finding`: experimental results, analysis conclusions
  - `procedure`: reusable step-by-step workflows
  - `decision`: decisions made and their rationale
  - `reference`: reference materials, tool configurations
- Each entry file goes to `knowledge/_memory/entries/` with structured frontmatter:

```yaml
---
title: "Entry title"
type: finding|procedure|decision|reference
project: ProjectName
tags: [tag1, tag2]
keywords: [keyword1, keyword2]
source: archive/YYYY-MM/original-file.md
created: YYYY-MM-DD
confidence: high|medium|low
---
```

- **Present extraction preview to user, wait for confirmation before writing**

#### Phase 3 — Update Indexes

- Append new entries to `knowledge/_memory/_index.md` table
- Update `knowledge/projects/_index.md` if new projects found
- Log all operations to `knowledge/logs/_changelog.md`

### Git Auto-Commit

After each phase completes, create a git commit in the `knowledge/` inner repo:
- Present commit message to user for confirmation before committing
- Format: `archive: 归档 N 项TODO + 处理 M 个inbox文件 (YYYY-MM-DD)`
- Only commit in `knowledge/` inner repo, do not affect outer repo

## Task Lifecycle

- New tasks enter `待规划任务` or `紧急任务` section
- Starting work: move to `进行中任务`
- Completed: mark `[x]`, archived during weekly review
- Cancelled: mark `[~]`, also archived during weekly review (labeled as "已取消")
- Keep `紧急任务` under 5 items
- Evaluate `进行中任务` that exceed 2 weeks

## Related Skills

- `knowledge-base` - Directory structure and formatting rules
- `notes-categorizer` - Category-specific rules (planning, programming, biology)
- `project-organization` - Project-based organization and rule loading
- `content-handling` - Content integrity and logging language rules
- `changelog` - Changelog format and maintenance
- `private-info` - Sensitive information handling
- `loop-page-creator` - For SOP/Loop pages
