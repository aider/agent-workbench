# Example: First Agent Request

Use this example when asking the workbench to create a new subagent.

## User request

```text
Create a Claude Code subagent that reviews agent files before I commit them.

Problem:
I keep adding more hard rules to global instructions. I want a separate reviewer that checks whether a new agent is focused, has a clear trigger, has the right tools, and includes a verifier handoff.

Expected output:
- create `.claude/agents/agent-reviewer.md`
- use `templates/agent-template.md`
- create a verification contract using `templates/verification-contract.md`
- then use the verifier agent to check the result
```

## Suggested workflow

1. Ask `agent-writer` to create or update the agent.
2. Ask `verifier` to check it against a contract.
3. Fix only the failed criteria.
4. Re-run verification.

## Example success criteria

| ID | Criterion | Required evidence |
|---|---|---|
| AC1 | Agent file has valid frontmatter | `.claude/agents/agent-reviewer.md` contains `name` and `description` |
| AC2 | Agent is review-only | tools do not include Write or Edit |
| AC3 | Agent has clear output | body includes an output format |
| AC4 | Agent does not approve itself | body includes verifier handoff or external evidence requirement |
