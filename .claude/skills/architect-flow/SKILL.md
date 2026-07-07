---
description: Turn a messy workflow into a small generated-agent architecture. Use when the user wants an agent, skill, prompt system, or AI workflow but does not want to manually define decomposition, subagents, verification, or profiling rules.
argument-hint: "[messy workflow]"
---

# Architect Flow

Use this skill when the workflow is messy, too large, or full of repeated rules.

## Goal

Design the generated-agent flow instead of asking the user how to split it.

## Core flow

```text
agent-architect -> agent-writer -> verifier
```

## Process

1. Identify the real failure pattern.
2. Decide the smallest useful artifact set:
   - `AGENTS.md` for stable project-wide rules
   - `.claude/agents/` for role-specific work
   - `.claude/skills/` or `skills/` for repeatable procedures
   - `templates/` for reusable output or trace shapes
   - verification contract for done criteria
3. Split instructions when one agent would mix planning, writing, review, and verification.
4. Add operation-tree profiling to non-trivial generated agents.
5. Prepare verification criteria.

## Operation-tree profiling requirement

For non-trivial generated agents, model the run as:

```text
phase -> operation -> optional sub_operation
```

Every planned operation must finish as:

```text
END | SKIP | ERROR
```

If exact timing is unavailable, still report counts and final states.

## Output

Return the shortest useful answer:

```text
Result: <one sentence>
Flow:
- <step 1> -> <step 2> -> <step 3>
Changed:
- <file or none>
Profiling:
- <operation tree added, not needed, or not run>
Verification:
- <contract, done, partial, or not run>
Next:
- <one next step or none>
```

## Rules

- Do not ask the user to define decomposition rules.
- Do not put long procedures into global instructions.
- Do not let one agent both write and final-approve its own work.
- Do not skip planned operations from generated-agent traces.
- Keep default output short and clear.
- If verification was not run, say so.
