---
name: verify-change
description: Verify a completed change against an explicit contract and report evidence. Use when the user asks to check, verify, validate, review, or confirm that an agent, skill, prompt, documentation, or code change is complete.
argument-hint: "[contract or files]"
---

# Verify Change

Use this skill to verify a change before calling it done.

## Goal

Check the actual repository state against a clear contract and report evidence.

## Inputs

Use `$ARGUMENTS` as the verification target. It may contain:

- a file path
- a list of changed files
- a contract
- a user request
- a commit or branch reference
- configured check commands

If `$ARGUMENTS` is empty, inspect the current repository state and infer the verification target from changed files.

## Verification workflow

1. Identify the contract.
   - If a contract file is provided, use it.
   - If not, infer acceptance criteria from the user request and changed files.
   - Mark inferred criteria as inferred.
2. Inspect the actual changed files.
3. Compare each criterion with evidence.
4. Run relevant checks when practical.
5. Return `PASS`, `FAIL`, or `PARTIAL`.

## Check selection

Run only checks that are relevant to the repository and safe for the current task.

Prefer checks from:

- the verification contract
- repository documentation
- configured check commands
- package or build files found in the repository
- user-provided runtime input

For documentation-only changes, prefer structural checks and file inspection.

If no meaningful automated check is available, report that and verify by file inspection.

## Generic discovery commands

Use only when relevant:

```bash
git status --short
git diff --stat HEAD
git diff HEAD
find . -maxdepth 4 -type f | sort
```

## Agent and skill checks

For `.claude/agents/*.md`:

- YAML frontmatter exists.
- `name` is present.
- `description` is present and specific.
- tools match the role.
- write-capable agents have a reason to write.
- verifier-style agents do not edit files.
- output format is clear.
- non-trivial generated agents have an external input boundary.
- non-trivial generated agents have a flow diagram or explain why one is not needed.
- non-trivial generated agents have operation-tree profiling.

For `SKILL.md`:

- frontmatter exists.
- `description` explains when to use it.
- body is concise.
- side effects are explicit.
- referenced files exist.

## Output

Use this exact structure:

```text
Result: PASS | FAIL | PARTIAL

Verified target:
- <target>

Contract source:
- <explicit or inferred>

Evidence:
| Criterion | Result | Evidence |
|---|---|---|
| <criterion> | PASS/FAIL/SKIPPED | <file, line, command, or observation> |

Commands run:
- <command> -> <result>

Not verified:
- <item or none>

Recommended fixes:
- <fix or none>
```

## Rules

- Do not mark a criterion as passed without evidence.
- Do not hide skipped checks.
- Do not change files while using this skill unless the user explicitly asks for fixes.
- If evidence is incomplete, use `PARTIAL`.
- Do not assume technology-specific commands unless they are present in contract, docs, config, or repository files.
