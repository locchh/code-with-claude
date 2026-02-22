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

- **CLAUDE.md files**: Markdown files you write and maintain with instructions, rules, and preferences for Claude to follow.

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

### CLAUDE.md

`/init` to create CLAUDE.md file

`/memory` to save context. You can place CLAUDE.md files in several locations:

- User-level: `~/.claude/CLAUDE.md` - applies to all projects

- Project-level: `./CLAUDE.md` - applies only to current project

- Parent directories: Useful for monorepos where both `root/CLAUDE.md` and `root/foo/CLAUDE.md` are pulled in automatically

- Child directories: Claude pulls in child `CLAUDE.md` files on demand when working with files in those directories

## 4. [Skills](https://code.claude.com/docs/en/skills)

Create SKILL.md files in `.claude/skills/` to give Claude domain knowledge and reusable workflows.

## 5. [Subagents](https://code.claude.com/docs/en/sub-agents)

Define specialized assistants in `.claude/agents/` that Claude can delegate to for isolated tasks.

## 6. [Agent teams](https://code.claude.com/docs/en/agent-teams)

## 7. [MCP](https://code.claude.com/docs/en/mcp)

Run `claude mcp add` to connect external tools like Notion, Figma, or your database.

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

## 13. Spec-driven

- [Plannotator](https://github.com/backnotprop/plannotator)
- [BMAD](https://github.com/bmad-code-org/BMAD-METHOD)
- [Spec-kit](https://github.com/github/spec-kit)