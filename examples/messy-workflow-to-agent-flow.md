# Example: Messy Workflow to Agent Flow

Use this example when the user has a large workflow and does not want to manually define all agent rules.

## User request

```text
I have a workflow, but the instructions are getting too large.
I keep adding hard rules because agents skip checks or do not verify their work.
I do not want to tell the system every time how to split the flow.

Design the generated-agent architecture yourself.
If the instructions are too large, split the workflow into subagents or skills.
Keep global rules short.
Keep reusable logic separate from external inputs.
Use config, supporting files, specialized skills, or runtime input for values that vary by project or run.
Add a small flow diagram to non-trivial generated agents.
Add operation-tree profiling to non-trivial generated agents.
Keep output short and clear.
Make verification explicit.
```

## Expected architecture decision

```text
agent-architect -> agent-writer -> verifier
```

Optional addition after a generated agent runs slowly or gets stuck:

```text
generated agent trace -> agent-flow-profiler
```

## What the architect should decide

| Question | Decision |
|---|---|
| Is this a stable rule? | Put it in `AGENTS.md` only if it applies broadly. |
| Is this a repeated checklist? | Put it in a skill. |
| Is this a role with its own context? | Put it in `.claude/agents/<name>.md`. |
| Is this a value that varies by project or run? | Put it in config, supporting file, specialized skill, or runtime input. |
| Is this a reusable output shape? | Put it in `templates/<name>.md`. |
| Is this a done condition? | Put it in a verification contract. |
| Is the generated agent non-trivial? | Add a flow diagram and operation-tree profiling. |

## Flow diagram requirement

Non-trivial generated agents should include a small diagram showing input, phases, decisions, handoffs, verification, output, and where trace is recorded.

## Operation-tree profiling requirement

Generated agents should model their run as:

```text
phase -> operation -> optional sub_operation
```

Every planned operation must finish as:

```text
END | SKIP | ERROR
```

## Success criteria

| ID | Criterion | Required evidence |
|---|---|---|
| AC1 | The flow starts with `agent-architect` | README or example shows architect as entry point |
| AC2 | Large instructions are not placed only in global rules | Design uses skills or subagents |
| AC3 | Writer and verifier are separate | Flow includes writer and independent verifier |
| AC4 | Reusable logic is separate from external inputs | Config boundary is defined |
| AC5 | Non-trivial generated agents have diagrams | Generated agent includes Mermaid or plain text diagram |
| AC6 | Non-trivial generated agents are observable | Generated agent includes operation-tree profiling |
| AC7 | Planned operations cannot disappear silently | Generated agent uses `END`, `SKIP`, or `ERROR` final states |
| AC8 | Verification is explicit | Contract or verifier handoff exists |

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
Add config boundary, flow diagram, and operation-tree profiling when needed.
```

## Expected short response

```text
Result: Created a generated-agent flow for the workflow.
Changed:
- .claude/agents/<agent>.md
Flow:
- agent-architect -> agent-writer -> verifier
Config:
- input boundary added
Diagram:
- added
Profiling:
- operation tree added
Verification:
- Contract prepared, verifier should run next.
Next:
- Run verifier on the changed files.
```
