# Example: Operation-Tree Profiling for Generated Agents

Use this example when the workbench creates agents and those generated agents may later be slow or stuck.

## Problem

```text
I use the workbench to create generated agents.
One generated agent has its own multi-step flow.
When I run that generated agent, it sometimes works slowly or appears stuck.
I need to know which phase or operation inside that generated agent caused the delay.
```

## Required behavior

When `agent-architect` or `agent-writer` creates a non-trivial generated agent, it should add operation-tree profiling.

The profiling belongs inside the generated agent.

Values that change by project or run should come from config, supporting files, specialized skills, or runtime input.

## Generated agent must model the run as

```text
phase -> operation -> optional sub_operation
```

Every planned operation must finish as:

```text
END | SKIP | ERROR
```

## Example generated trace

```text
trace_id: run-101
op_id: 1
parent_id: none
level: phase
event: PLAN
agent: generated-agent
phase: discovery
operation: discover configured targets
time: step-01
elapsed_ms: unknown
evidence: planned from config
status: planned

trace_id: run-101
op_id: 1.1
parent_id: 1
level: operation
event: START
agent: generated-agent
phase: discovery
operation: read configured target set
time: step-02
elapsed_ms: unknown
evidence: config target set
status: running

trace_id: run-101
op_id: 1.1
parent_id: 1
level: operation
event: END
agent: generated-agent
phase: discovery
operation: read configured target set
time: step-03
elapsed_ms: unknown
evidence: targets loaded
status: success

trace_id: run-101
op_id: 2
parent_id: none
level: phase
event: PLAN
agent: generated-agent
phase: analysis
operation: analyze configured signals
time: step-04
elapsed_ms: unknown
evidence: planned from config
status: planned

trace_id: run-101
op_id: 2.1
parent_id: 2
level: operation
event: START
agent: generated-agent
phase: analysis
operation: inspect configured signal group
time: step-05
elapsed_ms: unknown
evidence: configured signal group
status: running
```

## Diagnosis

Because `2.1` has `START` and no final state, the likely stuck point is:

```text
generated-agent / analysis / inspect configured signal group
```

## Expected trace summary

```text
Trace:
- trace_id: run-101
- phases: 2
- operations: 2
- completed: 1
- skipped: 0
- failed: 0
- slowest phase: unknown
- slowest operation: unknown
- stuck point: generated-agent/analysis/inspect configured signal group
```

## Expected phase table

```text
| Phase | Operations | Completed | Skipped | Failed | Elapsed |
|---|---:|---:|---:|---:|---:|
| discovery | 1 | 1 | 0 | 0 | unknown |
| analysis | 1 | 0 | 0 | 0 | unknown |
```

## Prompt to analyze it

```text
Use agent-flow-profiler.

Analyze this generated-agent trace and tell me where it got stuck:
<paste trace>

Keep output short.
```

## Expected answer

```text
Result: Likely stuck in analysis.
Slowest:
- phase: unknown
- operation: unknown
Stuck:
- generated-agent/analysis/inspect configured signal group
Evidence:
- operation 2.1 has START and no END/SKIP/ERROR.
Next:
- Split that configured signal group into smaller operations or narrow the configured target set.
```
