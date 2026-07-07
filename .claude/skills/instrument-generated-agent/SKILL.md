---
description: Add low-overhead operation-tree profiling to agents created by this workbench. Use whenever generating or refactoring an agent that has phases, operations, tool calls, file scans, commands, handoffs, loops, or any workflow where the user may need to know exactly which phase or operation took the most time or got stuck.
argument-hint: "[agent file or agent design]"
---

# Instrument Generated Agent

Use this skill when creating or refactoring an agent.

## Goal

Agents created by this workbench should be observable by default without making them noticeably slower.

The user should be able to answer:

- which generated agent ran
- which phases existed
- which operations existed inside each phase
- which operation ended, skipped, failed, or got stuck
- which phase took the most time
- which operation took the most time
- what evidence supports the diagnosis

## Important distinction

This skill instruments the agents produced by the workbench.

It is not primarily for profiling the workbench itself.

## Core requirement

Use an operation tree, not random trace points.

The generated agent must define its execution as:

```text
phase -> operation -> optional sub_operation
```

Every planned operation must finish with one final state:

```text
END | SKIP | ERROR
```

Do not silently drop planned operations.

If an operation is not executed, record `SKIP` with a short reason.

If the run hangs, the likely stuck point is the last `START` without `END`, `SKIP`, or `ERROR`.

## Low-overhead rule

Profiling must be cheap.

Default approach:

1. Build the operation tree at the start or during planning.
2. Keep operation entries in run notes while working.
3. Write one compact trace summary at the end.
4. Write to `.agent-runs/<trace-id>.md` only when the agent can write files and the workflow is complex enough to justify it.
5. Do not write a file after every operation.
6. Do not log every sentence, thought, or minor internal step.

Important: low overhead does not mean skipping operations from the tree.

It means batching the trace and recording logical operations only.

## What counts as an operation

An operation is a meaningful unit the user may want to debug later, for example:

- discover files
- read selected files
- search for a pattern
- inspect one subsystem
- generate a plan
- edit one target file
- run a command
- verify a contract
- hand off to another agent
- summarize results

Do not create operations for tiny internal thoughts or wording choices.

## Section to add to generated agents

Add a section named `Profiling and trace logging` to every non-trivial generated agent.

That section should say:

- use low-overhead operation-tree profiling
- define the run as `phase -> operation -> optional sub_operation`
- every planned operation must finish as `END`, `SKIP`, or `ERROR`
- do not silently drop planned operations
- record `START` before a major operation begins
- record `END`, `SKIP`, or `ERROR` when that operation finishes
- the likely stuck operation is the last `START` without a final state
- keep trace entries in run notes while working
- write a compact trace summary at the end
- write `.agent-runs/<trace-id>.md` only for complex workflows or when the user asks for a trace file
- do not write trace files after every operation
- do not log every sentence or minor internal step

Use this trace entry format:

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

If exact timing is not available, use step order and set `elapsed_ms: unknown`.

## Required final trace summary

Generated agents should include this summary when profiling is relevant:

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

When the run has more than one phase, include a compact phase table:

```text
| Phase | Operations | Completed | Skipped | Failed | Elapsed |
|---|---:|---:|---:|---:|---:|
| discovery | 3 | 3 | 0 | 0 | unknown |
| planning | 2 | 2 | 0 | 0 | unknown |
```

## Rules

- Keep profiling lightweight.
- Do not skip planned operations from the trace.
- Do not write trace files repeatedly during the run.
- Do not log every sentence or minor internal step.
- Log logical operations inside phases.
- Do not invent exact timing.
- If timing is unknown, still report operation counts and final states.
- If there is no trace data, say so.
- Keep profiling output short unless the user asks for the full tree.
