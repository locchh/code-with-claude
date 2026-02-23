# Claude Code Hands-On Guide

[Quick Start](https://code.claude.com/docs/en/quickstart)

[Common Workflows](https://code.claude.com/docs/en/common-workflows)

[Best Practices](https://code.claude.com/docs/en/best-practices)

## 1. [Settings](https://code.claude.com/docs/en/settings) ✅

Claude Code uses a scope system to determine where configurations apply and who they’re shared with.

| Scope | Location | Who it affects | Shared with team? |
|-------|----------|----------------|-------------------|
| Managed | System-level `managed-settings.json` | All users on the machine | Yes (deployed by IT) |
| User | `~/.claude/` directory | You, across all projects | No |
| Project | `.claude/` in repository | All collaborators on this repository | Yes (committed to git) |
| Local | `.claude/*.local.*` files | You, in this repository only | No (gitignored) |

Scopes apply to many Claude Code features:

| Feature | User location | Project location | Local location |
|---------|---------------|------------------|----------------|
| Settings | `~/.claude/settings.json` | `.claude/settings.json` | `.claude/settings.local.json` |
| Subagents | `~/.claude/agents/` | `.claude/agents/` | — |
| Skills | `~/.claude/skills/` | `.claude/skills/` | — |
| MCP servers | `~/.claude.json` | `.mcp.json` | `~/.claude.json` (per-project) |
| Plugins | `~/.claude/settings.json` | `.claude/settings.json` | `.claude/settings.local.json` |
| CLAUDE.md | `~/.claude/CLAUDE.md` | `CLAUDE.md` or `.claude/CLAUDE.md` | `CLAUDE.local.md` |

## 2. [Plan Mode](https://code.claude.com/docs/en/common-workflows#use-plan-mode-for-safe-code-analysis), [Extended Thinking](https://code.claude.com/docs/en/common-workflows#use-extended-thinking-thinking-mode) ✅

- `Shift-Tab` x2 to toggle thinking mode
- Press `Ctrl+G` to view and edit plan 
- `"Think hard"` to force extended thinking
- Press `Cmd+O` to display the thinking

## 3. [Memory](https://code.claude.com/docs/en/memory) ✅

Claude Code has two kinds of memory that persist across sessions:

- [Auto memory](https://code.claude.com/docs/en/memory#auto-memory): Claude automatically saves useful context like project patterns, key commands, and your preferences. This persists across sessions.

- [CLAUDE.md files](https://code.claude.com/docs/en/best-practices#write-an-effective-claude-md): Markdown files you write and maintain with instructions, rules, and preferences for Claude to follow.

Both are loaded into Claude’s context at the start of every session, though auto memory loads only the first 200 lines of its main file.

Claude Code offers several memory locations in a hierarchical structure, each serving a different purpose:

| Memory Type | Location | Purpose | Use Case Examples | Shared With |
|-------------|----------|---------|-------------------|-------------|
| Managed policy | • macOS: /Library/Application Support/ClaudeCode/CLAUDE.md<br>• Linux: /etc/claude-code/CLAUDE.md<br>• Windows: C:\Program Files\ClaudeCode\CLAUDE.md | Organization-wide instructions managed by IT/DevOps | Company coding standards, security policies, compliance requirements | All users in organization |
| Project memory | ./CLAUDE.md or ./.claude/CLAUDE.md | Team-shared instructions for the project | Project architecture, coding standards, common workflows | Team members via source control |
| [Project rules](https://code.claude.com/docs/en/memory#modular-rules-with-claude/rules/) | ./.claude/rules/*.md | Modular, topic-specific project instructions | Language-specific guidelines, testing conventions, API standards | Team members via source control |
| User memory | ~/.claude/CLAUDE.md | Personal preferences for all projects | Code styling preferences, personal tooling shortcuts | Just you (all projects) |
| Project memory (local) | ./CLAUDE.local.md | Personal project-specific preferences | Your sandbox URLs, preferred test data | Just you (current project) |
| Auto memory | ~/.claude/projects/<project>/memory/ | Claude’s automatic notes and learnings | Project patterns, debugging insights, architecture notes | Just you (per project) |

`/init` to create CLAUDE.md file

`/memory` to save context. You can place CLAUDE.md files in several locations:

- User-level: `~/.claude/CLAUDE.md` - applies to all projects

- Project-level: `./CLAUDE.md` - applies only to current project

- Parent directories: Useful for monorepos where both `root/CLAUDE.md` and `root/foo/CLAUDE.md` are pulled in automatically

- Child directories: Claude pulls in child `CLAUDE.md` files on demand when working with files in those directories

## 4. [Skills](https://code.claude.com/docs/en/skills)

Create `SKILL.md` file and skill folder at project scope in `.claude/skills/` or user scope in `~/.claude/skills/` to give Claude domain knowledge and reusable workflows.

## 5. [Subagents](https://code.claude.com/docs/en/sub-agents) ✅

Define specialized assistants at project scope in `.claude/agents/` or user scope in `~/.claude/agents/` that Claude can delegate to for isolated tasks. Use the `/agents` command to generate with Claude or configure manually. You can also create subagents manually using markdown files.

**Tip:** When creating subagents with Claude, you only need to write a clean description of the subagent and configure the tools - Claude will generate the rest. Subagents have access to these tools:

- Read-only tools
- Edit tools  
- Execution tools
- MCP tools
- Other tools

Or all tools.


## 6. [Agent teams](https://code.claude.com/docs/en/agent-teams)

## 7. [MCP](https://code.claude.com/docs/en/mcp) ✅

Usage:

```bash
claude mcp [options] [command]
```

Configure and manage MCP servers

Options: `-h, --help` Display help for command

Commands:

Add an MCP server to Claude Code

```bash
add [options] <name> <commandOrUrl> [args...]
```

Examples:

```bash
# Add HTTP server:
claude mcp add --transport http sentry https://mcp.sentry.dev/mcp

# Add HTTP server with headers:
claude mcp add --transport http corridor https://app.corridor.dev/api/mcp --header "Authorization: Bearer ..."

# Add stdio server with environment variables:
claude mcp add -e API_KEY=xxx my-server -- npx my-mcp-server
```

Other commands:

- `add-from-claude-desktop [options]` - Import MCP servers from Claude Desktop (Mac and WSL only)
- `add-json [options] <name> <json>` - Add an MCP server (stdio or SSE) with a JSON string
- `get <name>` - Get details about an MCP server
- `help [command]` - display help for command
- `list` - List configured MCP servers
- `remove [options] <name>` - Remove an MCP server
- `reset-project-choices` - Reset all approved and rejected project-scoped (.mcp.json) servers within this project
- `serve [options]` - Start the Claude Code MCP server

Here's an example using `add-json`:

```bash
# Add a stdio server (e.g., filesystem MCP):
claude mcp add-json filesystem '{"type":"stdio","command":"npx","args":["-y","@modelcontextprotocol/server-filesystem","/tmp"]}'

# Add an SSE server:
claude mcp add-json my-sse-server '{"type":"sse","url":"https://example.com/sse"}'

# Add with scope (user = all projects):
claude mcp add-json -s user my-server '{"type":"stdio","command":"npx","args":["-y","my-mcp-server"],"env":{"API_KEY":"xxx"}}'
```

The JSON schema for the value:

```json
{
    "type": "stdio",          // or "sse"
    "command": "npx",         // stdio only: executable
    "args": ["-y", "pkg"],    // stdio only: arguments
    "env": { "KEY": "val" },  // optional env vars
    "url": "https://..."      // sse only: endpoint URL
  }
```

`add-json` is useful when you have a full server config to paste in one shot, rather than building it up with flags.

There are three scopes:

| Scope | Flag | Location |
|--------|------|----------|
| local (default) | (none) | ~/.claude.json (per-project) |
| user | -s user | ~/.claude.json (global) |
| project | -s project | .mcp.json (committed to repo) |

For example, if you want context7 available in all your projects, use `-s user`:

```bash
claude mcp add --transport http -s user context7 https://mcp.context7.com/mcp \
    --header "CONTEXT7_API_KEY: ctx7sk-..."
```

Both `local` and `user` are stored in `~/.claude.json` but:                                                                      

| Scope | local (default) | user |
|---|---|---|
| Availability | Current project only | All projects globally |
| Tied to | Current working directory | Your user account |
| File | `~/.claude.json` (with project path key) | `~/.claude.json` (global entry) |
| Usage | "I want this MCP server only when I'm working in this specific project directory." | "I want this MCP server available everywhere, in every project." |

Example of run command on local scope:

```bash
claude mcp add -s user search-papers -- uv --directory /home/locch/Works/mcp-server-papers run mcp_server_papers
```
After add, you can run `claude mcp serve` to start the server and run `claude mcp list` to see the list of servers. In interactive mode use `/mcp`

## 8. [Hooks](https://code.claude.com/docs/en/hooks-guide)

[Hooks](https://code.claude.com/docs/en/hooks) are user-defined shell commands or LLM prompts that execute automatically at specific points in Claude Code’s lifecycle. Run `/hooks` for interactive configuration, or edit `.claude/settings.json` directly

## 9. [Plugins](https://code.claude.com/docs/en/plugins)

Run `/plugin` to browse the marketplace. Plugins add skills, tools, and integrations without configuration.

## 10. [Manage your session](https://code.claude.com/docs/en/best-practices#manage-your-session) ✅

`Esc` - Stop Claude mid-action with the Esc key.

Every action Claude makes creates a checkpoint. You can restore conversation, code, or both to any previous checkpoint. `Esc + Esc` or `/rewind` - Press Esc twice or run `/rewind` to open the rewind menu and restore previous conversation and code state, or summarize from a selected message.

`"Undo that"` - Have Claude revert its changes.

`/clear` - Reset context between unrelated tasks. Long sessions with irrelevant context can reduce performance.

Claude Code automatically compacts conversation history when you approach context limits, which preserves important code and decisions while freeing space.

For more control, run `/compact <instructions>`, like `/compact Focus on the API changes`

Delegate research with "use subagents to investigate X". They explore in a separate context, keeping your main conversation clean for implementation.

Run `claude --resume` to pick up where you left off, or `--resume` to choose from recent sessions.

Run `claude --continue` to resume the most recent conversation.

Monitor your context window usage with `/context`.

Monitor your usage with `/usage`.

Use `/rename` to give sessions descriptive names ("oauth-migration", "debugging-memory-leak") so you can find them later. Treat sessions like branches - different workstreams can have separate, persistent contexts.

## 11. Custom slash commands ✅

For built-in commands like /help and /compact, see [interactive mode](https://code.claude.com/docs/en/interactive-mode#built-in-commands).

Custom slash commands have been merged into skills. A file at `.claude/commands/review.md` and a skill at `.claude/skills/review/SKILL.md` both create `/review` and work the same way. Your existing `.claude/commands/` files keep working. Skills add optional features: a directory for supporting files, frontmatter to [control whether you or Claude invokes them](https://code.claude.com/docs/en/skills#control-who-invokes-a-skill), and the ability for Claude to load them automatically when relevant.

## 12. Worktrees

## 13. Claude code on GitHub ✅

Claude Code supports 3 commands related to GitHub:

- `/install-github-app` Set up Claude GitHub Actions for a repository (pre-requisites: `gh cli` with `workflow` scope)
- `/fix-issue` Fix a GitHub issue (project)
- `/pr-comments` Get comments from a GitHub pull request

After connecting Claude with GitHub, you can run `/install-github-app` to install the Claude GitHub App. Claude will create a workflow file in your repository for each one you select.

- @Claude Code - Tag @claude in issues and PR comments
- Claude Code Review - Automated code review on new PRs

[more details](https://github.com/marketplace/actions/claude-code-action-official)

## 14. Spec-driven

- [Plannotator](https://github.com/backnotprop/plannotator)
- [BMAD](https://github.com/bmad-code-org/BMAD-METHOD)
- [Spec-kit](https://github.com/github/spec-kit)
