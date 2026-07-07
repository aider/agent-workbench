# Example: Built-in Profiling for Generated Agents

Use this example when the workbench creates agents and those agents may later be slow or stuck.

## Problem

```text
I use the workbench to create different agents.
One generated agent has its own flow.
When I run that generated agent, it sometimes works slowly or appears stuck.
I need to know which phase or operation inside that generated agent caused the delay.
```

## Required behavior

When `agent-architect` or `agent-writer` creates a non-trivial agent, it should add built-in profiling hooks.

The profiling belongs inside the generated agent.

## Generated agent must include

```markdown
## Profiling and trace logging

Record `START` before each major phase.
Record `END` after each major phase.
If a run hangs, the last `START` without `END` is the likely stuck point.
```

## Example generated trace

```text
trace_id: run-101
event: START
agent: project-investigator
phase: discovery
operation: scan project files
time: step-01
elapsed_ms: unknown
evidence: Glob src/**/*.java
status: running

trace_id: run-101
event: END
agent: project-investigator
phase: discovery
operation: scan project files
time: step-02
elapsed_ms: unknown
evidence: found 42 files
status: success

trace_id: run-101
event: START
agent: project-investigator
phase: analysis
operation: inspect service dependencies
time: step-03
elapsed_ms: unknown
evidence: Grep Feign|RestTemplate|WebClient
status: running
```

## Diagnosis

Because the last `START` has no matching `END`, the likely stuck point is:

```text
project-investigator / analysis / inspect service dependencies
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
Result: Likely stuck at project-investigator/analysis/inspect service dependencies.
Evidence:
- Last START has no matching END.
Next:
- Split dependency inspection into smaller searches or limit file scope.
```
