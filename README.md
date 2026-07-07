# Agent Workbench

A small workbench for designing generated AI agents without turning global instructions into a wall of hard rules.

The core goal is simple: create focused reusable agents from messy workflows, keep variable run values outside agent instructions, show each non-trivial flow as a small diagram, verify the result, and make generated agents observable through operation-tree profiling.

## Repository layout

```text
AGENTS.md                                      Project operating rules
.claude/agents/agent-architect.md              Designs the generated-agent architecture
.claude/agents/agent-writer.md                 Writes specific agents, skills, templates, and contracts
.claude/agents/agent-flow-profiler.md          Analyzes operation-tree traces from generated agents
.claude/agents/verifier.md                     Verifies changes against a contract
.claude/skills/architect-flow/SKILL.md         Short entry point for messy workflows
.claude/skills/instrument-generated-agent/SKILL.md Adds operation-tree profiling to generated agents
skills/verify-change/SKILL.md                  Shared verification workflow

templates/
  agent-template.md                            Template for a generated subagent
  concise-output.md                            Default short output format
  flow-architecture.md                         Template for splitting messy workflows
  generated-agent-config.md                    Template for values supplied outside reusable agents
  generated-agent-flow-diagram.md             Template for generated-agent block diagrams
  verification-contract.md                     Template for defining done and evidence
  workbench-verification-contract.md           Contract for verifying this workbench

examples/
  first-agent-request.md                       Simple agent creation example
  generated-agent-profiling.md                 Example for profiling agents created by the workbench
  messy-workflow-to-agent-flow.md              Example where architect decides the split
```

## Core flow

```text
/architect-flow -> agent-architect -> agent-writer -> verifier
```

For generated agents that later run slowly or get stuck:

```text
generated agent trace -> agent-flow-profiler
```

## What the workbench does

1. `agent-architect` reads a messy workflow and decides the smallest useful structure.
2. `agent-architect` owns the generated-agent flow diagram.
3. `agent-writer` creates the actual agent, skill, template, or contract.
4. Values that vary by project or run are supplied through config, supporting files, specialized skills, or runtime input.
5. Non-trivial generated agents get operation-tree profiling.
6. `verifier` checks the changed files against evidence.
7. `agent-flow-profiler` can later analyze the trace from a generated agent.

## Configuration boundary

Generated agents should contain reusable behavior, not values that vary by project or run.

Keep inside the agent:

- role
- workflow logic
- decision rules
- tool boundaries
- input contract
- output format
- verification handoff
- flow diagram shape
- operation-tree profiling structure

Keep outside the agent:

- variable values
- lookup terms
- file locations
- command strings
- labels
- runtime settings
- reference records

Use `templates/generated-agent-config.md` for the shape of those external inputs.

## Flow diagrams

For every non-trivial generated agent, create a small block diagram.

Use a diagram when the generated agent has more than one meaningful phase, branch, handoff, verification step, or operation-tree trace.

The diagram should show:

- input
- phases
- decisions
- handoffs
- verification
- output
- where operation-tree trace is recorded

Use `templates/generated-agent-flow-diagram.md` as the default shape.

## Operation-tree profiling

Generated agents should model their work as:

```text
phase -> operation -> optional sub_operation
```

Every planned operation must finish as one of:

```text
END | SKIP | ERROR
```

No planned operation should disappear silently.

If an operation is skipped, record `SKIP` with a short reason.

If a generated agent hangs, the likely stuck point is the last `START` without `END`, `SKIP`, or `ERROR`.

## Low-overhead rule

Profiling must not become the bottleneck.

Use this approach:

- keep trace entries in run notes while working
- write one compact trace summary at the end
- write `.agent-runs/<trace-id>.md` only for complex write-capable agents
- do not write trace files after every operation
- do not log every sentence or minor internal step

## Required trace summary

Generated agents should report:

```text
Trace:
- trace_id: <id or none>
- phases: <count>
- operations: <count>
- completed: <count>
- skipped: <count>
- failed: <count>
- slowest phase: <phase or unknown>
- slowest operation: <operation or unknown>
- stuck point: <last START without END/SKIP/ERROR or none>
```

For multi-phase runs, include a compact phase table:

```text
| Phase | Operations | Completed | Skipped | Failed | Elapsed |
|---|---:|---:|---:|---:|---:|
| discovery | 3 | 3 | 0 | 0 | unknown |
```

## Quick start

```bash
git clone https://github.com/aider/agent-workbench.git
cd agent-workbench
claude
```

Run:

```text
/architect-flow

Here is my messy workflow:
<paste workflow>
```

## Analyze a slow generated agent

```text
Use agent-flow-profiler.

Analyze this trace from a generated agent:
<paste trace>
```

## Working principles

- Start with architecture, not more global rules.
- Keep global instructions short.
- Move repeatable procedures into skills.
- Move role-specific work into subagents.
- Keep reusable agent logic separate from external inputs.
- Add a flow diagram to non-trivial generated agents.
- Add operation-tree profiling to non-trivial generated agents.
- Keep profiling low-overhead.
- Keep default output short and clear.
- Verify before calling work done.
- Prefer evidence over confidence.
