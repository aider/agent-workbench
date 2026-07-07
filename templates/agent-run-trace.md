# Agent Run Trace Template

Use this template to debug a slow or stuck agent workflow.

## Key idea

Every major phase writes a `START` and `END` entry.

If the run hangs, the likely stuck place is the last `START` entry without a matching `END`.

## Trace file path

Use this path pattern:

```text
.agent-runs/<trace-id>.md
```

Example:

```text
.agent-runs/2026-07-06-agent-flow-001.md
```

## Trace entry format

```text
trace_id: <id>
event: START | END | SKIP | ERROR
agent: <agent-name>
phase: <phase-name>
operation: <operation-name>
time: <timestamp or step number>
elapsed_ms: <number or unknown>
evidence: <file, command, tool, or observation>
status: running | success | failed | skipped
```

## Minimal example

```text
trace_id: run-001
event: START
agent: agent-architect
phase: discovery
operation: scan repo files
time: step-01
elapsed_ms: unknown
evidence: Glob **/*.md
status: running

trace_id: run-001
event: END
agent: agent-architect
phase: discovery
operation: scan repo files
time: step-02
elapsed_ms: unknown
evidence: found 12 files
status: success

trace_id: run-001
event: START
agent: agent-writer
phase: write
operation: update agent file
time: step-03
elapsed_ms: unknown
evidence: .claude/agents/example.md
status: running
```

If the trace stops here, the likely stuck operation is:

```text
agent-writer / write / update agent file
```

## Required phases for generated agents

At minimum, generated agents should trace:

- discovery
- planning
- file read
- file write
- command run
- verification
- handoff

## Debug rule

When diagnosing a slow run:

1. Find the latest trace file.
2. Find the last `START` entry.
3. Check whether it has a matching `END` entry.
4. If not, report that phase as the likely stuck point.
5. If all phases have `END`, compare elapsed time and repeated operations.

## Short diagnosis format

```text
Result: Likely stuck at <agent>/<phase>/<operation>.
Evidence:
- Last START without END: <trace entry>
Next:
- Inspect or split this operation.
```
