# Workbench Verification Contract

Use this contract to verify the core workbench.

## Change

The repository should provide a small system for turning messy workflows into reusable generated agents with explicit verification, external input boundaries, flow diagrams, and operation-tree profiling.

## Required core flow

```text
/architect-flow -> agent-architect -> agent-writer -> verifier
```

For a slow or stuck generated agent:

```text
generated agent trace -> agent-flow-profiler
```

## Acceptance criteria

| ID | Criterion | Required evidence |
|---|---|---|
| AC1 | README describes the small core flow | `README.md` shows the required core flow |
| AC2 | README layout includes config and diagram templates | README lists `generated-agent-config.md` and `generated-agent-flow-diagram.md` |
| AC3 | Architect owns decomposition | `agent-architect.md` says the user does not provide decomposition rules |
| AC4 | Architect owns flow diagrams | `agent-architect.md` requires a small flow diagram for non-trivial generated agents |
| AC5 | Writer creates specific artifacts after architecture is known | `agent-writer.md` says to hand off to architect when architecture is unclear |
| AC6 | Writer keeps reusable logic separate from external inputs | `agent-writer.md` includes an external input boundary |
| AC7 | Writer adds flow diagrams to non-trivial generated agents | `agent-writer.md` includes flow diagram rules |
| AC8 | Writer adds operation-tree profiling to generated agents | `agent-writer.md` includes generated-agent profiling rules |
| AC9 | Generated-agent instrumentation skill exists | `instrument-generated-agent/SKILL.md` defines operation-tree profiling |
| AC10 | Agent-flow profiler analyzes generated-agent traces | `agent-flow-profiler.md` analyzes operation-tree traces |
| AC11 | Every planned operation has a final-state rule | relevant files say planned operations must end as `END`, `SKIP`, or `ERROR` |
| AC12 | Trace template exists | `templates/agent-run-trace.md` uses operation-tree trace format |
| AC13 | Config template exists | `templates/generated-agent-config.md` exists for values supplied outside reusable agents |
| AC14 | Flow diagram template exists | `templates/generated-agent-flow-diagram.md` exists |
| AC15 | Verifier checks the new architecture | `verifier.md` checks external input boundary, flow diagram, and operation-tree profiling |
| AC16 | Output remains short | core agents or templates require short output |

## Manual checks

Check these files:

```text
README.md
.claude/agents/agent-architect.md
.claude/agents/agent-writer.md
.claude/agents/agent-flow-profiler.md
.claude/agents/verifier.md
.claude/skills/architect-flow/SKILL.md
.claude/skills/instrument-generated-agent/SKILL.md
templates/agent-template.md
templates/agent-run-trace.md
templates/generated-agent-config.md
templates/generated-agent-flow-diagram.md
templates/verification-contract.md
```

## Expected verifier result

```text
Result: PASS | FAIL | PARTIAL
Evidence:
- <short evidence for each criterion>
Not verified:
- <item or none>
Next:
- <one next step or none>
```
