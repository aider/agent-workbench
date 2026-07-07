---
description: Turn a messy workflow into a small generated-agent architecture. Use when the user wants an agent, skill, prompt system, or AI workflow but does not want to manually define decomposition, subagents, external input boundaries, support scripts, flow diagrams, verification, or profiling rules.
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
   - `templates/` for reusable output, config, diagram, or trace shapes
   - verification contract for done criteria
3. Split instructions when one agent would mix planning, writing, review, and verification.
4. Define the external input boundary for values that vary by project or run.
5. Decide whether predictable deterministic actions need prepared support scripts.
6. Add a small flow diagram to non-trivial generated agents.
7. Add operation-tree profiling to non-trivial generated agents.
8. Prepare verification criteria.

## External input boundary

Reusable generated agents should contain behavior.

Values that vary by project or run should come from config, supporting files, specialized skills, or runtime input.

## Support script requirement

Support scripts are prepared tools for deterministic generated-agent actions that should already exist before the normal flow runs.

Add them only when the generated agent would otherwise write ad hoc helper code during execution.

Do not add scripts just because an agent exists.

## Flow diagram requirement

For non-trivial generated agents, create a small diagram showing:

- input
- phases
- decisions
- handoffs
- verification
- output
- where operation-tree trace is recorded

The architect owns the diagram design. The writer implements it.

## Operation-tree profiling requirement

For non-trivial generated agents, model the run as:

```text
phase -> operation -> optional sub_operation
```

Every planned operation must finish as:

```text
END | SKIP | ERROR
```

If an operation is skipped, record `SKIP` with a short reason.

If a run gets stuck, the likely stuck point is the last `START` without `END`, `SKIP`, or `ERROR`.

If exact timing is unavailable, still report counts and final states.

Keep profiling low-overhead: keep trace entries in run notes, write one compact summary at the end, write trace files only for complex write-capable agents, and do not log every sentence or minor internal step.

## Output

Return the shortest useful answer:

```text
Result: <one sentence>
Flow:
- <step 1> -> <step 2> -> <step 3>
Changed:
- <file or none>
Config:
- <input boundary added, not needed, or not run>
Scripts:
- <support scripts added, not needed, or not run>
Diagram:
- <added, not needed, or not run>
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
- Do not embed values that vary by project or run in reusable generated agents.
- Do not skip planned operations from generated-agent traces.
- Keep default output short and clear.
- If verification was not run, say so.
