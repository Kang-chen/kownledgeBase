---
name: private-info
description: >-
  Rules for storing and isolating sensitive/private notes and credentials.
  Defines detection patterns, handling procedures, and organization guidelines.
---

# Private Information Management Rules

To ensure sensitive information security, all private or sensitive notes, configuration files, and credentials must be stored in a dedicated `private` directory.

## Directory Structure

The private information directory should be under `knowledge/`, parallel to other knowledge categories:

```
/knowledge/
  ├── private/       # For sensitive info: API keys, passwords, PII, etc.
  ├── planning/
  ├── programming/
  └── biology/
  └── ...
```

## Scope

- Applies to all text and config files under `knowledge/` outside of `private/`
- If sensitive information is detected, execute the "Handling Procedure" immediately

## Detection Keywords and Patterns

### Keywords (case-insensitive)
- `password`, `passwd`, `token`, `secret`
- `api_key`, `client_id`, `client_secret`
- `refresh_token`, `Bearer`, `Authorization:`

### Common Prefixes/Patterns
- `ghp_` / `gho_` (GitHub Token)
- `AKIA` (AWS Access Key)
- `xoxb-` (Slack Bot Token)
- `-----BEGIN PRIVATE KEY-----` / `-----BEGIN.*KEY-----` (various private keys)

### Other Indicators
- URL containing `?token=` or `access_token=` parameters
- Long (>20 char) Base64/hex strings that appear to be credentials

## Handling Procedure

1. Immediately move files containing sensitive info to `knowledge/private/`
2. Update all reference paths to point to new location under `knowledge/private/`
3. If logging migration, add entry to `knowledge/logs/changelog.md` (with timestamp, reason, affected paths)
4. If must reference in public notes, keep only necessary non-sensitive summary info, never include plaintext keys

## Organization and Naming Guidelines

- Filenames: lowercase English with hyphens (kebab-case), e.g., `github-config.md`, `aws-credentials.md`
- Separate credentials/configs for different services into individual files
- Minimum disclosure principle (only store required fields)
- Suggested subdirectories within `private/` by service type (e.g., `private/cloud/`, `private/vcs/`)

## Examples

**Compliant**: Store file containing GitHub Token in `knowledge/private/`, reference it in controlled manner elsewhere
- Example file: `github-config.md` in `knowledge/private/`

**Non-compliant**: Storing files with plaintext keys directly in `knowledge/programming/` or `knowledge/biology/`

## Integration with Other Rules

- Works with knowledge base structure rules to ensure clear categorization and sensitive info isolation
- If conflict with other rules, "sensitive info isolation first" principle takes precedence
