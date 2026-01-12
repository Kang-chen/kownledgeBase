---
name: loop-page-creator
description: Creates Loop Page documents following company SOP standards. Use when the user wants to create a new Loop page, meeting record, project document, or any documentation following the company's Loop workspace structure and conventions.
---

# Loop Page Creator

This skill creates Loop Page documents that follow the company's standard documentation guidelines.

## When to Use

- When user wants to create a new Loop page
- When user needs a meeting record template
- When user needs a project document
- When user mentions "Loop", "SOP", or company documentation
- When user wants to create documentation following team standards

## Instructions

### Step 1: Load SOP Reference

Before creating any Loop page, read the SOP file to understand current standards:

```
knowledge/SOP/Loop.md
```

This file contains:
- Workspace structure overview
- Structure Design Guidelines (Module → Entity → Perspective)
- Document Writing Guidelines
- Naming conventions with tags: `[Doc]`, `[SOP]`, `[Log]`, `[Ref]`, `[Draft]`
- Page structure requirements (Abstract, TOC, Metadata)

### Step 2: Determine Document Type

Ask user or infer from context:
- `[Doc]` - General documentation or knowledge
- `[SOP]` - Standard Operating Procedure (step-by-step)
- `[Log]` - Immutable record of events
- `[Ref]` - Reference sheets or dictionaries
- `[Draft]` - Work in progress

### Step 3: Generate File Name

Follow naming convention:
- Format: `[tag]-brief-description.md`
- Use lowercase letters
- Use hyphens to separate words
- Examples: `doc-lamindb-architecture.md`, `sop-database-restore.md`

### Step 4: Apply Page Structure

Every Loop page must include:

1. **Abstract** (Quote Block) - First element
   - Help reader decide in 5 seconds if this is the right document
   - Use `> 💡` format

2. **Table of Contents** - After abstract
   - List of H1, H2, H3 headers

3. **Body Content** - Main content sections

4. **Metadata** - At the end
   - Status: Stable / Draft / Needs Update / Deprecated / In Review
   - Maintainer: Person responsible
   - Last updated: Date

### Step 5: Create the Document

Use this template structure:

```markdown
> 💡 Abstract
>
> [Brief summary of document purpose and target audience]

- TOC entries...

# 1. Context & Goal

[Why this document exists]

- Objective: [Goal]
- Prerequisites: [Requirements]

# 2. Implementation / Steps

[Main content]

## 2.1 [Step/Section]

[Details]

# 3. Reference / Troubleshooting

[Optional: Links, error codes, FAQ]

---

Status: [Draft/Stable/etc.]
Maintainer: [Name]
Last updated: [Date]
```

### Step 6: Save and Log

1. Save to appropriate directory under `knowledge/`
2. Add entry to `knowledge/logs/changelog.md`
3. Report success to user

## Icon Reference

For directory/container naming:
- 💾 Storage / Database
- ☁️ Cloud / Infrastructure  
- 🔀 Pipeline / Workflow
- 🧪 Experiments / Protocols

For perspectives:
- 📘 User Guide / QuickStart
- 🔧 Maintenance / Admin
- 💻 Developer / Internals
- 📝 Logs / Records

## Important Notes

- 🚫 No sensitive information in plain text
- 🔗 Link to existing docs instead of duplicating
- 🧹 Deprecate documents, don't delete them
- Keep directory nesting to 3-4 levels max
