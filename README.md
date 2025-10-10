# TaskWarrior MCP Server

Comprehensive Node.js server implementing Model Context Protocol (MCP) for [TaskWarrior](https://taskwarrior.org/) operations. Provides 18 powerful tools for complete task management, reporting, and analytics.

<a href="https://glama.ai/mcp/servers/e8w3e1su1x">
  <img width="380" height="200" src="https://glama.ai/mcp/servers/e8w3e1su1x/badge" alt="TaskWarrior Server MCP server" />
</a>

## Features

- **Complete Task Management**: Create, modify, delete, and track tasks with full TaskWarrior feature support
- **Advanced Filtering**: Project-based, tag-based, priority, and custom filter support
- **Bulk Operations**: Modify multiple tasks simultaneously using TaskWarrior filter syntax
- **Comprehensive Reporting**: Built-in reports, custom reports, and visualization tools
- **Task Analytics**: Count tasks, track completion rates, and generate insights
- **Professional Workflows**: Support for GTD methodology, project management, and team collaboration
- **Secure Shell Integration**: Properly escaped commands prevent injection vulnerabilities

**Requirements**: TaskWarrior (`task` binary) must be installed and configured on your system.

## API

The server provides 18 comprehensive MCP tools organized into the following categories:

### Core Task Management

- **get_next_tasks** - Get a list of all pending tasks
  - Optional filters: `project`, `tags`
- **add_task** - Add a new task with full metadata support
  - Required: `description`
  - Optional: `due`, `priority`, `project`, `tags`
- **mark_task_done** - Mark a task as completed
  - Required: `identifier` (task ID or UUID)
- **get_task_info** - Get detailed information about a specific task
  - Required: `identifier`

### Task Modification

- **modify_task** - Modify an existing task's attributes
  - Required: `identifier`
  - Optional: `description`, `due`, `priority`, `project`, `tags`, `start`, `stop_task`, `wait`, `until`, `scheduled`, `depends`, `clear_fields`
- **modify_tasks_bulk** - Modify multiple tasks using TaskWarrior filter syntax
  - Required: `filter` (TaskWarrior query syntax)
  - Optional: Same modification fields as `modify_task`
- **delete_task** - Delete a task from TaskWarrior
  - Required: `identifier`
- **annotate_task** - Add an annotation to a task
  - Required: `identifier`, `annotation`
- **append_task** - Append text to a task description
  - Required: `identifier`, `text`
- **prepend_task** - Prepend text to a task description
  - Required: `identifier`, `text`
- **duplicate_task** - Duplicate an existing task
  - Required: `identifier`

### System Operations

- **count_tasks** - Count tasks matching specified filters
  - Optional: `status`, `project`, `priority`, `tags`
- **undo_last** - Undo the last TaskWarrior operation
  - No parameters required

### Reporting & Analytics

- **list_tasks_filtered** - List tasks with comprehensive filtering options
  - Optional: `status`, `project`, `priority`, `tags`, `report`
- **builtin_report** - Generate built-in TaskWarrior reports
  - Required: `report` (list, all, active, completed, blocked, overdue, ready, recurring)
  - Optional: `project`, `priority`, `tags`
- **visualization_report** - Generate TaskWarrior visualization reports
  - Required: `report` (burndown, calendar, history, summary, timesheet)
  - Optional: `project`, `tags`
- **custom_report** - Execute custom reports with user-defined columns and filters
  - Required: `report`
  - Optional: `columns`, `filter`

## Usage with Claude Desktop

Add this to your `claude_desktop_config.json`:

```json
{
  "mcpServers": {
    "taskwarrior": {
      "command": "npx",
      "args": [
        "-y",
        "mcp-server-taskwarrior"
      ]
    }
  }
}
```

## Installation

```bash
npm install -g mcp-server-taskwarrior
```

**Prerequisites:**
- TaskWarrior (`task`) must be installed and configured on your system
- Node.js version 16 or higher
- Operating systems: macOS, Linux, Windows (with WSL recommended)

### TaskWarrior Setup
If you don't have TaskWarrior installed:

**macOS:**
```bash
brew install task
```

**Ubuntu/Debian:**
```bash
sudo apt-get install taskwarrior
```

**Other platforms:** Visit [taskwarrior.org](https://taskwarrior.org/download/) for installation instructions.

## Security Features

This MCP server implements robust security measures:
- **Shell Injection Prevention**: All user input is properly escaped using single-quote escaping
- **Command Validation**: Input validation using Zod schemas prevents malformed commands
- **Safe Defaults**: Commands are constructed safely to prevent unintended system access
- **Error Handling**: Comprehensive error handling prevents information leakage

## Advanced Usage Examples

### Basic Task Management
- **Add a high-priority task**: "Add a task to call John with high priority"
  - Executes: `task add priority:H "Call John"`
- **List work tasks**: "What are my current work tasks?"
  - Uses: `get_next_tasks` with `project: "work"`
- **Complete a task**: "Mark task 5 as done"
  - Uses: `mark_task_done` with `identifier: "5"`

### Bulk Operations
- **Update multiple tasks**: "Change all high priority tasks in the Testing project to medium priority"
  - Uses: `modify_tasks_bulk` with `filter: "priority:H and project:Testing"` and `priority: "M"`
- **Add tags to filtered tasks**: "Add the 'urgent' tag to all tasks due this week"
  - Uses: `modify_tasks_bulk` with appropriate date filter and tag addition

### Reporting & Analytics
- **Generate project status**: "Show me a summary of all active tasks by project"
  - Uses: `builtin_report` with `report: "active"`
- **Custom reporting**: "Show me task IDs, projects, and priorities for high priority tasks"
  - Uses: `custom_report` with `columns: ["id", "project", "priority"]` and `filter: "priority:H"`
- **Burndown analysis**: "Generate a burndown chart for the last month"
  - Uses: `visualization_report` with `report: "burndown"`

### Professional Workflows
- **GTD Weekly Review**: Use `builtin_report` to review completed, active, and blocked tasks
- **Sprint Planning**: Use `list_tasks_filtered` to analyze task distribution across projects
- **Team Coordination**: Use `count_tasks` to get metrics on task completion rates
- **Project Management**: Use custom filters to track dependencies and deadlines

### Filter Syntax Examples
The server supports full TaskWarrior filter syntax:
- `project:Work and priority:H` - High priority work tasks
- `due.before:tomorrow` - Tasks due before tomorrow
- `status:completed and end.after:2023-01-01` - Completed tasks since New Year
- `+urgent or priority:H` - Tasks tagged urgent OR high priority
- `project.not:Personal and status:pending` - Non-personal pending tasks

## Credits

Forked from [Brock Wilcox's mcp-server-taskwarrior](https://github.com/awwaiid/mcp-server-taskwarrior).

## License

This MCP server is licensed under the MIT License. See the LICENSE file for details.