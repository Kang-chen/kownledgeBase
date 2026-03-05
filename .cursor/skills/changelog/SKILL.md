---
name: changelog
description: >-
  Rules for maintaining the changelog file. Defines log format, required fields,
  and update triggers for knowledge base file operations.
---

# Changelog Rules

This skill defines the format and maintenance process for `knowledge/logs/_changelog.md`, which tracks all significant changes to the knowledge base.

## Log Format

Each entry in the changelog must be in Markdown format and include:
- **Timestamp**: The date and time of the change in `YYYY-MM-DD HH:MM:SS` format
- **Action**: The type of action performed (e.g., `CREATE`, `UPDATE`, `DELETE`, `MOVE`, `ARCHIVE`, `EXTRACT`)
- **Path**: The full path to the file that was changed
- **Description**: A brief summary of the changes made (in Chinese)

### Example Entries

```markdown
- **2026-03-04 10:00:00** | `CREATE` | `knowledge/biology/new-research-note.md` | 新增蛋白质折叠研究笔记
- **2026-03-04 10:05:00** | `ARCHIVE` | `inbox/model_tuning.md` → `archive/2026-03/2026-03-04-model-tuning.md` | 归档原始笔记到 archive
- **2026-03-04 10:10:00** | `EXTRACT` | `_memory/entries/xgboost-tuning.md` | 从归档文件提取知识条目：XGBoost 调参策略
```

### Action Keywords

| Action | Usage |
|--------|-------|
| `CREATE` | New file created |
| `UPDATE` | Existing file modified |
| `DELETE` | File removed |
| `MOVE` | File relocated (include source → destination) |
| `ARCHIVE` | Inbox file moved to `archive/` (include source → destination) |
| `EXTRACT` | Knowledge entry extracted to `_memory/entries/` from an archived file |

## Update Trigger

The `NoteOrganizer` agent (or any other agent making changes) must append a new entry to `knowledge/logs/_changelog.md` whenever a file is created, updated, or deleted within the `knowledge/` directory.

## Best Practices

1. Always get current timestamp using system `date` command before logging
2. Use consistent action keywords from the table above
3. Keep descriptions concise but informative
4. Include both source and destination paths for `MOVE` and `ARCHIVE` operations
