# Example: Debug a Stuck Agent Flow

Use this example when a generated agent run becomes slow or appears stuck.

## Problem

```text
I ran a generated agent.
The generated agent has a multi-step flow.
One phase or operation is slow or stuck.
I need to know exactly which agent, phase, or operation caused the delay.
```

## Required design

Generated non-trivial agents should include operation-tree profiling.

Flow:

```text
generated agent trace -> agent-flow-profiler -> suggested improvement
```

## Trace rule

Generated agents model work as:

```text
phase -> operation -> optional sub_operation
```

Every planned operation ends as:

```text
END | SKIP | ERROR
```

If an operation is skipped, record `SKIP` with a short reason.

If the workflow hangs, find the last `START` without `END`, `SKIP`, or `ERROR`.

That is the likely stuck point.

## Example trace

```text
trace_id: run-42
op_id: 1
parent_id: none
level: phase
event: PLAN
agent: generated-agent
phase: discovery
operation: discover configured targets
time: step-01
elapsed_ms: unknown
evidence: planned from external input
reason: none
status: planned

trace_id: run-42
op_id: 1.1
parent_id: 1
level: operation
event: START
agent: generated-agent
phase: discovery
operation: read configured target set
time: step-02
elapsed_ms: unknown
evidence: configured targets
reason: none
status: running

trace_id: run-42
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
reason: none
status: success

trace_id: run-42
op_id: 2
parent_id: none
level: phase
event: PLAN
agent: generated-agent
phase: analysis
operation: analyze configured signals
time: step-04
elapsed_ms: unknown
evidence: planned from external input
reason: none
status: planned

trace_id: run-42
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
reason: none
status: running
```

## Diagnosis

```text
Result: Likely stuck in analysis.
Evidence:
- Operation 2.1 has START and no END/SKIP/ERROR.
Next:
- Split that configured signal group into smaller operations or narrow the configured target set.
```

## Prompt to run

```text
Use agent-flow-profiler.

Analyze this trace from a generated agent and tell me where it is stuck:
<paste trace>

Keep output short.
```

## What good instrumentation looks like

Trace only important operations:

- discovery
- planning
- broad search
- read many files
- write files
- run commands
- verify
- handoff

Do not trace every sentence.

Keep trace entries in run notes while working and write one compact trace summary at the end. Write a trace file only for complex write-capable generated agents.
