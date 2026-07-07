# Agent Workbench

A small workbench for designing AI agents without turning global instructions into a wall of hard rules.

The goal is simple: let `agent-architect` decide the structure, keep stable project rules small, move repeatable procedures into skills, and make generated agents observable by default.

## Repository layout

```text
AGENTS.md                              Project operating rules for agents
.claude/agents/agent-architect.md      Main entry point for messy workflows
.claude/agents/agent-flow-profiler.md  Analyzes traces from generated agents
.claude/agents/agent-writer.md         Writes specific agents, skills, templates, and contracts
.claude/agents/performance-profiler.md Profiles slow apps, APIs, tests, builds, DB queries, infra, or service flows
.claude/agents/verifier.md             Verifies changes against a contract
.claude/skills/agent-flow-trace/SKILL.md Adds built-in profiling to generated agents
.claude/skills/architect-flow/SKILL.md Project skill for turning messy workflows into agent flows
.claude/skills/instrument-generated-agent/SKILL.md Instruments agents created by the workbench
.claude/skills/profile-slow-flow/SKILL.md Project skill for profiling app, test, build, infra, or service slowness
skills/verify-change/SKILL.md          Shared verification workflow

templates/
  agent-run-trace.md                   Template for debugging slow or stuck generated agents
  agent-template.md                    Template for a new subagent
  concise-output.md                    Default short output format
  flow-architecture.md                 Template for splitting messy workflows
  verification-contract.md             Template for defining done and evidence
  workbench-verification-contract.md   Contract for verifying this workbench

examples/
  agent-flow-hang-debug.md             Example for finding a stuck agent phase
  first-agent-request.md               Simple agent creation example
  generated-agent-profiling.md         Example for profiling agents created by the workbench
  messy-workflow-to-agent-flow.md      Example where architect decides the split
  profile-slow-flow.md                 Example for profiling app, test, build, infra, or service slowness
```

## Core idea

Do not fix agent reliability by adding more and more global rules.

Use this split instead:

1. **Architect-flow skill** gives a short repeatable entry point.
2. **Agent architect** decides the workflow structure and whether generated agents need profiling hooks.
3. **Agent writer** creates or updates agents and adds built-in profiling to non-trivial generated agents.
4. **Agent-flow profiler** analyzes traces from generated agents when one is slow or stuck.
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

Create an agent flow from a messy workflow:

```text
/architect-flow

Here is my messy workflow:
<paste workflow>
```

Make sure generated agents include profiling hooks:

```text
/agent-flow-trace

Add built-in profiling to this generated agent:
<paste agent file or design>
```

Analyze a slow or stuck generated agent:

```text
Use agent-flow-profiler.

Analyze this trace from a generated agent and tell me where it is stuck:
<paste trace>
```

Use the profiling skill for app, test, build, infra, or service slowness:

```text
/profile-slow-flow

This is slow:
<paste symptom, timing, logs, trace, or file path>
```

## Generated-agent hang diagnosis rule

If a generated agent hangs, inspect its trace.

The likely stuck point is the last `START` entry without a matching `END`.

```text
START project-investigator/analysis/inspect dependencies
# no END
```

Diagnosis:

```text
Result: Likely stuck at project-investigator/analysis/inspect dependencies.
Next: split or limit that operation.
```

## When to use each piece

Use `/architect-flow` when you want the shortest repeatable entry point.

Use `agent-architect` when the workflow is unclear, too large, or needs splitting.

Use `/agent-flow-trace` or `instrument-generated-agent` when generated agents need built-in profiling.

Use `agent-flow-profiler` when a generated agent is slow or stuck and you have trace data.

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
- Add built-in profiling to non-trivial generated agents.
- Profile slow app or service behavior before changing code.
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
- an instrument-generated-agent skill
- a profile-slow-flow skill
- an architect agent
- an agent-flow profiler agent
- a writer agent
- a performance profiler agent
- a verifier agent
- one shared verification skill
- templates for agents, traces, flows, short output, and verification contracts
- examples for simple, messy, generated-agent profiling, slow, and stuck workflows
