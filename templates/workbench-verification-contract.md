# Workbench Verification Contract

Use this contract to verify the agent workbench structure.

## Change

The repository should provide a minimal system for turning messy workflows into maintainable agents, adding built-in profiling to generated agents, and profiling app or service slowness before changing code.

## Required flows

```text
/architect-flow -> agent-architect -> agent-writer -> verifier
generated agent with profiling hooks -> agent-flow-profiler analyzes trace if needed
/profile-slow-flow -> performance-profiler -> verifier if files change
```

## Acceptance criteria

| ID | Criterion | Required evidence |
|---|---|---|
| AC1 | README makes `agent-architect` the main agent entry point | `README.md` mentions `agent-architect` as the main entry point |
| AC2 | README provides a short reusable architecture invocation | `README.md` shows `/architect-flow` |
| AC3 | Architect-flow project skill exists | `.claude/skills/architect-flow/SKILL.md` exists |
| AC4 | Architect decides decomposition | `.claude/agents/agent-architect.md` says it decides agents, skills, templates, and verification |
| AC5 | Writer is separate from architect | `.claude/agents/agent-writer.md` exists and writes specific artifacts |
| AC6 | Writer adds profiling to generated agents | `.claude/agents/agent-writer.md` says to add profiling hooks for non-trivial generated agents |
| AC7 | Generated-agent instrumentation skill exists | `.claude/skills/instrument-generated-agent/SKILL.md` exists |
| AC8 | Generated-agent trace skill exists | `.claude/skills/agent-flow-trace/SKILL.md` exists |
| AC9 | Agent-flow profiler exists | `.claude/agents/agent-flow-profiler.md` exists |
| AC10 | Trace template explains START/END hang diagnosis | `templates/agent-run-trace.md` explains last START without END |
| AC11 | Generated-agent profiling example exists | `examples/generated-agent-profiling.md` exists |
| AC12 | Verifier is separate from writer | `.claude/agents/verifier.md` exists and is verification-focused |
| AC13 | Short output is a default rule | `AGENTS.md`, `agent-architect`, or templates mention short clear output |
| AC14 | Large workflows have an example | `examples/messy-workflow-to-agent-flow.md` exists |
| AC15 | Verification is explicit | `templates/verification-contract.md` and `skills/verify-change/SKILL.md` exist |
| AC16 | App or service slowness has a profiling flow | `.claude/skills/profile-slow-flow/SKILL.md` and `.claude/agents/performance-profiler.md` exist |

## Checks to run

```bash
find . -maxdepth 5 -type f | sort
grep -R "instrument-generated-agent\|agent-flow-trace\|agent-flow-profiler" README.md .claude/agents .claude/skills examples templates
grep -R "last START\|START.*END\|without.*END" README.md .claude/agents .claude/skills examples templates
grep -R "short" README.md AGENTS.md .claude/agents .claude/skills templates examples
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
