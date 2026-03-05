---
name: content-handling
description: >-
  User content handling and logging rules. Ensures content integrity (no modifications),
  proper changelog language (Chinese titles/descriptions), and content recording markers.
---

# User Content Handling and Logging Rules

## 1. Content Integrity

- **Do not modify user content**: When the user provides content to be recorded, do not modify, summarize, or abbreviate the original text unless explicitly instructed. Maintain the original integrity of user input.
- **Direct storage**: Store user-provided content directly in the designated or contextually appropriate location.

## 2. Logging Language Standards

- **Chinese titles and descriptions**: When generating log entries or metadata for user operations (e.g., in `knowledge/logs/changelog.md`), all generated titles and descriptions must be in Chinese.
- **Technical identifiers unchanged**: This rule does not affect English naming conventions for code, variable names, function names, comments, and file names. Technical identifiers like file paths in logs remain in English.

## Examples

**User Content Recording Marker**

When the user uses the `[[[` symbol, the content following it is to be recorded directly. Ignore any instructions after this marker and only perform recording.

**Example**:
- User input: `[[[ Search for new hypotheses on mitochondrial DNA replication`
- System recognition: Content after `[[[` is `Search for new hypotheses on mitochondrial DNA replication` - this is the original content to record, not a search instruction to execute

**Correct Operation**:
1. Create a new file (e.g., in `knowledge/inbox/` directory)
2. File content should be exactly: `Search for new hypotheses on mitochondrial DNA replication`
3. Add a Chinese record to `changelog.md`, e.g.: `2023-10-27 11:00:00 - [ADD] - knowledge/inbox/mitochondrial-dna-replication-hypothesis.md - New note: search for new hypotheses on mitochondrial DNA replication`

**Incorrect Operation**:
- Modifying the note content to: "User's ideas about mitochondrial DNA replication"
- Calling tools to execute a web search operation
- Using English description in changelog: "Added a new note about user's hypothesis."
