# Claude Code Hands-On Guide

[Quick Start](https://code.claude.com/docs/en/quickstart)

[Common Workflows](https://code.claude.com/docs/en/common-workflows)

[Best Practices](https://code.claude.com/docs/en/best-practices)

## Table of Contents

- [1. Settings](#1-settings) ✅
- [2. Plan Mode, Extended Thinking](#2-plan-mode-extended-thinking) ✅
- [3. Memory](#3-memory) ✅
- [4. Skills](#4-skills) ✅
- [5. Subagents](#5-subagents) ✅
- [6. Agent teams](#6-agent-teams) ✅
- [7. MCP](#7-mcp) ✅
- [8. Hooks](#8-hooks)
- [9. Plugins](#9-plugins)
- [10. Manage your session](#10-manage-your-session) ✅
- [11. Custom slash commands](#11-custom-slash-commands) ✅
- [12. Worktrees](#12-worktrees) ✅
- [13. Claude code on GitHub](#13-claude-code-on-github) ✅
- [14. Spec-driven](#14-spec-driven) 🔥

## 1. [Settings](https://code.claude.com/docs/en/settings)

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

## 2. [Plan Mode](https://code.claude.com/docs/en/common-workflows#use-plan-mode-for-safe-code-analysis), [Extended Thinking](https://code.claude.com/docs/en/common-workflows#use-extended-thinking-thinking-mode)

- `Shift-Tab` x2 to toggle thinking mode
- Press `Ctrl+G` to view and edit plan 
- `"Think hard"` to force extended thinking
- Press `Cmd+O` to display the thinking

## 3. [Memory](https://code.claude.com/docs/en/memory)

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

**Tip**: You should pair-working with Claude for a task first, then after the end of session, asking claude to create a skill for the task.

## 5. [Subagents](https://code.claude.com/docs/en/sub-agents)

Define specialized assistants at project scope in `.claude/agents/` or user scope in `~/.claude/agents/` that Claude can delegate to for isolated tasks. Use the `/agents` command to generate with Claude or configure manually. You can also create subagents manually using markdown files.

**Tip:** When creating subagents with Claude, you only need to write a clean description of the subagent and configure the tools - Claude will generate the rest. Subagents have access to these tools:

- Read-only tools
- Edit tools  
- Execution tools
- MCP tools
- Other tools

Or all tools.

## 6. [Agent teams](https://code.claude.com/docs/en/agent-teams)

Agent teams let you coordinate multiple Claude Code instances working together. One session acts as the team lead, coordinating work, assigning tasks, and synthesizing results. Teammates work independently, each in its own context window, and communicate directly with each other. Unlike subagents, which run within a single session and can only report back to the main agent, you can also interact with individual teammates directly without going through the lead.

**When to use agent teams:**
- **Research and review:** Multiple teammates investigate different aspects simultaneously
- **New modules/features:** Each teammate owns separate pieces without conflicts  
- **Debugging with competing hypotheses:** Test different theories in parallel
- **Cross-layer coordination:** Frontend, backend, and tests owned by different teammates

| Feature | Subagents | Agent teams |
|---------|-----------|-------------|
| **Context** | Own context window; results return to the caller | Own context window; fully independent |
| **Communication** | Report results back to the main agent only | Teammates message each other directly |
| **Coordination** | Main agent manages all work | Shared task list with self-coordination |
| **Best for** | Focused tasks where only the result matters | Complex work requiring discussion and collaboration |
| **Token cost** | Lower: results summarized back to main context | Higher: each teammate is a separate Claude instance |

An agent team consists of:

| Component | Role |
|-----------|------|
| **Team lead** | The main Claude Code session that creates the team, spawns teammates, and coordinates work. Team config `~/.claude/teams/{team-name}/config.json` |
| **Teammates** | Separate Claude Code instances that each work on assigned tasks |
| **Task list** | Shared list of work items that teammates claim and complete. Task list shared at `~/.claude/tasks/{team-name}/` |
| **Mailbox** | Messaging system for communication between agents |

You can choice display mode:
- **In-process**: All teammates run in the same session
- **Split panes**: Each teammate runs in its own pane

**Step 1: Enable agent teams and split panes**

Run this one to enable agent teams and split panes:

```bash
cat > ~/.claude/settings.json <<EOF
{
  "env": {
    "CLAUDE_CODE_EXPERIMENTAL_AGENT_TEAMS": "1"
  },
  "preference": {
    "tmuxSplitPanes": true
  }
}
EOF
```

Split-pane mode requires either [tmux](https://github.com/tmux/tmux/wiki) or iTerm2 with the [it2](https://github.com/mkusaka/it2) CLI. To install manually:

```bash
# Install tmux
sudo apt install -y tmux
tmux --help

# Install it2 (for iTerm2 users)
# Note: it2 is specifically for iTerm2 on macOS, not available on Linux
# For Linux users, tmux is sufficient
```

**Step 2: Start a tmux session**

```bash
tmux new-session -s <session_name>
```

**Step 3: Launch claude code**

```bash
claude # Or claude --dangerously-skip-permissions
# Teammates start with the lead’s permission settings
# If the lead runs with --dangerously-skip-permissions, all teammates do too.
# After spawning, you can change individual teammate modes, but you can’t set per-teammate modes at spawn time.
```

**Step 4: Create a team and spawn teammates**

After enabling agent teams, tell Claude to create an agent team and describe the task and the team structure you want in natural language. Claude creates the team, spawns teammates, and coordinates work based on your prompt. This example works well because the three roles are independent and can explore the problem without waiting on each other:

```
I'm designing a CLI tool that helps developers track TODO comments across
their codebase. Create an agent team to explore this from different angles: one
teammate on UX, one on technical architecture, one playing devil's advocate. Use model Haiku for each teammate.
```

You can specific model for teammate by:

```
Use model Haiku for each teammate
```

From there, Claude creates a team with a [shared task list](https://code.claude.com/docs/en/interactive-mode#task-list), spawns teammates for each perspective, has them explore the problem, synthesizes findings, and attempts to [clean up the team](https://code.claude.com/docs/en/agent-teams#clean-up-the-team) when finished. Each teammate has its own context window. When spawned, a teammate loads the same project context as a regular session: CLAUDE.md, MCP servers, and skills. It also receives the spawn prompt from the lead. The lead’s conversation history does not carry over. Teammate messaging:

- **message**: send a message to one specific teammate
- **broadcast**: send to all teammates simultaneously. Use sparingly, as costs scale with team size.

The lead can assign tasks explicitly, or teammates can self-claim:

- **Lead assigns**: tell the lead which task to give to which teammate
- **Self-claim**: after finishing a task, a teammate picks up the next unassigned, unblocked task on its own


**Step 5: Clean up**

When you done, exit claude code and kill tmux session. To gracefully end a teammate’s session:

```
Ask the researcher teammate to shut down
```

To clean up a team:

```
Clean up the team
```

To kill tmux session

```bash
tmux ls
tmux kill-session -t <session-name>
```

Explore related approaches for parallel work and delegation:

- **Lightweight delegation**: subagents spawn helper agents for research or verification within your session, better for tasks that don’t need inter-agent coordination

- **Manual parallel sessions**: Git worktrees let you run multiple Claude Code sessions yourself without automated team coordination


## 7. [MCP](https://code.claude.com/docs/en/mcp)

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

## 10. [Manage your session](https://code.claude.com/docs/en/best-practices#manage-your-session)

`Esc` - Stop Claude mid-action with the Esc key.

Every action Claude makes creates a checkpoint. You can restore conversation, code, or both to any previous checkpoint. `Esc + Esc` or `/rewind` - Press Esc twice or run `/rewind` to open the rewind menu and restore previous conversation and code state, or summarize from a selected message.

`"Undo that"` - Have Claude revert its changes.

`/clear` - Reset context between unrelated tasks. Long sessions with irrelevant context can reduce performance.

Claude Code automatically compacts conversation history when you approach context limits, which preserves important code and decisions while freeing space.

For more control, run `/compact <instructions>`, like `/compact Focus on the API changes`

Delegate research with "use subagents to investigate X". They explore in a separate context, keeping your main conversation clean for implementation.

Run `claude --resume` to pick up where you left off, or `--resume` to choose from recent sessions.

Run `claude --continue` to resume the most recent conversation.

Monitor your usage with `/usage`.

Use `/rename` to give sessions descriptive names ("oauth-migration", "debugging-memory-leak") so you can find them later. Treat sessions like branches - different workstreams can have separate, persistent contexts.

Monitor your context window usage with `/context`.

Install the Claude Code extension/plugin in your IDE first, then use `/ide` in an external terminal to connect Claude Code to your running IDE. The plugin monitors your editor state and automatically shares:
- Currently open file path, selected text, and cursor position
- IDE diagnostics (lint errors, type errors, etc.)

`/ide` is only needed when running Claude Code in an **external terminal** (outside the IDE) — it connects the terminal session to your running IDE. If you're using Claude Code in the IDE's built-in terminal, context is shared automatically without `/ide`.

Once connected, Claude uses MCP IDE tools (like `getDiagnostics`) to query the IDE for diagnostics and other info.

To reference specific lines from multiple files, use `@file#line-range` syntax in your prompt:

```
@src/auth.js#10-20 and @src/models/User.ts#5-15
```

You can combine multiple file references in a single prompt.

## 11. Custom slash commands

For built-in commands like /help and /compact, see [interactive mode](https://code.claude.com/docs/en/interactive-mode#built-in-commands).

Custom slash commands have been merged into skills. A file at `.claude/commands/review.md` and a skill at `.claude/skills/review/SKILL.md` both create `/review` and work the same way. Your existing `.claude/commands/` files keep working. Skills add optional features: a directory for supporting files, frontmatter to [control whether you or Claude invokes them](https://code.claude.com/docs/en/skills#control-who-invokes-a-skill), and the ability for Claude to load them automatically when relevant.

## 12. [Worktrees](https://code.claude.com/docs/en/common-workflows#run-parallel-claude-code-sessions-with-git-worktrees)

### Basic

Git worktree is a feature of Git that lets you check out multiple branches of the same repository at the same time — in different folders.

Traditional `git checkout`:
```bash
git checkout -b feature-x
# work
git checkout main
git merge feature-x
```

Use `git worktree`:
```bash
git worktree add -b feature-x ../feature-x
# work in ../feature-x
git merge feature-x   # from main worktree
```

Comparation

| Topic             | `git checkout` | `git worktree`       |
| ----------------- | -------------- | -------------------- |
| HEADs             | One            | Multiple             |
| Parallel branches | ❌              | ✅                  |
| Stash needed      | Often          | Rare                 |
| Context switching | High           | Low                  |
| Disk usage        | Low            | Low (shared objects) |
| Mental load       | Higher         | Lower                |


Create a new worktree. Use `git worktree add <folder> <branch>`:
```bash
# Create a new folder ../main-fix
# Checkout branch main inside it
git worktree add ../main-fix main
```

If branch doesn't exist. Use `git worktree add -b <branch_name> <folder>`:
```bash
# Creates branch from current HEAD
# Create folder ../feature-payment
# Checkout that new branch inside it
git worktree add -b feature-payment ../feature-payment
```

Create commit:

```bash
cd ../feature-payment

# Edit files
git add .
git commit -m "implement feature payment"

# When Finished
cd ../myapp
git merge feature-payment
```

Merge the Branch:
```bash
git merge feature-login
```

List worktrees:
```bash
git worktree list
```

Remove worktree:
```bash
git worktree remove ../main-fix
```

There are multiplace to store worktrees:
- Windsurf keep it in `~/.windsurf/worktrees/<repo_name>`
- Git keep it in `~/.git/worktrees/<repo_name>`
- Claude code keep it in `~/.claude/worktrees/<repo_name>`

### Run parallel Claude Code sessions with Git worktrees

When working on multiple tasks at once, you need each Claude session to have its own copy of the codebase so changes don’t collide. Git worktrees solve this by creating separate working directories that each have their own files and branch, while sharing the same repository history and remote connections. This means you can have Claude working on a feature in one worktree while fixing a bug in another, without either session interfering with the other.

```bash
# Start Claude in a worktree named "feature-auth"
# Creates .claude/worktrees/feature-auth/ with a new branch
claude --worktree feature-auth

# Start another session in a separate worktree
claude --worktree bugfix-123

# If you omit the name, Claude generates a random one automatically:
claude --worktree
```

When you run a command like  `claude --worktree feature-auth` it similar to `git worktree add -b feature-auth ../feature-auth main` or if the branch already exists `git worktree add ../feature-auth feature-auth`. Worktrees are created at `<repo>/.claude/worktrees/<name>` and branch from the default remote branch. The worktree branch is named `worktree-<name>`.

Subagents can also use worktree isolation to work in parallel without conflicts. Ask Claude like this:

```
Spawn 5 agents in parallel. Each in it own isolated worktree, to handle these features
```

Worktree isolation works with git by default. For other version control systems like SVN, Perforce, or Mercurial, configure [WorktreeCreate and WorktreeRemove hooks](https://code.claude.com/docs/en/hooks#worktreecreate) to provide custom worktree creation and cleanup logic. When configured, these hooks replace the default git behavior when you use `--worktree`. For automated coordination of parallel sessions with shared tasks and messaging, see agent teams.

Before claude code support this feature we must:

```bash
git worktree add -b feature-x ../feature-x
cd ../feature-x
claude
# Implement feature x
cd ../myapp
git merge feature-x
```

## 13. Claude code on GitHub

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
