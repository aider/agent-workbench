# Example: Messy Workflow to Agent Flow

Use this example when the user has a large workflow and does not want to manually define all agent rules.

## User request

```text
I have a workflow, but the instructions are getting too large.
I keep adding hard rules because agents skip checks or do not verify their work.
I do not want to tell the system every time how to split the flow.

Design the agent architecture yourself.
If the instructions are too large, split the workflow into subagents or skills.
Keep global rules short.
Keep output short and clear.
Make sure verification is explicit.
```

## Expected architecture decision

```text
agent-architect -> agent-writer -> verifier
```

Optional additions:

```text
research-agent -> agent-architect -> agent-writer -> verifier
```

Use the optional research step only when the workflow needs a lot of reading, logs, docs, or search results.

## What the architect should decide

| Question | Decision |
|---|---|
| Is this a stable rule? | Put it in `AGENTS.md` only if it applies broadly. |
| Is this a repeated checklist? | Put it in `skills/<name>/SKILL.md`. |
| Is this a role with its own context? | Put it in `.claude/agents/<name>.md`. |
| Is this a reusable output shape? | Put it in `templates/<name>.md`. |
| Is this a done condition? | Put it in a verification contract. |
| Is this deterministic? | Prefer a script or command. |

## Success criteria

| ID | Criterion | Required evidence |
|---|---|---|
| AC1 | The flow starts with `agent-architect` | README or example shows architect as entry point |
| AC2 | Large instructions are not placed only in global rules | Design uses skills or subagents |
| AC3 | Writer and verifier are separate | Flow includes writer and independent verifier |
| AC4 | Output is short by default | Template or agent instruction says to return short output |
| AC5 | Verification is explicit | Contract or verifier handoff exists |

## Example prompt to run

```text
Use agent-architect.

Here is my messy workflow:
<paste workflow>

Do not ask me how to split it.
Decide the architecture yourself.
Create or update the needed agents, skills, templates, and contracts.
Keep output short.
Prepare verification criteria.
```

## Expected short response

```text
Result: Created a split agent flow for the workflow.
Changed:
- .claude/agents/<agent>.md
- skills/<skill>/SKILL.md
Flow:
- agent-architect -> agent-writer -> verifier
Verification:
- Contract prepared, verifier should run next.
Next:
- Run verifier on the changed files.
```
