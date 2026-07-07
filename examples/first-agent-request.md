# Example: First Agent Request

Use this example when asking the workbench to create a new subagent.

## User request

```text
Use agent-architect.

Create a Claude Code subagent that reviews generated agent files before I commit them.

Problem:
I want a separate reviewer that checks whether a generated agent is focused, has a clear trigger, has the right tools, has a config boundary, has a small diagram when needed, has operation-tree profiling when needed, and includes a verifier handoff.

Expected output:
- design the smallest generated-agent architecture
- create `.claude/agents/agent-reviewer.md` if needed
- use `templates/agent-template.md`
- create a verification contract
- use the verifier agent to check the result
```

## Suggested workflow

```text
agent-architect -> agent-writer -> verifier
```

## Example success criteria

| ID | Criterion | Required evidence |
|---|---|---|
| AC1 | Agent file has valid frontmatter | file contains `name` and `description` |
| AC2 | Agent is review-only | tools do not include Write or Edit |
| AC3 | Agent has clear output | body includes an output format |
| AC4 | Agent does not approve itself | body includes verifier handoff |
| AC5 | Agent has a config boundary | body says external values come from config or runtime input |
| AC6 | Non-trivial flow has a diagram | body includes Mermaid or plain text diagram, or explains why it is not needed |
| AC7 | Non-trivial flow has operation-tree profiling | body includes `phase -> operation -> optional sub_operation` and final states `END`, `SKIP`, or `ERROR` |
