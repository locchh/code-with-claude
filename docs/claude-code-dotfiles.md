
# Claude Code Dotfiles

This document explains what can live inside a `.claude/` folder and how to use it.

## What can live inside a `.claude/` folder:

**`CLAUDE.md`** — The main project instructions file. Loaded at startup, it gives Claude persistent context about your codebase, conventions, and workflows.

**`rules/`** — A directory of `.md` files that are automatically loaded as project memory. You can organize them by topic (e.g., `code-style.md`, `testing.md`, `security.md`) and even scope rules to specific file paths using YAML frontmatter with a `paths` field. Subdirectories are supported too.

**`settings.json`** — Structured configuration for permissions (allow/ask/deny rules for tools), hooks, environment variables, MCP server config, enabled plugins, and other behavioral settings.

**`settings.local.json`** — Personal project settings that are gitignored. Good for API keys or experimental config you don't want to commit.

**`commands/`** — Custom slash commands (`.md` files) that you invoke with `/<command-name>`. These are user-invoked prompt shortcuts.

**`skills/`** — Model-invoked capabilities. Each skill is a subfolder containing a `SKILL.md` (with YAML frontmatter for name/description) plus optional scripts and resources. Unlike commands, Claude automatically discovers and uses skills based on context rather than you explicitly calling them.

**`agents/`** (subagents) — Specialized Claude instances with their own context windows and personas, defined via markdown files with frontmatter (model, color, description). Used for domain-specific tasks like code review or debugging.

So the full picture looks something like:

```
.claude/
├── CLAUDE.md
├── settings.json
├── settings.local.json
├── rules/
│   ├── code-style.md
│   └── security.md
├── commands/
│   └── review.md
├── skills/
│   └── testing-patterns/
│       ├── SKILL.md
│       └── helpers.py
└── agents/
    └── reviewer.md
```