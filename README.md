# Agent Workbench

A small workbench for designing and reviewing generated AI agents without turning global instructions into a wall of hard rules.

The core goal is simple: create focused reusable agents from messy workflows, review existing agent flows before rewriting them, keep variable run values outside agent instructions, add support scripts for deterministic generated-agent checks when useful, show each non-trivial flow as a small diagram, verify the result, and make generated agents observable through operation-tree profiling.

## Repository layout

```text
AGENTS.md                                      Project operating rules
REVIEW.md                                      Short entrypoint for reviewing existing external agent flows
.claude/agents/agent-architect.md              Designs the generated-agent architecture
.claude/agents/agent-flow-reviewer.md          Reviews existing external agent flows before rewrite
.claude/agents/agent-writer.md                 Writes specific agents, skills, templates, support scripts, and contracts
.claude/agents/agent-flow-profiler.md          Analyzes operation-tree traces from generated agents
.claude/agents/verifier.md                     Verifies changes against a contract
.claude/skills/architect-flow/SKILL.md         Short entry point for messy workflows
.claude/skills/instrument-generated-agent/SKILL.md Adds operation-tree profiling to generated agents
skills/verify-change/SKILL.md                  Shared verification workflow

templates/
  agent-run-trace.md                           Template for generated-agent operation traces
  agent-template.md                            Template for a generated subagent
  concise-output.md                            Default short output format
  flow-architecture.md                         Template for splitting messy workflows
  generated-agent-config.md                    Template for values supplied outside reusable agents
  generated-agent-flow-diagram.md             Template for generated-agent block diagrams
  generated-agent-support-scripts.md          Template for generated-agent support scripts
  verification-contract.md                     Template for defining done and evidence
  workbench-verification-contract.md           Contract for verifying this workbench

examples/
  agent-flow-hang-debug.md                     Example for finding a stuck generated-agent operation
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

For existing external agent systems that need review before rewrite:

```text
existing agent system -> agent-flow-reviewer -> evidence-based improvement plan
```

## What the workbench does

1. `agent-architect` reads a messy workflow and decides the smallest useful structure.
2. `agent-architect` owns the generated-agent flow diagram.
3. `agent-architect` decides whether generated-agent support scripts are useful.
4. `agent-writer` creates the actual agent, skill, template, support script, or contract.
5. Values that vary by project or run are supplied through config, supporting files, specialized skills, or runtime input.
6. Non-trivial generated agents get operation-tree profiling.
7. `verifier` checks the changed files against evidence.
8. `agent-flow-profiler` can later analyze the trace from a generated agent.
9. `agent-flow-reviewer` audits an existing external agent system before any rewrite.

## Review an existing agent flow

Use the reviewer when the first goal is understanding an existing agent system.

The reviewer should inventory files, reconstruct the actual flow, identify dependencies and role boundaries, then propose a prioritized plan with evidence.

It does not rewrite by default.

Primary prompt:

```text
Review <target-project>.
```

`AGENTS.md` routes review requests to `.claude/agents/agent-flow-reviewer.md`.

`REVIEW.md` is an optional reference entrypoint, not required.

```text
Use agent-flow-reviewer.

Review this existing agent flow from another project.
Understand how it works.
Find where it is confusing, overloaded, hardcoded, unverified, or hard to debug.
Propose a plan to improve it so it matches the workbench standards.
```

The review checks:

- inventory beyond README
- actual flow and artifact dependencies
- role separation similar to `architect -> writer -> verifier`
- external input boundary
- support scripts as prepared deterministic tools
- ad hoc helper code during normal flow
- flow diagrams
- operation-tree profiling
- verifier separation and evidence checks

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
- support-script contract
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

## Generated-agent support scripts

Support scripts are optional tools for agents created by this workbench.

Use support scripts when a generated agent has a predictable deterministic action that should already exist before the normal flow runs.

The purpose is to prevent generated agents from writing ad hoc helper code during execution. The generated agent should call the prepared script instead.

Do not add scripts or Python packaging just because an agent exists.

When scripts are useful, the generated-agent architecture should define:

- script owner: agent, skill, or shared tool
- script location
- install command, if needed
- run command
- README update
- verifier check

Use `templates/generated-agent-support-scripts.md` as the default shape.

Preferred locations:

```text
.claude/agents/<agent-name>.md
.claude/agents/<agent-name>.scripts/<script>.py

.claude/skills/<skill-name>/SKILL.md
.claude/skills/<skill-name>/scripts/<script>.py
.claude/skills/<skill-name>/README.md

tools/<tool-name>.py

pyproject.toml
src/<package_name>/<module>.py
README.md
```

Only use the Python package layout when command-line tooling is actually needed.

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
- Review existing flows before rewriting them.
- Keep reusable agent logic separate from external inputs.
- Add support scripts for deterministic generated-agent checks when useful.
- Add a flow diagram to non-trivial generated agents.
- Add operation-tree profiling to non-trivial generated agents.
- Keep profiling low-overhead.
- Keep default output short and clear.
- Verify before calling work done.
- Prefer evidence over confidence.
