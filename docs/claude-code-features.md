# Claude Code Features

Claude code features are organized into several categories, each serving a specific purpose in enhancing your development workflow. Here's a comprehensive overview of these features:

| # | Feature | Priority level | What it does | When to use it | Example | When it loads | What loads | Context cost |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| 1 | [CLAUDE.md](https://code.claude.com/docs/en/memory) | Additive (all levels contribute) | Persistent context loaded every conversation | Project conventions, "always do X" rules | "Use pnpm, not npm. Run tests before committing." | Session start | Full content | Every request |
| 2 | [Skill](https://code.claude.com/docs/en/skills) | managed > user > project | Instructions, knowledge, and workflows Claude can use | Reusable content, reference docs, repeatable tasks | /review runs your code review checklist; API docs skill with endpoint patterns | Session start + when used | Descriptions at start, full content when used | Low (descriptions every request)* |
| 3 | [Subagents](https://code.claude.com/docs/en/subagents) | managed > CLI flag > project > user > plugin | Isolated execution context that returns summarized results | Context isolation, parallel tasks, specialized workers | Research task that reads many files but returns only key findings | When spawned | Fresh context with specified skills | Isolated from main session |
| 4 | [Agent teams](https://code.claude.com/docs/en/agent-teams) | N/A | Coordinate multiple independent Claude Code sessions | Parallel research, new feature development, debugging with competing hypotheses | Spawn reviewers to check security, performance, and tests simultaneously | When launched | Independent context per session | Separate context per teammate |
| 5 | [MCP](https://code.claude.com/docs/en/mcp) | local > project > user | Connect to external services | External data or actions | Query your database, post to Slack, control a browser | Session start | All tool definitions and schemas | Every request |
| 6 | [Hooks](https://code.claude.com/docs/en/hooks-guide) | Merge (all hooks fire) | Deterministic script that runs on events | Predictable automation, no LLM involved | Run ESLint after every file edit | On trigger | Nothing (runs externally) | Zero, unless hook returns additional context |
| 7 | [Plugins](https://code.claude.com/docs/en/plugins) | Namespaced (avoids conflicts) | Bundles skills, hooks, subagents, and MCP servers into a single installable unit | Reuse setup across repositories or distribute to others via marketplace | /my-plugin:review uses namespaced skill from plugin | Session start | All bundled components (skills, hooks, subagents, MCP servers) | Depends on bundled components |

[More details](https://code.claude.com/docs/en/features-overview)

Claude Code has access to a set of powerful tools that help it understand and modify your codebase:

| Tool | Description | Permission Required |
| --- | --- | --- |
| AskUserQuestion | Asks multiple-choice questions to gather requirements or clarify ambiguity | No |
| Bash | Executes shell commands in your environment (see Bash tool behavior below) | Yes |
| TaskOutput | Retrieves output from a background task (bash shell or subagent) | No |
| Edit | Makes targeted edits to specific files | Yes |
| ExitPlanMode | Prompts the user to exit plan mode and start coding | Yes |
| Glob | Finds files based on pattern matching | No |
| Grep | Searches for patterns in file contents | No |
| KillShell | Kills a running background bash shell by its ID | No |
| MCPSearch | Searches for and loads MCP tools when tool search is enabled | No |
| NotebookEdit | Modifies Jupyter notebook cells | Yes |
| Read | Reads the contents of files | No |
| Skill | Executes a skill within the main conversation | Yes |
| Task | Runs a sub-agent to handle complex, multi-step tasks | No |
| TaskCreate | Creates a new task in the task list | No |
| TaskGet | Retrieves full details for a specific task | No |
| TaskList | Lists all tasks with their current status | No |
| TaskUpdate | Updates task status, dependencies, details, or deletes tasks | No |
| WebFetch | Fetches content from a specified URL | Yes |
| WebSearch | Performs web searches with domain filtering | Yes |
| Write | Creates or overwrites files | Yes |
| LSP | Code intelligence via language servers. Reports type errors and warnings automatically after file edits. Also supports navigation operations: jump to definitions, find references, get type info, list symbols, find implementations, trace call hierarchies. Requires a code intelligence plugin and its language server binary | No |

[More details](https://code.claude.com/docs/en/settings#tools-available-to-claude)

## Compare similar features

Some features can seem similar. Here's how to tell them apart.

### Skill vs Subagent

Skills and subagents solve different problems:

- **Skills** are reusable content you can load into any context
- **Subagents** are isolated workers that run separately from your main conversation

| Aspect | Skill | Subagent |
| --- | --- | --- |
| What it is | Reusable instructions, knowledge, or workflows | Isolated worker with its own context |
| Key benefit | Share content across contexts | Context isolation. Work happens separately, only summary returns |
| Best for | Reference material, invocable workflows | Tasks that read many files, parallel work, specialized workers |

**Skills can be reference or action.** Reference skills provide knowledge Claude uses throughout your session (like your API style guide). Action skills tell Claude to do something specific (like `/deploy` that runs your deployment workflow).

**Use a subagent** when you need context isolation or when your context window is getting full. The subagent might read dozens of files or run extensive searches, but your main conversation only receives a summary. Custom subagents can have their own instructions and can preload skills.

**They can combine.** A subagent can preload specific skills (`skills:` field). A skill can run in isolated context using `context: fork`.

### CLAUDE.md vs Skill

Both store instructions, but they load differently and serve different purposes.

| Aspect | CLAUDE.md | Skill |
| --- | --- | --- |
| Loads | Every session, automatically | On demand |
| Can include files | Yes, with `@path` imports | Yes, with `@path` imports |
| Can trigger workflows | No | Yes, with `/<name>` |
| Best for | "Always do X" rules | Reference material, invocable workflows |

**Put it in CLAUDE.md** if Claude should always know it: coding conventions, build commands, project structure, "never do X" rules.

**Put it in a skill** if it's reference material Claude needs sometimes (API docs, style guides) or a workflow you trigger with `/<name>` (deploy, review, release).

**Rule of thumb:** Keep CLAUDE.md under ~500 lines. If it's growing, move reference content to skills.

### Subagent vs Agent team

Both parallelize work, but they're architecturally different:

- **Subagents** run inside your session and report results back to your main context
- **Agent teams** are independent Claude Code sessions that communicate with each other

| Aspect | Subagent | Agent team |
| --- | --- | --- |
| Context | Own context window; results return to the caller | Own context window; fully independent |
| Communication | Reports results back to the main agent only | Teammates message each other directly |
| Coordination | Main agent manages all work | Shared task list with self-coordination |
| Best for | Focused tasks where only the result matters | Complex work requiring discussion and collaboration |
| Token cost | Lower: results summarized back to main context | Higher: each teammate is a separate Claude instance |

**Use a subagent** when you need a quick, focused worker: research a question, verify a claim, review a file. The subagent does the work and returns a summary. Your main conversation stays clean.

**Use an agent team** when teammates need to share findings, challenge each other, and coordinate independently. Agent teams are best for research with competing hypotheses, parallel code review, and new feature development where each teammate owns a separate piece.

**Transition point:** If you're running parallel subagents but hitting context limits, or if your subagents need to communicate with each other, agent teams are the natural next step.

> Note: Agent teams are experimental and disabled by default.

### MCP vs Skill

MCP connects Claude to external services. Skills extend what Claude knows, including how to use those services effectively.

| Aspect | MCP | Skill |
| --- | --- | --- |
| What it is | Protocol for connecting to external services | Knowledge, workflows, and reference material |
| Provides | Tools and data access | Knowledge, workflows, reference material |
| Examples | Slack integration, database queries, browser control | Code review checklist, deploy workflow, API style guide |

These solve different problems and work well together:

**MCP** gives Claude the ability to interact with external systems. Without MCP, Claude can't query your database or post to Slack.

**Skills** give Claude knowledge about how to use those tools effectively, plus workflows you can trigger with `/<name>`. A skill might include your team's database schema and query patterns, or a `/post-to-slack` workflow with your team's message formatting rules.

**Example:** An MCP server connects Claude to your database. A skill teaches Claude your data model, common query patterns, and which tables to use for different tasks.

## Combine features

Each extension solves a different problem: CLAUDE.md handles always-on context, skills handle on-demand knowledge and workflows, MCP handles external connections, subagents handle isolation, and hooks handle automation. Real setups combine them based on your workflow.

For example, you might use CLAUDE.md for project conventions, a skill for your deployment workflow, MCP to connect to your database, and a hook to run linting after every edit. Each feature handles what it's best at.

| Pattern | How it works | Example |
| --- | --- | --- |
| Skill + MCP | MCP provides the connection; a skill teaches Claude how to use it well | MCP connects to your database, a skill documents your schema and query patterns |
| Skill + Subagent | A skill spawns subagents for parallel work | `/review` skill kicks off security, performance, and style subagents that work in isolated context |
| CLAUDE.md + Skills | CLAUDE.md holds always-on rules; skills hold reference material loaded on demand | CLAUDE.md says "follow our API conventions," a skill contains the full API style guide |
| Hook + MCP | A hook triggers external actions through MCP | Post-edit hook sends a Slack notification when Claude modifies critical files |

## Shortcuts

| Mode | Shortcut | Action |
|------|----------|--------|
| **Input modes** | `!` | Switch to bash mode |
| | `/` | Switch to commands mode |
| | `@` | Switch to file paths mode |
| | `&` | Switch to background mode |
| **Navigation** | `Shift + Tab` (double tap) | Auto-accept edits |
| | `Ctrl + O` | Toggle verbose output |
| | `Ctrl + T` | Toggle tasks view |
| | `Ctrl + G` | Edit in $EDITOR |
| | `Shift + Enter` | Insert newline |
| | `Meta + P` | Switch model |
| **Control** | `Double tap Esc` | Clear input |
| | `Ctrl + Shift + -` | Undo |
| | `Ctrl + Z` | Suspend |
| | `Ctrl + S` | Stash prompt |
| **Customization** | `/keybindings` | Customize keybindings |