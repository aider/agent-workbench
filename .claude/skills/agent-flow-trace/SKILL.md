---
description: Add phase-level tracing to an agent workflow so slow agents, phases, and operations can be diagnosed later. Use when creating or refactoring agents, when the user wants to profile agent execution, or when agent workflows are slow and need observable phases.
argument-hint: "[agent workflow or files]"
---

# Agent Flow Trace

Use this skill to make agent workflows observable.

## Goal

Every important agent workflow should expose enough phase-level trace data to answer:

- which agent ran
- which phase ran
- which operation was slow
- what evidence supports that finding
- what was skipped or repeated

## Important limitation

Claude cannot automatically intercept every internal model step.

So this skill adds explicit phase logging at workflow boundaries and around expensive operations.

## What to instrument

Add trace points around:

- architecture decision
- file discovery
- broad search
- reading many files
- writing files
- running tests or checks
- verification
- repeated loops
- handoff to another agent

## Trace format

Use this simple format in run notes or `.agent-runs/<trace-id>.md`:

```text
trace_id: <id>
agent: <agent-name>
phase: <phase-name>
operation: <operation-name>
start: <timestamp or step number>
end: <timestamp or step number>
elapsed_ms: <number or unknown>
evidence: <file, command, or observation>
status: success | failed | skipped
```

## Agent instruction block

When adding tracing to an agent, include this section:

```markdown
## Trace logging

For each major phase, record a short trace entry:

- agent
- phase
- operation
- evidence
- status
- elapsed time if available

Log before and after expensive operations such as broad search, full repo scan, file writes, tests, and verification.

If exact time is not available, record step order and mark elapsed time as unknown.
```

## Flow

```text
agent with trace logging -> agent-flow-profiler -> suggested improvement
```

## Output

Keep it short:

```text
Result: <tracing added or needed>
Trace points:
- <phase or operation>
Next:
- <how to run or inspect trace>
```

## Rules

- Do not create noisy logs for every sentence.
- Log workflow phases and expensive operations only.
- Do not claim exact timing when only step order is available.
- Prefer a small trace that can actually be used.
