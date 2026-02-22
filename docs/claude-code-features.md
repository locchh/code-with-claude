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