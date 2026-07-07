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
| AC2 | README layout includes all core examples and templates | README lists config, diagram, profiling, messy-flow, first-request, and hang-debug examples |
| AC3 | AGENTS includes the core architecture standards | `AGENTS.md` mentions external input boundary, flow diagrams, and operation-tree profiling |
| AC4 | Architect owns decomposition | `agent-architect.md` says the user does not provide decomposition rules |
| AC5 | Architect owns external input boundaries | `agent-architect.md` defines external input boundary decisions |
| AC6 | Architect owns flow diagrams | `agent-architect.md` requires a small flow diagram for non-trivial generated agents |
| AC7 | Writer creates specific artifacts after architecture is known | `agent-writer.md` says to hand off to architect when architecture is unclear |
| AC8 | Writer keeps reusable logic separate from external inputs | `agent-writer.md` includes an external input boundary |
| AC9 | Writer adds flow diagrams to non-trivial generated agents | `agent-writer.md` includes flow diagram rules |
| AC10 | Writer adds operation-tree profiling to generated agents | `agent-writer.md` includes generated-agent profiling rules |
| AC11 | Generated-agent instrumentation skill exists | `instrument-generated-agent/SKILL.md` defines operation-tree profiling |
| AC12 | Agent-flow profiler analyzes generated-agent traces | `agent-flow-profiler.md` analyzes operation-tree traces |
| AC13 | Every planned operation has a final-state rule | relevant files say planned operations must end as `END`, `SKIP`, or `ERROR` |
| AC14 | Trace template exists and is generic | `templates/agent-run-trace.md` uses operation-tree trace format without project-specific examples |
| AC15 | Config template exists | `templates/generated-agent-config.md` exists for values supplied outside reusable agents |
| AC16 | Flow diagram template exists | `templates/generated-agent-flow-diagram.md` exists |
| AC17 | Verifier checks the new architecture | `verifier.md` checks external input boundary, flow diagram, and operation-tree profiling |
| AC18 | Examples follow the current architecture | examples start from architect and include config boundary, diagram, profiling, and verification where relevant |
| AC19 | Output remains short | core agents or templates require short output |

## Manual checks

Check these files:

```text
README.md
AGENTS.md
.claude/agents/agent-architect.md
.claude/agents/agent-writer.md
.claude/agents/agent-flow-profiler.md
.claude/agents/verifier.md
.claude/skills/architect-flow/SKILL.md
.claude/skills/instrument-generated-agent/SKILL.md
skills/verify-change/SKILL.md
templates/agent-template.md
templates/agent-run-trace.md
templates/generated-agent-config.md
templates/generated-agent-flow-diagram.md
templates/flow-architecture.md
templates/verification-contract.md
examples/agent-flow-hang-debug.md
examples/first-agent-request.md
examples/generated-agent-profiling.md
examples/messy-workflow-to-agent-flow.md
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
