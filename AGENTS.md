# Agent Operating Rules

These rules apply to this repository.

## Purpose

This repo is for designing agents, skills, and verification contracts. The main goal is to reduce instruction overload by moving repeated workflows into focused files.

## How to work in this repo

1. Understand the requested workflow before editing files.
2. Put persistent project guidance in `AGENTS.md` only when it applies broadly.
3. Put repeatable procedures in `skills/`.
4. Put reusable structure in `templates/`.
5. Put Claude Code project subagents in `.claude/agents/`.
6. Prefer small, composable agents over one large agent.

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

Every agent should have:

- a specific job
- a clear `description` trigger
- the smallest practical tool set
- explicit boundaries
- an output format
- a verification or handoff step

Avoid vague agents like "do everything" or "senior expert" without a measurable task.

## Writing style

Be direct, calm, and useful. Do not hide uncertainty. Do not produce long explanations when a short answer is enough.

When criticism is needed, make it actionable:

- weak: This is bad.
- better: This mixes planning, implementation, and verification in one agent. Split verification into a separate read-only agent so the writer cannot silently approve its own work.

## Escalation rule

Ask a question only when the missing detail blocks meaningful progress. Otherwise, make a reasonable assumption, state it, and continue.
