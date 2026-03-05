---
name: workspace-manager
description: >-
  Unified CLI for workspace, projects, and skills management.
  Use when users want to manage projects, check workspace status,
  or perform skill operations. Delegates skill commands to skill-manager.
---

# Workspace Manager

Unified CLI for managing workspace projects and skills.

## Quick Reference

| Task | Command |
|------|---------|
| Show workspace status | `ws` or `ws status` |
| List all projects | `ws project list` |
| Create new project | `ws project create <name>` |
| Show project details | `ws project info <name>` |
| Update project metadata | `ws project update <name>` |
| Skill operations | `ws skill <subcommand>` (delegates to skill-manager) |

## Usage Examples

**User:** "What projects do I have?"

```bash
python scripts/ws project list
```

**User:** "Create a new project called 'genome-analysis'"

```bash
python scripts/ws project create genome-analysis
```

**User:** "Show me the workspace status"

```bash
python scripts/ws status
```

## Project Commands

### ws status
Displays overall workspace status including:
- Active projects count
- Recent activity
- Skills count
- Directory health

### ws project list
Lists all projects from `knowledge/projects/_index.md` with:
- Project name
- Status (active/archived)
- Last modified date

### ws project create <name>
Creates a new project with:
- Entry in `_index.md`
- Project metadata file in `knowledge/projects/`
- Optional subdirectories in content areas

### ws project info <name>
Shows detailed project information:
- Description
- Related directories
- Keywords
- Activity history

### ws project update <name>
Updates project metadata interactively.

## Skill Commands

All `ws skill <subcommand>` commands are delegated to the `skill-manager` skill.

Examples:
- `ws skill list` → `skills list`
- `ws skill sync` → `skills sync`
- `ws skill search "query"` → `skills search "query"`

## Configuration

Config file: `config.json`

```json
{
  "projects": {
    "index_file": "knowledge/projects/_index.md",
    "metadata_dir": "knowledge/projects/",
    "content_dirs": {
      "programming": "knowledge/programming/",
      "biology": "knowledge/biology/",
      "planning": "knowledge/planning/"
    }
  },
  "skills": {
    "delegate_to": "skill-manager",
    "source_dir": ".agent/skills/"
  }
}
```

## Directory Structure

```
workspace-manager/
├── SKILL.md           # This file
├── config.json        # Configuration
├── scripts/
│   └── ws             # Main CLI script
├── templates/         # Project templates
└── references/
    └── skill-creator/ # Embedded skill creation guidelines
```
