# Agent Operating Rules

These rules apply to this repository.

## Purpose

This repo is for designing and reviewing generated agents, skills, templates, and verification contracts.

The main goal is to reduce instruction overload by moving repeated workflows into focused artifacts, reviewing existing agent flows before rewriting them, keeping reusable agent logic separate from external inputs, showing non-trivial generated-agent flows with small diagrams, and making non-trivial generated agents observable through operation-tree profiling.

## Core flow

```text
/architect-flow -> agent-architect -> agent-writer -> verifier
```

For a slow or stuck generated agent:

```text
generated agent trace -> agent-flow-profiler
```

For an existing external agent system that needs review:

```text
existing agent system -> agent-flow-reviewer -> evidence-based improvement plan
```

For a failed, interrupted, or incorrect previous generated-agent run:

```text
visible chat context + repo evidence -> agent-run-fixer -> smallest correct fix
```

## How to work in this repo

1. Understand the requested workflow before editing files.
2. Put persistent project guidance in `AGENTS.md` only when it applies broadly.
3. Put repeatable procedures in skills.
4. Put reusable structure in `templates/`.
5. Put Claude Code project subagents in `.claude/agents/`.
6. Prefer small, composable agents over one large agent.
7. Keep reusable agent logic separate from external inputs.
8. Add a flow diagram to non-trivial generated agents.
9. Add operation-tree profiling to non-trivial generated agents.
10. Review existing external agent flows before proposing rewrites.
11. Route "Fix this" style generated-agent run failures to the fixer workflow.

## Instruction maintenance

When changing instructions, first search for existing related rules.

Prefer updating or merging existing rules over adding new overlapping rules.

Add a new rule only when no existing rule covers the behavior.

Remove duplicate wording when possible.

Final responses should say whether the change updated an existing rule or added a new rule.

## External input boundary

Generated agents should contain reusable behavior.

Values that vary by project or run should stay outside reusable agent instructions.

Keep these inside generated agents:

- role
- workflow logic
- decision rules
- tool boundaries
- input contract
- output format
- verification handoff
- support-script contract
- flow diagram shape
- operation-tree profiling structure

Keep these outside generated agents:

- variable values
- lookup terms
- file locations
- command strings
- labels
- runtime settings
- reference records

Use config files, supporting files, specialized skills, or runtime input for those values.

## Existing agent flow review

When the user asks to review an existing external agent system, agent flow, or another project, use `.claude/agents/agent-flow-reviewer.md`.

The user does not need to mention `REVIEW.md`.

Default mode is review-only.

Do not modify the target project unless the user explicitly asks for changes, fixes, refactor, rewrite, or implementation.

A review means:

1. inventory files first
2. reconstruct actual flow
3. check workbench standards
4. produce an evidence-based improvement plan

Do not say "looks good" without evidence.

## Previous agent run fixer

When the user says "Fix this", "Fix the previous agent run", or "Fix what went wrong" after a generated-agent run, use `.claude/agents/agent-run-fixer.md`.

The fixer should use visible chat context when available, but must not assume chat context is complete.

Do not try to read VS Code internal chat history files unless official documentation says that is supported.

The fixer must verify against repository evidence:

- `git status`
- `git diff`
- changed files
- `.claude/agents/*.md`
- `.claude/skills/*/SKILL.md`
- `.agent-runs/*.md`
- failure handoff blocks
- command, script, or test output visible in files or chat

The fixer should identify the previous agent/run, classify the issue, and fix the smallest correct thing.

Ask the user only when multiple plausible fixes could damage the wrong files.

## Support script standard

Support scripts are prepared tools for predictable deterministic actions in generated-agent flows.

Use a support script when the action should already exist before the generated agent runs and the generated agent would otherwise write ad hoc helper code during execution.

Do not add scripts just because an agent exists.

Preferred locations:

- one generated agent: `.claude/agents/<agent-name>.scripts/<script>.py`
- one skill: `.claude/skills/<skill-name>/scripts/<script>.py`
- shared by multiple generated agents: `tools/<tool-name>.py`
- installable command-line tooling: `pyproject.toml`, `src/<package_name>/<module>.py`, and `README.md`

Only add Python packaging when command-line scripts are actually needed.

## Definition of done

A change is not done until the assistant can answer these questions:

- What changed?
- Why was it changed?
- Which files changed?
- What verification was performed?
- What evidence supports the result?
- What was not verified?

## Verification standard

When verifying a change:

- Check the actual files, not only the intended design.
- Compare the result against a contract or acceptance criteria.
- Run available checks when they are relevant.
- For documentation-only changes, verify structure, links, and consistency.
- For code changes, run the narrowest meaningful test first, then broader tests if needed.
- If a command cannot be run, report the blocker and do not mark that item verified.

## Agent design standard

Every generated agent should have:

- a specific job
- a clear `description` trigger
- the smallest practical tool set
- explicit boundaries
- an output format
- a verification or handoff step
- an external input boundary
- a flow diagram when the agent is non-trivial
- operation-tree profiling when the agent is non-trivial

Avoid vague agents like "do everything" or "senior expert" without a measurable task.

## Flow diagram standard

A non-trivial generated agent should have a small diagram when it has more than one meaningful phase, branch, handoff, verification step, or operation-tree trace.

The diagram should show input, phases, decisions, handoffs, verification, output, and where trace is recorded.

Use generic phase and operation names. Do not put external values into the diagram.

## Operation-tree profiling standard

A non-trivial generated agent is one that has phases, operations, tool calls, file scans, commands, handoffs, loops, retries, or verification.

It should model work as:

```text
phase -> operation -> optional sub_operation
```

Every planned operation must finish as one of:

```text
END | SKIP | ERROR
```

Rules:

- Do not silently drop planned operations.
- Use `SKIP` with a short reason when an operation is not executed.
- The likely stuck point is the last `START` without `END`, `SKIP`, or `ERROR`.
- Keep profiling low-overhead by batching trace entries and writing a compact summary at the end.
- Do not log every sentence or minor internal step.

## Output style standard

Default output should be simple, short, and clear.

Prefer this shape:

```text
Result: <one sentence>
Changed:
- <file or decision>
Why:
- <one short reason>
Next:
- <one next step or none>
```

Rules:

- Start with the result.
- Use short bullets.
- Avoid long background explanations.
- Do not repeat the full reasoning unless the user asks.
- Keep details in files, contracts, or appendices, not in the main response.
- Include evidence only when verification or trust is important.

## Writing style

Be direct, calm, and useful. Do not hide uncertainty. Do not produce long explanations when a short answer is enough.

When criticism is needed, make it actionable:

- weak: This is bad.
- better: This mixes planning, implementation, and verification in one agent. Split verification into a separate read-only agent so the writer cannot silently approve its own work.

## Escalation rule

Ask a question only when the missing detail blocks meaningful progress. Otherwise, make a reasonable assumption, state it, and continue.
