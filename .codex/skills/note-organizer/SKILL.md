---
name: note-organizer
description: >-
  Organizes notes into the personal knowledge base based on predefined rules.
  Use when the user wants to save, categorize, or organize notes and ideas,
  or when content is prefixed with `[[[`.
  Trigger on: 周回顾, weekly review (runs full weekly review workflow),
  or any inbox processing request.
  Proactive: On Fridays/weekends when last review was more than 5 days ago, suggest "本周还未进行周回顾".
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
├── automation/    # Agent-generated automation reports and weekly summaries.
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
2. **Present review packet first**: classify each inbox file or folder into chunks before moving content
3. **Analyze file content**: Identify topics, keywords, source links, and unfinished context
4. **Match project**: Determine project affiliation based on keyword mapping
5. **Load target rules**: Read corresponding skill based on target directory
6. **Determine target path**: `{category}/{project}/`, `knowledge/planning/`, a confirmed project hub, or archive-only
7. **Execute only confirmed chunks**: Archive or extract approved chunks; leave active chunks in inbox or a hub
8. **Update project index**: If new project/hub is created, update `_index.md`

### Inbox Chunk Packet Rules

Use chunk packets for any inbox file that is large, mixed-topic, unfinished, or project-like.

Each chunk must be assigned exactly one status:
- `archive-ready`: finished context that can be moved or extracted now
- `active-context`: unfinished work that must remain easy to resume
- `reference-only`: supporting material that should be linked from a hub or memory entry, not expanded in reports
- `needs-user-decision`: ambiguous material that should not be moved until the user confirms

Chunk packet requirements:
- Preserve the original inbox file unless the user confirms a move.
- Record `source`, `suggested target`, `project/topic`, `reason`, and `next action` for every chunk.
- Prefer partial archival over all-or-nothing archival for large files.
- For folders, classify reports, scripts, raw data, figures, and generated assets separately.
- Never treat an unfinished project note as blocked from archival; archive completed chunks and keep the remaining context active.
- Present chunk packets in the conversation by default. Do not create persistent dry-run documents unless the user explicitly asks for a saved file.
- If a saved packet is requested, write it in Chinese and place it in an agent-maintained review area, not in a domain directory such as `planning/`, `biology/`, or `programming/`.

### Project Hub Rules

Recommend a hub when an inbox item spans multiple review cycles, contains recurring weekly work, or mixes plans, reports, scripts, and decisions. Create or update the hub only after the user confirms that it should become a durable note.

Confirmed hub files should live in the most natural active area, usually:
- `knowledge/planning/{topic}-hub.md` for workflows, recurring reports, and project management
- `knowledge/projects/{project}.md` when it represents durable project metadata
- `knowledge/programming/{project}/` or `knowledge/biology/{project}/` for domain-specific reusable work

Hub content should be concise, written in Chinese unless source terminology requires otherwise:
- current status
- active questions
- latest useful links
- archived chunks with source paths
- next review decision

## Weekly Review (周回顾)

Unified entry point for periodic knowledge base maintenance.

### Flow

0. **Lightweight Harness Scan**: Report clutter risks before moving files
1. **TODO Archive**: Load notes-categorizer skill, run Archive TODO Phase 0 review packet, then confirmed archive phases
2. **Inbox Processing**: Run chunk-packet inbox workflow (below)
3. **Health Check**: Report counts per area + overdue alerts
4. **Digest**: Generate `knowledge/automation/_YYYY-Www-digest.md`, log to changelog

### Lightweight Harness Scan

Before TODO or inbox archival, present a scan report in the conversation. This is advisory and must not block work.

Flag:
- `temp.md`, `Temp.md`, unnamed notes, and root-level generated reports
- files under `knowledge/tmp/`
- large files or folders in active areas
- generated assets, screenshots, browser/runtime artifacts, and ad-hoc scripts
- inbox folders that combine reports, code, data, and figures
- possible sensitive config or credentials outside `knowledge/private/`
- untracked or dirty inner-repo state that should be committed in batches

For each candidate, recommend one action: `keep`, `archive`, `private`, `delete-after-confirmation`, or `needs-review`.

Do not create a persistent scan report by default. Save one only when the user explicitly asks for a file, and keep it outside durable domain-note directories.

### Inbox Processing Workflow (Chunk-First Phases)

#### Phase 0 — Chunk Review Packet

Do this before moving any inbox file.

1. Read `knowledge/projects/_index.md` and `knowledge/_memory/_index.md`.
2. Classify each inbox item into chunk statuses: `archive-ready`, `active-context`, `reference-only`, `needs-user-decision`.
3. Identify suggested project hubs for recurring or unfinished work; do not create them yet.
4. Present the chunk packet and wait for confirmation before moving, extracting, or deleting anything.
5. Keep original files intact during Phase 0.
6. Keep Phase 0 output ephemeral unless the user explicitly asks to persist it.

#### Phase 1 — Archive Original Files

Preserve original content integrity:
- Move only user-confirmed `archive-ready` files or chunks to `knowledge/archive/YYYY-MM/` with date prefix
- Move associated `.assets/` directories alongside their files only when their parent file is moved
- Do not modify file content
- If only part of a file is `archive-ready`, create an extracted archive note that cites the original source; keep the original inbox file until the user confirms pruning

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

#### Phase 4 — Weekly Archive Signal Report

After TODO and inbox processing, produce a short decision-oriented report. Do not list every extracted item.

Use this filter:
1. **AAR**: What was intended? What happened? What changed in practice? What should be reused next time?
2. **Reuse class**: command, process, skill, agent, method, or archive-only.
3. **Skill decision**: recommend `create skill`, `refine existing skill`, `memory only`, or `defer`.
4. **Priority**: use an adapted RICE check: reuse frequency, impact, confidence, and maintenance effort.

Report format:

```markdown
## 本周值得注意
1. <one reusable insight>
   Decision: create skill | refine existing skill | memory only | defer
   Why: <one sentence>
   Next: <one concrete action>
```

Rules:
- Include at most 3 recommendations.
- Include at most 2 memory-only notes.
- Do not recommend a new skill unless the trigger and steps are clear.
- Prefer refining an existing skill over creating a new one when the workflow is already covered.
- Avoid broad lists such as "commands/processes/agents/methods" unless each item has a decision and next action.

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
