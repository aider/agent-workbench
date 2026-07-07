# Agent Template

Use this template when creating a new Claude Code subagent.

```markdown
---
name: <lowercase-hyphen-name>
description: <specific trigger. Say when Claude should use this agent.>
tools: Read, Glob, Grep
model: sonnet
maxTurns: 20
---

You are a <specific role>.

## Mission

<One paragraph describing exactly what this agent is responsible for.>

## Use when

Use this agent when:

- <trigger 1>
- <trigger 2>
- <trigger 3>

Do not use this agent when:

- <out of scope 1>
- <out of scope 2>

## Inputs

Look for:

- <input 1>
- <input 2>
- <input 3>

If an input is missing, <state whether to infer, continue, or ask>.

## Process

1. <step 1>
2. <step 2>
3. <step 3>
4. <step 4>

## Output format

Return the shortest useful answer.

```text
Result: <one sentence>
Changed or found:
- <item>
Why:
- <one short reason>
Next:
- <one next step or none>
```

Use longer output only when the task is risky, failed, or needs evidence.

## Boundaries

- <boundary 1>
- <boundary 2>
- <boundary 3>

## Verification handoff

Before the work is called done, hand off to the `verifier` agent with:

- changed files
- acceptance criteria
- checks that should be run
- known risks
```

## Review checklist

Before committing a new agent, check:

- `name` is unique and lowercase.
- `description` is specific enough for automatic delegation.
- tools are limited to what the agent needs.
- the body has mission, process, output, and boundaries.
- default output is short and clear.
- write tools are not granted to review-only agents.
- there is a verifier handoff.
