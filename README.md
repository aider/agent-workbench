# Agent Workbench

A small workbench for designing AI agent workflows without turning global instructions into a wall of hard rules.

The goal is simple: let `agent-architect` decide the structure, keep stable project rules small, move repeatable procedures into skills, add phase-level tracing for complex flows, and use focused subagents when the workflow is too large for one agent.

## Repository layout

```text
AGENTS.md                              Project operating rules for agents
.claude/agents/agent-architect.md      Main entry point for messy workflows
.claude/agents/agent-flow-profiler.md  Profiles slow agent runs using phase traces
.claude/agents/agent-writer.md         Writes specific agents, skills, templates, and contracts
.claude/agents/performance-profiler.md Profiles slow apps, APIs, tests, builds, DB queries, infra, or agent flows
.claude/agents/verifier.md             Verifies changes against a contract
.claude/skills/agent-flow-trace/SKILL.md Adds phase-level tracing to agent workflows
.claude/skills/architect-flow/SKILL.md Project skill for turning messy workflows into agent flows
.claude/skills/profile-slow-flow/SKILL.md Project skill for profiling slow behavior
skills/verify-change/SKILL.md          Shared verification workflow

templates/
  agent-run-trace.md                   Template for debugging slow or stuck agent runs
  agent-template.md                    Template for a new subagent
  concise-output.md                    Default short output format
  flow-architecture.md                 Template for splitting messy workflows
  verification-contract.md             Template for defining done and evidence
  workbench-verification-contract.md   Contract for verifying this workbench

examples/
  agent-flow-hang-debug.md             Example for finding a stuck agent phase
  first-agent-request.md               Simple agent creation example
  messy-workflow-to-agent-flow.md      Example where architect decides the split
  profile-slow-flow.md                 Example for profiling slow behavior
```

## Core idea

Do not fix agent reliability by adding more and more global rules.

Use this split instead:

1. **Architect-flow skill** gives a short repeatable entry point.
2. **Agent architect** decides the workflow structure and trace points.
3. **Agent writer** creates or updates specific artifacts.
4. **Agent-flow profiler** finds slow or stuck agent phases from trace logs.
5. **Performance profiler** investigates slow app, build, infra, or service behavior before fixes.
6. **Verification contract** defines what must be true before the work is done.
7. **Verifier** checks the result and reports evidence.
8. **Skills** capture repeatable procedures so they do not live in every prompt.

## Quick start

Clone the repo and open it in Claude Code:

```bash
git clone https://github.com/aider/agent-workbench.git
cd agent-workbench
claude
```

Use the project skill for messy workflows:

```text
/architect-flow

Here is my messy workflow:
<paste workflow>
```

Use tracing when a generated agent flow is slow or can hang:

```text
/agent-flow-trace

Add trace logging to this workflow:
<paste workflow or files>
```

Use the profiler to find where an agent run got stuck:

```text
Use agent-flow-profiler.

Analyze this trace and tell me where it is stuck:
<paste trace>
```

Use the profiling skill for app, test, build, infra, or service slowness:

```text
/profile-slow-flow

This is slow:
<paste symptom, timing, logs, trace, or file path>
```

## Hang diagnosis rule

If an agent workflow hangs, inspect the trace file.

The likely stuck point is the last `START` entry without a matching `END`.

```text
START agent-writer/write/update file
# no END
```

Diagnosis:

```text
Result: Likely stuck at agent-writer/write/update file.
Next: split or inspect that operation.
```

## When to use each piece

Use `/architect-flow` when you want the shortest repeatable entry point.

Use `agent-architect` when the workflow is unclear, too large, or needs splitting.

Use `/agent-flow-trace` when generated agents need observable phases.

Use `agent-flow-profiler` when an agent run is slow or stuck and you have trace data.

Use `agent-writer` only after the architecture is known and a specific file needs to be written.

Use `/profile-slow-flow` when an app, test, build, infra flow, or service is slow and you want profiling before fixing.

Use `performance-profiler` when you need a read-only investigation of latency, timeout, CPU, memory, DB, build, test, infra, or service slowness.

Use `verifier` after changes are made and you need evidence that the result matches the contract.

Use `verify-change` when you need a reusable verification checklist.

## Working principles

- Start with architecture, not more rules.
- Keep global instructions short.
- Move long procedures into skills.
- Move role-specific work into subagents.
- Add phase-level tracing to complex agent flows.
- Profile slow behavior before changing code.
- Give every agent a clear trigger in `description`.
- Give write-capable agents only the tools they need.
- Keep default output short and clear.
- Make verification explicit before calling work done.
- If a check was not run, say that it was not run.
- Prefer evidence over confidence.

## First milestone

This version gives you a minimal agent architecture workbench:

- an architect-flow skill
- an agent-flow-trace skill
- a profile-slow-flow skill
- an architect agent
- an agent-flow profiler agent
- a writer agent
- a performance profiler agent
- a verifier agent
- one shared verification skill
- templates for agents, traces, flows, short output, and verification contracts
- examples for simple, messy, slow, and stuck workflows
