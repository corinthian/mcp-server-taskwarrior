# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

MCP server for TaskWarrior providing 17 tools for task management, reporting, and analytics. Executes TaskWarrior CLI commands via Node.js `execSync`.

## Development Commands

```bash
npm run build    # Compile TypeScript to dist/ and chmod +x
npm run watch    # Auto-rebuild on changes
npm link         # Create global symlink for local testing with Claude Desktop
```

## Architecture

**Single-file implementation**: All code in `index.ts` (~750 lines)

**Key functions:**
- `escapeShellArg()` (line 21) - Shell injection prevention via single-quote escaping
- `parseFlexibleDate()` (line 27) - Validates ISO timestamps and TaskWarrior date shorthands
- `buildTaskModifyArgs()` (line 59) - Constructs modify commands with proper escaping

**CLI execution pattern:**
```typescript
execSync(`task ${args.join(" ")}`, { maxBuffer: 1024 * 1024 * 10 })
```

**Bulk operations** use `yes |` piped to bash for confirmation bypass (line 495).

## Tool Categories (17 total)

| Category | Tools |
|----------|-------|
| Core | get_next_tasks, add_task, mark_task_done |
| Modification | modify_task, modify_tasks_bulk, delete_task, annotate_task, append_task, prepend_task, duplicate_task |
| Query | get_task_info, count_tasks, list_tasks_filtered |
| Reports | builtin_report, visualization_report, custom_report |
| System | undo_last |

## Schema Constraints

- **Project names**: `/^[a-zA-Z0-9._-]+$/`
- **Tags**: `/^[@a-z0-9_-]+$/` (lowercase)
- **Priority**: "H", "M", "L" only
- **Dates**: ISO timestamps, relative (+7d, -2w), special (today, eom), day names, ordinals

## Security

- User text (descriptions, annotations) MUST be escaped with `escapeShellArg()`
- TaskWarrior keywords (priority, status, filters) are NOT escaped
- Zod schemas validate all input before command execution

## Testing

No automated tests. Manual testing requires:
1. TaskWarrior installed and configured
2. `npm run build` or `npm run watch`
3. Test via Claude Desktop with `npm link`

Debug output goes to stderr (stdout reserved for MCP protocol).

## Configuration

Requires TaskWarrior (`task` binary) installed. Server returns raw CLI output with 10MB maxBuffer.

## Distribution

Published as npm package. Binary entry point: `dist/index.js`. Only `dist/` is included in package.
