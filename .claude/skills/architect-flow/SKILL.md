---
description: Turn a messy workflow into a small agent architecture. Use when the user wants an agent, skill, prompt system, or automation but does not want to manually define all rules, decomposition, subagents, or verification steps.
argument-hint: "[messy workflow]"
---

# Architect Flow

Use this skill when the workflow is messy, too large, or full of repeated rules.

## Goal

Design the agent flow instead of asking the user how to split it.

## Input

Use `$ARGUMENTS` as the workflow.

If the workflow is missing, ask for it in one short sentence.

## Process

1. Identify the real failure pattern.
2. Decide what belongs in each artifact:
   - `AGENTS.md` for stable project-wide rules
   - `.claude/agents/` for role-specific work
   - `.claude/skills/` or `skills/` for repeatable procedures
   - `templates/` for reusable output shapes
   - verification contract for done criteria
   - script or command for deterministic checks
3. Split instructions when one agent would mix planning, writing, and verification.
4. Prefer this default flow:

```text
agent-architect -> agent-writer -> verifier
```

5. Create or update files when requested.
6. Prepare verification criteria.

## Output

Return the shortest useful answer:

```text
Result: <one sentence>
Flow:
- <step 1> -> <step 2> -> <step 3>
Changed:
- <file or none>
Verification:
- <contract, done, partial, or not run>
Next:
- <one next step or none>
```

## Rules

- Do not ask the user to define decomposition rules.
- Do not put long procedures into global instructions.
- Do not let one agent both write and final-approve its own work.
- Keep default output short and clear.
- If verification was not run, say so.
