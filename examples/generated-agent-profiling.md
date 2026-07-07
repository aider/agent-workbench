# Example: Operation-Tree Profiling for Generated Agents

Use this example when the workbench creates agents and those generated agents may later be slow or stuck.

## Problem

```text
I use the workbench to create different agents.
One generated agent has its own multi-step flow.
When I run that generated agent, it sometimes works slowly or appears stuck.
I need to know which phase or operation inside that generated agent caused the delay.
```

## Required behavior

When `agent-architect` or `agent-writer` creates a non-trivial generated agent, it should add operation-tree profiling.

The profiling belongs inside the generated agent.

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
agent: project-investigator
phase: discovery
operation: discover project files
time: step-01
elapsed_ms: unknown
evidence: planned
status: planned

trace_id: run-101
op_id: 1.1
parent_id: 1
level: operation
event: START
agent: project-investigator
phase: discovery
operation: scan source files
time: step-02
elapsed_ms: unknown
evidence: Glob src/**/*.java
status: running

trace_id: run-101
op_id: 1.1
parent_id: 1
level: operation
event: END
agent: project-investigator
phase: discovery
operation: scan source files
time: step-03
elapsed_ms: unknown
evidence: found 42 files
status: success

trace_id: run-101
op_id: 2
parent_id: none
level: phase
event: PLAN
agent: project-investigator
phase: analysis
operation: analyze service dependencies
time: step-04
elapsed_ms: unknown
evidence: planned
status: planned

trace_id: run-101
op_id: 2.1
parent_id: 2
level: operation
event: START
agent: project-investigator
phase: analysis
operation: inspect dependency usage
time: step-05
elapsed_ms: unknown
evidence: Grep Feign|RestTemplate|WebClient
status: running
```

## Diagnosis

Because `2.1` has `START` and no final state, the likely stuck point is:

```text
project-investigator / analysis / inspect dependency usage
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
- stuck point: project-investigator/analysis/inspect dependency usage
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
- project-investigator/analysis/inspect dependency usage
Evidence:
- operation 2.1 has START and no END/SKIP/ERROR.
Next:
- Split dependency inspection into smaller operations or limit file scope.
```
