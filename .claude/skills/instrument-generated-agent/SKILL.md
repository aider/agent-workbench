---
description: Add low-overhead built-in profiling and trace hooks to agents created by this workbench. Use whenever generating or refactoring an agent that may run multi-step work, call tools, scan files, run commands, hand off to another agent, or become slow or stuck.
argument-hint: "[agent file or agent design]"
---

# Instrument Generated Agent

Use this skill when creating or refactoring an agent.

## Goal

Agents created by this workbench should be observable by default without making them noticeably slower.

The user should be able to answer:

- which generated agent ran
- which phase started
- which phase ended
- which operation was slow
- which operation appears stuck
- what evidence supports that conclusion

## Important distinction

This skill instruments the agents produced by the workbench.

It is not primarily for profiling the workbench itself.

## Low-overhead rule

Profiling must be cheap.

Default approach:

1. Keep trace entries in the agent's run notes while working.
2. Write one compact trace summary at the end.
3. Write to `.agent-runs/<trace-id>.md` only when the agent can write files and the workflow is complex enough to justify it.
4. Do not write a file after every operation.
5. Do not log every sentence, thought, or minor tool call.

Target size:

- normal run: 3 to 8 trace entries
- complex run: 8 to 20 trace entries
- avoid more unless the user asks for deep profiling

## When to add profiling

Add profiling to any generated agent that:

- has more than one phase
- reads many files
- runs broad search
- writes files
- runs commands
- performs verification
- delegates to another agent
- can loop or retry
- may be used in a larger workflow

For tiny one-step agents, only add a final trace summary or skip profiling if it adds no value.

## Required profiling section

Add this section to generated agents:

```markdown
## Profiling and trace logging

Use low-overhead phase tracing.

Record only major phases and expensive operations.
Do not log every sentence or minor tool call.
Keep trace entries in run notes while working.
Write a compact trace summary at the end.
Write `.agent-runs/<trace-id>.md` only for complex workflows or when the user asks for a trace file.

Record `START` before a major phase begins.
Record `END` when that phase completes.
Record `ERROR` if the phase fails.
Record `SKIP` if the phase is intentionally skipped.

If the run becomes slow or appears stuck, the last `START` without a matching `END` is the likely stuck operation.

Trace major phases only:

- discovery
- planning
- broad search
- file read
- file write
- command run
- verification
- handoff

Trace format:

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

If exact timing is not available, use step order and set `elapsed_ms: unknown`.
```

## Where the trace goes

Use the lowest-overhead option that still supports diagnosis:

1. For normal runs, include a compact trace summary in the final response.
2. For complex write-capable agents, write one trace file at `.agent-runs/<trace-id>.md` near the end of the run.
3. If the platform provides tool timing, reference that timing as evidence instead of duplicating it.
4. If tracing would add more overhead than value, record `trace: skipped` and explain why in one line.

## Output requirement for generated agents

Generated agents should include this in their output when profiling is relevant:

```text
Trace:
- trace_id: <id or none>
- entries: <count or none>
- slowest phase: <phase or unknown>
- stuck point: <last START without END or none>
```

## Rules

- Keep profiling lightweight.
- Do not log every sentence.
- Do not write trace files repeatedly during the run.
- Log major phases and expensive operations only.
- Do not invent exact timing.
- If there is no trace data, say so.
- Keep profiling output short.
