# Agent Workbench

A small workbench for creating, testing, and refining AI coding agents without turning global instructions into a wall of hard rules.

The goal is simple: keep stable project rules small, move repeatable procedures into skills, and use focused subagents for work that needs its own context.

## Repository layout

```text
AGENTS.md                              Project operating rules for agents
.claude/agents/agent-writer.md         Subagent that writes and refactors agents
.claude/agents/verifier.md             Subagent that verifies changes against a contract
skills/verify-change/SKILL.md          Reusable verification workflow

/ templates
  agent-template.md                    Template for a new subagent
  verification-contract.md             Template for defining done and evidence

/ examples
  first-agent-request.md               Example request for creating an agent
```

## Core idea

Do not solve agent reliability by adding more and more global rules.

Use this split instead:

1. **Agent writer** creates or refactors a focused agent from a workflow.
2. **Verification contract** defines what must be true before the work is done.
3. **Verifier** checks the result and reports evidence.
4. **Skill** captures repeatable verification steps so they do not live in every prompt.

## Quick start

Clone the repo and open it in Claude Code:

```bash
git clone https://github.com/aider/agent-workbench.git
cd agent-workbench
claude
```

Ask Claude Code:

```text
Use the agent-writer agent to create a focused subagent for this workflow:
<describe the workflow>

Use templates/verification-contract.md as the done contract.
After writing it, use the verifier agent to check the result.
```

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

- Keep global instructions short.
- Keep each subagent focused on one job.
- Give every agent a clear trigger in `description`.
- Give write-capable agents only the tools they need.
- Make verification explicit before calling work done.
- If a check was not run, say that it was not run.
- Prefer evidence over confidence.

## First milestone

This initial version gives you a minimal agent factory:

- a writer agent
- a verifier agent
- one verification skill
- templates for future agents and verification contracts
- one example request
