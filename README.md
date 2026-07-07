# Agent Workbench

A small workbench for designing AI agent workflows without turning global instructions into a wall of hard rules.

The goal is simple: let `agent-architect` decide the structure, keep stable project rules small, move repeatable procedures into skills, and use focused subagents when the workflow is too large for one agent.

## Repository layout

```text
AGENTS.md                              Project operating rules for agents
.claude/agents/agent-architect.md      Main entry point for messy workflows
.claude/agents/agent-writer.md         Writes specific agents, skills, templates, and contracts
.claude/agents/verifier.md             Verifies changes against a contract
skills/verify-change/SKILL.md          Reusable verification workflow

templates/
  agent-template.md                    Template for a new subagent
  concise-output.md                    Default short output format
  flow-architecture.md                 Template for splitting messy workflows
  verification-contract.md             Template for defining done and evidence

examples/
  first-agent-request.md               Simple agent creation example
  messy-workflow-to-agent-flow.md      Example where architect decides the split
```

## Core idea

Do not fix agent reliability by adding more and more global rules.

Use this split instead:

1. **Agent architect** decides the workflow structure.
2. **Agent writer** creates or updates specific artifacts.
3. **Verification contract** defines what must be true before the work is done.
4. **Verifier** checks the result and reports evidence.
5. **Skills** capture repeatable procedures so they do not live in every prompt.

## Quick start

Clone the repo and open it in Claude Code:

```bash
git clone https://github.com/aider/agent-workbench.git
cd agent-workbench
claude
```

Start with `agent-architect`:

```text
Use agent-architect.

Here is my messy workflow:
<paste workflow>

Design the agent flow yourself.
If instructions are too large, split them into subagents or skills.
Keep the output short and clear.
Create or update files.
Prepare verification criteria.
```

## When to use each piece

Use `agent-architect` when the workflow is unclear, too large, or needs splitting.

Use `agent-writer` only after the architecture is known and a specific file needs to be written.

Use `verifier` after changes are made and you need evidence that the result matches the contract.

Use `verify-change` when you need a reusable verification checklist.

## Skill installation note

This repo keeps shared skills under `skills/` as source files. Claude Code project skills normally live under `.claude/skills/<skill-name>/SKILL.md`.

To use the `verify-change` skill directly in this project, copy or symlink it:

```bash
mkdir -p .claude/skills
ln -s ../../skills/verify-change .claude/skills/verify-change
```

On Windows PowerShell, copy it instead:

```powershell
New-Item -ItemType Directory -Force .claude\skills
Copy-Item -Recurse skills\verify-change .claude\skills\verify-change
```

## Working principles

- Start with architecture, not more rules.
- Keep global instructions short.
- Move long procedures into skills.
- Move role-specific work into subagents.
- Give every agent a clear trigger in `description`.
- Give write-capable agents only the tools they need.
- Keep default output short and clear.
- Make verification explicit before calling work done.
- If a check was not run, say that it was not run.
- Prefer evidence over confidence.

## First milestone

This version gives you a minimal agent architecture workbench:

- an architect agent
- a writer agent
- a verifier agent
- one verification skill
- templates for agents, flows, short output, and verification contracts
- examples for simple and messy workflows
