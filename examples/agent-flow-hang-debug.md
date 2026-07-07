# Example: Debug a Stuck Agent Flow

Use this example when an agent workflow creates agents, runs them, and one run becomes slow or appears stuck.

## Problem

```text
I created an agent flow.
The flow creates or runs other agents.
One agent run is slow or stuck.
I need to know exactly which agent, phase, or operation caused the delay.
```

## Required design

Generated complex agents should include phase-level trace logging.

Flow:

```text
agent with trace logging -> agent-flow-profiler -> suggested improvement
```

## Trace rule

Every major operation writes:

```text
START <agent>/<phase>/<operation>
END   <agent>/<phase>/<operation>
```

If the workflow hangs, find the last `START` without a matching `END`.

That is the likely stuck point.

## Example trace

```text
trace_id: run-42
event: START
agent: agent-architect
phase: discovery
operation: scan existing agents
time: step-01
elapsed_ms: unknown
evidence: .claude/agents/*.md
status: running

trace_id: run-42
event: END
agent: agent-architect
phase: discovery
operation: scan existing agents
time: step-02
elapsed_ms: unknown
evidence: found 5 agents
status: success

trace_id: run-42
event: START
agent: agent-writer
phase: write
operation: generate new subagent
time: step-03
elapsed_ms: unknown
evidence: .claude/agents/new-agent.md
status: running
```

## Diagnosis

```text
Result: Likely stuck at agent-writer/write/generate new subagent.
Evidence:
- Last START has no matching END.
Next:
- Split generation into smaller phases or inspect the target file write.
```

## Prompt to run

```text
Use agent-flow-profiler.

Analyze this trace and tell me where the agent workflow is stuck:
<paste trace>

Keep output short.
```

## What good instrumentation looks like

Trace only important phases:

- discovery
- planning
- broad search
- read many files
- write files
- run commands
- verify
- handoff

Do not trace every sentence.
