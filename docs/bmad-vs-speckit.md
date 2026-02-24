# BMAD vs SpecKit: A Comparison

## Overview

| Dimension           | **SpecKit** (GitHub)                                               | **BMAD Method**                                                 |
| ------------------- | ------------------------------------------------------------------ | --------------------------------------------------------------- |
| **Full name**       | Spec-Driven Development Toolkit                                    | Breakthrough Method of Agile AI Driven Development              |
| **Core idea**       | Specs _are_ the driver — executable specs generate implementations | AI agents guide you through the full dev lifecycle              |
| **Philosophy**      | Spec-first: write the spec, then AI builds from it                 | Agent-first: specialized AI personas orchestrate phases         |
| **Workflow**        | Linear: Constitution → Specify → Plan → Tasks → Implement          | Phased: Discovery → Architecture → Stories → Dev                |
| **Artifacts**       | Constitution, Specs, Plan, Task list                               | PRD, Architecture doc, User stories, Agent prompts              |
| **AI coupling**     | Agnostic — works with Claude, Copilot, Cursor, Gemini              | Deeply integrated with agent personas (Analyst, Architect, Dev) |
| **Entry point**     | CLI (`specify init`) + slash commands                              | Slash commands within AI chat (e.g. `/bmad-help`)               |
| **Maturity/origin** | GitHub-backed, newer, tool-centric                                 | Community-driven, broader adoption, docs-heavy                  |
| **Extensibility**   | Add custom commands                                                | BMad Builder for custom agents/modules                          |
| **Target user**     | Devs who want structured AI-assisted coding                        | Devs comfortable with LLM tools wanting workflow scaffolding    |

## Do They Require Calling LLM APIs?

**No.** Both are prompt frameworks / slash command packs that run _inside_ your existing AI coding tool.

|                            | **SpecKit**                                           | **BMAD**                                      |
| -------------------------- | ----------------------------------------------------- | --------------------------------------------- |
| **How it works**           | Slash commands inside Claude, Copilot, Cursor, Gemini | Slash commands inside Claude, Cursor, Copilot |
| **You need an API key?**   | No — uses whatever AI tool you already have           | No — same                                     |
| **Who calls the LLM?**     | Your AI coding tool does, under the hood              | Your AI coding tool does, under the hood      |
| **What they actually are** | Prompt templates / skills installed into your AI tool | Agent persona prompts + workflow guides       |

Think of them like **Claude Code skills** — structured prompts that extend what your AI assistant does, not standalone apps that hit an API.

## Key Difference in Mental Model

- **SpecKit** treats the _specification document_ as the source of truth — it drives everything. The spec is almost a contract the AI executes against.
- **BMAD** treats _AI agents_ as the driver — each agent (Analyst, Architect, Developer) has a role and hands off context to the next, more like an AI-powered team.

Both are effectively Claude Code skills — sets of slash commands that inject structured prompts into your AI tool. The difference is intent:

|              | SpecKit                       | BMAD                              |
| ------------ | ----------------------------- | --------------------------------- |
| Core value   | Spec quality                  | Process discipline                |
| Analogy      | A doc template system with AI | An AI-powered scrum team          |
| End artifact | Well-written specs → code     | Working software via agile phases |
| Mental model | "Spec first, build second"    | "Plan → Sprint → Ship"            |

## When to Use Each

```
Unclear what to build?  →  SpecKit (clarify first)
Clear what to build?    →  BMAD (execute efficiently)
```

**Use SpecKit when:**

- Starting a new project and requirements are fuzzy
- You're a solo dev who wants structured spec discipline without process overhead
- Your main problem is miscommunication between intent and implementation
- You want traceability: requirement → spec → task → code

**Use BMAD when:**

- You already know roughly what to build and need help executing it
- You're managing a complex project with multiple concerns (frontend, backend, infra)
- You think in sprints/stories and want AI to play team roles
- You want an opinionated end-to-end process, not just better docs

**Or combined:** SpecKit (define) → BMAD (execute)

## Fit for Company Environment

**BMAD** is the better fit for most company environments.

| Company type                                     | Recommendation                                       |
| ------------------------------------------------ | ---------------------------------------------------- |
| Startup, small team, solo                        | SpecKit or SpecKit → BMAD                            |
| Mid-size with agile process                      | BMAD                                                 |
| Enterprise with product/design/dev/QA separation | BMAD (maps to org structure)                         |
| Agency building for clients                      | SpecKit first — client sign-off on spec before build |
| Open source / community project                  | SpecKit (spec as shared truth)                       |

## Alignment with Spec-First / Engineering Philosophy

> _"AI has shifted coding from 'what are you coding' to 'what are you designing.'"_

**SpecKit aligns more closely** with a spec-first, engineering-discipline philosophy.

| Philosophy Principle                                           | SpecKit                                              | BMAD                                      |
| -------------------------------------------------------------- | ---------------------------------------------------- | ----------------------------------------- |
| Human focuses on high-level design, AI on implementation       | ✅ Explicit — spec = human work, implement = AI work | Partial — agents blur this boundary       |
| Engineering discipline: requirements → design → implementation | ✅ Direct match: Specify → Plan → Tasks → Implement  | ✅ But less strict                        |
| Structured decomposition into isolated atomic units            | ✅ `/speckit.tasks` breaks specs into atomic tasks   | Partial                                   |
| Human as architect/decision-maker, AI as force multiplier      | ✅ Human writes the constitution + spec, AI executes | BMAD agents sometimes take over decisions |

SpecKit's linear phase structure mirrors the mechanical engineering analogy: requirements before design, design before build, review gates at each stage.

## Career Growth: Junior → Senior

**SpecKit grows the developer. BMAD makes the developer productive.**

```
Junior  →  writes code
Mid     →  breaks problems into tasks        ← BMAD helps here
Senior  →  designs systems, writes specs     ← SpecKit trains this
```

### Why BMAD Can Stunt Growth

BMAD's agent personas do the thinking _for_ you — Analyst figures out requirements, Architect designs the system, Developer implements. If you always let AI play these roles, you never build the mental muscles yourself.

### Why SpecKit Builds Seniority

| Senior skill                   | SpecKit forces you to...                               |
| ------------------------------ | ------------------------------------------------------ |
| Clarify ambiguous requirements | Write a constitution + spec before touching code       |
| Think in systems, not features | Define the full plan before breaking into tasks        |
| Communicate precisely          | Specs must be clear enough for AI to execute correctly |
| Catch problems early           | `/speckit.analyze` finds inconsistencies at spec time  |
| Own the design                 | You author the spec, AI is just a builder              |

**Use BMAD to ship faster today. Use SpecKit to become a better engineer tomorrow.**

The ideal path: use SpecKit deliberately as a learning discipline, even when it feels slower. The spec-writing habit is what senior engineers do naturally — SpecKit makes you practice it explicitly.
