---
description: Moved out of the core agent-workbench MVP. This file is retained only as a note that app, service, API, build, infra, and database performance profiling are outside the current generated-agent architecture scope.
argument-hint: "[archived]"
---

# Moved out of MVP

The core workbench flow is focused on generated-agent architecture.

Core flow:

```text
/architect-flow -> agent-architect -> agent-writer -> verifier
```

Generated agents can include operation-tree profiling through `instrument-generated-agent`.
