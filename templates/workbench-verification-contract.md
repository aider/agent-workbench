# Workbench Verification Contract

Use this contract to verify the agent workbench structure.

## Change

The repository should provide a minimal system for turning messy workflows into maintainable agent flows.

## Required flow

```text
/architect-flow -> agent-architect -> agent-writer -> verifier
```

## Acceptance criteria

| ID | Criterion | Required evidence |
|---|---|---|
| AC1 | README makes `agent-architect` the main agent entry point | `README.md` mentions `agent-architect` as the main entry point |
| AC2 | README provides a short reusable invocation | `README.md` shows `/architect-flow` |
| AC3 | Project skill exists | `.claude/skills/architect-flow/SKILL.md` exists |
| AC4 | Architect decides decomposition | `.claude/agents/agent-architect.md` says it decides agents, skills, templates, and verification |
| AC5 | Writer is separate from architect | `.claude/agents/agent-writer.md` exists and writes specific artifacts |
| AC6 | Verifier is separate from writer | `.claude/agents/verifier.md` exists and is verification-focused |
| AC7 | Short output is a default rule | `AGENTS.md`, `agent-architect`, or templates mention short clear output |
| AC8 | Large workflows have an example | `examples/messy-workflow-to-agent-flow.md` exists |
| AC9 | Verification is explicit | `templates/verification-contract.md` and `skills/verify-change/SKILL.md` exist |

## Checks to run

```bash
find . -maxdepth 5 -type f | sort
grep -R "agent-architect" README.md .claude/agents .claude/skills examples templates
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
