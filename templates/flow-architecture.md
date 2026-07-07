# Agent Flow Architecture Template

Use this template when a user describes a messy workflow and expects the agent system to decide how to structure it.

## User problem

```text
<describe the workflow or pain>
```

## Failure pattern

What is going wrong now?

- [ ] too many global instructions
- [ ] agent skips verification
- [ ] agent asks for too many confirmations
- [ ] one prompt mixes planning, writing, and reviewing
- [ ] context gets polluted by logs or search results
- [ ] different steps need different tools
- [ ] user no longer trusts the result
- [ ] other: `<describe>`

## Architecture decision

| Instruction type | Artifact | Reason |
|---|---|---|
| Stable project-wide rule | `AGENTS.md` | Always relevant |
| Repeatable procedure | `skills/<name>/SKILL.md` | Load only when needed |
| Role-specific work | `.claude/agents/<name>.md` | Own context and tools |
| Output shape | `templates/<name>.md` | Reusable structure |
| Done criteria | `templates/verification-contract.md` or local contract | Evidence-based verification |
| Deterministic check | `scripts/<name>` or documented command | More reliable than prose |

## Proposed flow

```text
<agent or skill> -> <agent or skill> -> <verifier>
```

## Subagents

| Agent | Role | Tools | Why separate |
|---|---|---|---|
| `<name>` | `<role>` | `<tools>` | `<reason>` |

## Skills

| Skill | Trigger | Why skill instead of agent |
|---|---|---|
| `<name>` | `<when used>` | `<reason>` |

## Verification contract

| ID | Criterion | Required evidence |
|---|---|---|
| AC1 | `<criterion>` | `<file, command, or observation>` |
| AC2 | `<criterion>` | `<file, command, or observation>` |
| AC3 | `<criterion>` | `<file, command, or observation>` |

## Tool permission review

- Read-only agents do not have Write or Edit.
- Writer agents have only the tools needed to modify target files.
- Verifier agents can inspect files and run safe checks, but should not edit files.
- Side-effect workflows are not auto-invoked.

## Done

The architecture is acceptable when:

- each artifact has one clear responsibility
- no single agent both writes and final-approves its own work
- long procedures are not placed in global instructions
- verification criteria are explicit
- open risks are documented
