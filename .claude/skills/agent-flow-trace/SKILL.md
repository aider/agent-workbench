---
description: Compatibility note. Generated-agent profiling is now handled by instrument-generated-agent, which defines operation-tree profiling with final states for every planned operation.
argument-hint: "[generated agent file or design]"
---

# Compatibility note

Use `instrument-generated-agent` for the current generated-agent profiling model.

Current model:

```text
phase -> operation -> optional sub_operation
```

Every planned operation must finish as:

```text
END | SKIP | ERROR
```

The analyzer for completed traces is `agent-flow-profiler`.
