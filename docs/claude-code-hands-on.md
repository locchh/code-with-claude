# Claude Code Hands-On Guide

[Quick Start](https://code.claude.com/docs/en/quickstart)

[Common Workflows](https://code.claude.com/docs/en/common-workflows)

[Best Practices](https://code.claude.com/docs/en/best-practices)


## 1. [Settings](https://code.claude.com/docs/en/settings) ✅

## 2. [Plan Mode](https://code.claude.com/docs/en/common-workflows#use-plan-mode-for-safe-code-analysis), Extended Thinking ✅

- `Shift-Tab` x2 to toggle thinking mode
- Press `Ctrl+G` to view and edit plan 
- `"Think hard"` to force extended thinking
- Press `Cmd+O` to display the thinking

## 3. CLAUDE.md ✅

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

## 10. Manage your session ✅

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

## 11. Custom slash commands

## 12. Worktrees, tmux

## 13. Spec-driven

- [Plannotator](https://github.com/backnotprop/plannotator)
- [BMAD](https://github.com/bmad-code-org/BMAD-METHOD)
- [Spec-kit](https://github.com/github/spec-kit)