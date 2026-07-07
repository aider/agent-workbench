# Workbench Verification Contract

Use this contract to verify the refactored core workbench.

## Change

The repository should provide a small system for turning messy workflows into generated agents with explicit verification and operation-tree profiling.

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
| AC2 | README core layout stays focused on generated-agent design | README layout lists architect, writer, verifier, generated-agent profiler, and instrumentation skill |
| AC3 | Architect owns decomposition | `agent-architect.md` says the user does not provide decomposition rules |
| AC4 | Architect requires operation-tree profiling for non-trivial generated agents | `agent-architect.md` describes `phase -> operation -> optional sub_operation` |
| AC5 | Writer creates specific artifacts after architecture is known | `agent-writer.md` says to hand off to architect when architecture is unclear |
| AC6 | Writer adds operation-tree profiling to generated agents | `agent-writer.md` includes generated-agent profiling rules |
| AC7 | Generated-agent instrumentation skill exists | `instrument-generated-agent/SKILL.md` defines operation-tree profiling |
| AC8 | Agent-flow profiler analyzes generated-agent traces | `agent-flow-profiler.md` analyzes operation-tree traces |
| AC9 | Every planned operation has a final-state rule | relevant files say planned operations must end as `END`, `SKIP`, or `ERROR` |
| AC10 | Trace template exists | `templates/agent-run-trace.md` uses operation-tree trace format |
| AC11 | Verifier is separate from writer | `verifier.md` is verification-focused |
| AC12 | Output remains short | core agents or templates require short output |

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
