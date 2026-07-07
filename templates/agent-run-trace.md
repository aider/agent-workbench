# Generated Agent Operation Trace Template

Use this template to debug a slow or stuck generated agent.

## Key idea

A generated agent should record an operation tree:

```text
phase -> operation -> optional sub_operation
```

Every planned operation must finish as one of:

```text
END | SKIP | ERROR
```

If the run hangs, the likely stuck place is the last `START` without `END`, `SKIP`, or `ERROR`.

## Trace file path

Use this path only for complex write-capable agents:

```text
.agent-runs/<trace-id>.md
```

For normal runs, a compact final trace summary is enough.

## Trace entry format

```text
trace_id: <id>
op_id: <phase.operation.number>
parent_id: <parent op_id or none>
level: phase | operation | sub_operation
event: PLAN | START | END | SKIP | ERROR
agent: <agent-name>
phase: <phase-name>
operation: <operation-name>
time: <timestamp or step number>
elapsed_ms: <number or unknown>
evidence: <file, command, tool, or observation>
status: planned | running | success | skipped | failed
```

## Minimal example

```text
trace_id: run-001
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

trace_id: run-001
op_id: 1.1
parent_id: 1
level: operation
event: START
agent: project-investigator
phase: discovery
operation: scan src files
time: step-02
elapsed_ms: unknown
evidence: Glob src/**/*.java
status: running

trace_id: run-001
op_id: 1.1
parent_id: 1
level: operation
event: END
agent: project-investigator
phase: discovery
operation: scan src files
time: step-03
elapsed_ms: unknown
evidence: found 42 files
status: success

trace_id: run-001
op_id: 2.1
parent_id: 2
level: operation
event: START
agent: project-investigator
phase: analysis
operation: inspect dependencies
time: step-04
elapsed_ms: unknown
evidence: Grep Feign|WebClient|RestTemplate
status: running
```

If the trace stops here, the likely stuck operation is:

```text
project-investigator / analysis / inspect dependencies
```

## Required final summary

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

## Required phase table

```text
| Phase | Operations | Completed | Skipped | Failed | Elapsed |
|---|---:|---:|---:|---:|---:|
| discovery | 3 | 3 | 0 | 0 | unknown |
| analysis | 4 | 3 | 0 | 0 | unknown |
```

## Low-overhead rule

- Keep trace entries in run notes while working.
- Write one compact trace summary at the end.
- Write `.agent-runs/<trace-id>.md` only for complex write-capable agents.
- Do not write trace files after every operation.
- Do not log every sentence or minor internal step.
