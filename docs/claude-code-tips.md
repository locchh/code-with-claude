
# Claude Code Tips/

## ACT 1: Foundations (Tips 1-25)

**Tip 1: Run from root directory** When you first launch Claude Code, run it from your project root directory. Claude works best when it can see your whole project structure.

**Tip 2: Run /init immediately** Creates CLAUDE.md file. Think of it as Claude's long-term memory. `/init` is a starting point. Review it, refine it, make it yours.

**Tip 3: CLAUDE.md is hierarchical** Global at `~/.claude/CLAUDE.md`. Project-level at `./CLAUDE.md`. Subdirectory for module rules. Most specific wins.

**Tip 4: Keep CLAUDE.md concise** It gets prepended to every prompt. A bloated file eats your context window and degrades performance. Focus on what matters.

**Tip 5: Structure: What, Domain, Validation** What = tech stack. Domain context = what each part does. Validation steps = tests, linters, type checks, etc.

### Keyboard Shortcuts

**Tip 6: Shift+Tab toggles modes** Cycles between Normal, Auto-accept and Plan mode Memorize this. You'll use it constantly.

**Tip 7: Escape interrupts** If you see the thinking block going off-rails, stop it early with Escape. Don't wait for Claude to finish making a mistake.

**Tip 8: Double Escape clears input** Double tap Escape to clear your current input. Vim users can use vim keybindings to edit prompts.

**Tip 9: Double Escape on empty = rewind** Rewind code-only, conversation-only, or both. Your undo button for Claude's mistakes.

**Tip 10: Screenshot and drag** On Mac `Cmd+Shift+4` to capture a screenshot, then drag and drop the image into Claude. Claude sees them.

**Tip 11: Add context to screenshots** "This button is broken on Safari" beats just dropping an image. tell Claude what you're looking at.

### Essential Commands

**Tip 12: /clear resets context** Use when you've switched directions and old  context would confuse the new work.

**Tip 13: /context shows token usage** Audit your context. Is it clean? Relevant? Or full of dead-end explorations?

**Tip 14: Let auto-compaction work** Don't manually run `/compact`. Create context documents Claude can reference on demand. Lazy loading beats preloading.

**Tip 15: /model switches models** Cost-conscious? Use `/model` to switch to a cheaper model. Not? Just use the best model for everything.

**Tip 16: /resume recovers sessions** Work saved for 30 days. Your escape hatch for unrecoverable errors or accidental quits.

**Tip 17: /mcp shows MCP status** MCP eat tokens. Every tool adds weight. Be selective. Project-scope over global.

**Tip 18: /help shows all commands** Forgot a command? `/help` lists everything. Commands, shortuts. flags, etc. Your quick reference.

**Tip 19: Git is your safety net** Commit often, think about your commits before coding. Make it atomic and narrative. Before big changes. Before risky changes. Revert when needed. There are other command to revert your code changes by `/rewind`.

### CLAUDE.md Deep Dive

**Tip 20: Add a Critical Rules section** "Never do X" or "Always do Y". Document mistakes here. Add to it whenever Claude makes a mistake you don't want repeated.

**Tip 21: Ask Claude to update rules** Say "add this to my rules" and Claude updates the file. Your CLAUDE.md becomes how you actually work.

**Tip 22: Use workflow triggers** "When user says deploy, run deploy script". This turns Claude into a workflow engine. 

**Tip 23: Commit CLAUDE.md to git** Team gets instant access. They contribute back. Shared knowledge.

**Tip 24: dangerously-skip for throwaway envs** Docker container, Unpushed branch. If you can blow it away, let Claude run free by using `claude --dangerously-skip-permissions`.

**Tip 25: Combine skip with allowlists** Event with `--dangerously-skip-permissions` use `/permissions` to define what's allowed.

## ACT 2: Daily Workflow (Tips 26-32)

**Tip 26: Start features in Plan Mode** `Shift+Tab` twice. Claude can read but can't execute. Explores without side risk.

**Tip 27: Fresh context beats bloated** Less is more. When context is full of dead ends, start w a new session.

**Tip 28: Persist before ending sessions** Sessions are temporary. Persist before ending sessions. CLAUDE.md is permanent. This is long-term memory. But there are other ways to persist your work, like TODO.md, code commits, etc.

**Tip 29: Lazy load context** Index at root pointing to subdirectory docs. Claude loads detail only when needed.

**Tip 30: Give verification commands** Test, lint, type check commands in CLAUDE.md. This 2-3x's output quality. 

**Tip 31: Consider Opus for complex work** Steer it less. better at tool use. Almost always faster for multi-step tasks.

**Tip 32: Read thinking blocks** "I assume..." or "i'm not sure..." = course-correct before Claude commits to wrong path.

## ACT 3: Power User (Tips 33-40)

**Tip 33: Four composability primitives** Skills, Slash commands, MCPs, Subagents. How they work together separates casuals from power users.

**Tip 34: Skills = recurring workflows** `.claude/skills` directory. Templates that load context when triggered. Lazy-loaded. Don't eat tokens until needed.

**Tip 35: Commands = quick shorthand** `.claude/commands` + git. Power users run `/commit-push-pr` dozens of times a daily.

**Tip 36: Never create commands manually** "Create a command that does X." let Claude manage the file structure.

**Tip 37: MCPs = external service docs** Not just API connections. Structured documentation for databases. browsers, systems.

**Tip 38: Ask Claude to install MCPs**

**Tip 39: Subagents = isolated context** `Task()` spawns clones for parallel work. Each get fresh context window.

**Tip 40: Avoid instruction overload** Context is best served fresh and condensed. Quality over quantity.

## ACT 4: Advanced (Tips 41-50)

**Tip 41: Run multiple instances** Different termnals, different tasks. No shared context = feature, not bug.

**Tip 42: iTerm split panes** `Cmd+D` split. `Cmd+[/]` navigate. `Cmd+1-9` jump. Optimal for parallel work.

**Tip 43: Enable notifications** Custom sound when Claude finishes. Context-switch while claude works.

**Tip 44: Git worktrees for isolation** Multiple Claude instances, same repo, isolated files. Each worktree = separate checkout. 

**Tip 45: /chrome connects browser** See and interact with web pages. Authenticated session. No API keys.

**Tip 46: Powerful for debugging**  Navigate click, fill forms, read console, "Fix the error console". One flow.

**Tip 47: Hooks & Automation** PreToolUse = before. PostToolUse = after. 

**Tip 48: Hooks intercept actions** Format after edits. Catches CI edge cases. Never commit ugly code.

**Tip 49: Auto-format with PostToolUse** PreToolUse blocks before execution. Guardrails for destructive ops. 

**Tip 50: Block dangerous commands**  Pre-built skills, commands, hooks install and run immediately. 