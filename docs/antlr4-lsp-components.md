# ANTLR4 & LSP Components Reference

A consolidated reference for ANTLR4 parser pipeline components, LSP server frameworks, and language tooling features.

---

## Claude Code Intelligence Plugins

These plugins (from Claude Code's official marketplace) connect Claude to a **Language Server Protocol (LSP)** server, giving Claude two key capabilities:

1. **Automatic diagnostics** — after every file edit, the language server reports errors/warnings back to Claude so it can self-correct in the same turn
2. **Code navigation** — jump to definitions, find references, get type info, list symbols, find implementations, trace call hierarchies

| Language | Plugin | Required binary |
|---|---|---|
| Python | `pyright-lsp` | `pyright-langserver` |
| TypeScript | `typescript-lsp` | `typescript-language-server` |
| Rust | `rust-analyzer-lsp` | `rust-analyzer` |
| Go | `gopls-lsp` | `gopls` |
| Java | `jdtls-lsp` | `jdtls` |
| C/C++ | `clangd-lsp` | `clangd` |
| C# | `csharp-lsp` | `csharp-ls` |

Install a plugin:
```shell
/plugin install pyright-lsp@claude-plugins-official
```

You can also create a custom LSP plugin for any language not listed above.

---

## LSP Server Frameworks by Language

| Language | Framework |
|---|---|
| Python | `pygls` |
| Java | `lsp4j` |
| TypeScript | `vscode-languageserver-node` |
| Rust | `tower-lsp` |
| Go | `glsp` |

---

## Architecture: ANTLR4 + LSP

```
Source file
    ↓
ANTLR4 Lexer + Parser  ←── your .g4 grammar
    ↓
Parse Tree
    ↓
ErrorListener → syntax errors (line, col, message)
Visitor/Listener → semantic analysis (type errors, undefined vars, etc.)
    ↓
LSP Server (pygls / lsp4j / vscode-languageserver)
    ↓
Diagnostics pushed to client (Claude Code, VS Code, etc.)
```

---

## Core Pipeline Components

| Component | What it does |
|---|---|
| Lexer | Tokenizes raw text |
| Parser | Builds parse tree from tokens |
| AST Builder | Transforms parse tree → cleaner AST |
| Symbol Table | Tracks declared variables, functions, scopes |
| Type Checker | Resolves and validates types |
| Error Listener | Collects syntax errors |
| Semantic Analyzer | Checks meaning (undefined vars, wrong arg count, etc.) |

---

## Execution Components

| Component | What it does |
|---|---|
| Interpreter | Walk AST, execute directly |
| Compiler | AST → bytecode or machine code |
| Transpiler | AST → another language (e.g., your lang → JavaScript) |
| Optimizer | Transform AST to improve performance |
| REPL | Interactive interpreter loop |

---

## LSP Tooling Features

| Component | What it does |
|---|---|
| Linter | Style + semantic warnings |
| Formatter / Pretty-printer | AST → formatted source code |
| Syntax highlighter | Semantic token coloring |
| Autocomplete provider | Context-aware suggestions |
| Hover provider | Type info on hover |
| Go-to-definition | Symbol resolution |
| Find references | Usages of a symbol |
| Rename refactoring | Rename symbol everywhere |
| Code folding | Collapse blocks based on tree |
| Document outline | File structure / symbol list |
| Call hierarchy | Who calls what |

---

## Other Components

| Component | What it does |
|---|---|
| Debugger (DAP) | Breakpoints, step, inspect — via Debug Adapter Protocol |
| Doc generator | Extract comments from AST → HTML/Markdown docs |
| Test runner | Execute test cases written in your language |
| Macro/Preprocessor | Transform code before parsing |
