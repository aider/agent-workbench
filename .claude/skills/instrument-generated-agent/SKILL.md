---
description: Add built-in profiling and trace hooks to agents created by this workbench. Use whenever generating or refactoring an agent that may run multi-step work, call tools, scan files, run commands, hand off to another agent, or become slow or stuck.
argument-hint: "[agent file or agent design]"
---

# Instrument Generated Agent

Use this skill when creating or refactoring an agent.

## Goal

Agents created by this workbench should be observable by default.

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

For tiny one-step agents, a short trace summary is enough.

## Required profiling section

Add this section to generated agents:

```markdown
## Profiling and trace logging

For each major phase, record a compact trace entry.

Record `START` before the phase begins.
Record `END` when the phase completes.
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

Use one of these options:

1. If the agent can write files, write traces to `.agent-runs/<trace-id>.md`.
2. If the agent is read-only, include the trace summary in the final response.
3. If the platform provides tool timing, reference that timing as evidence.

## Output requirement for generated agents

Generated agents should include this in their output when profiling is relevant:

```text
Trace:
- trace_id: <id or none>
- slowest phase: <phase or unknown>
- stuck point: <last START without END or none>
```

## Rules

- Do not log every sentence.
- Log major phases and expensive operations only.
- Do not invent exact timing.
- If there is no trace data, say so.
- Keep profiling output short.
