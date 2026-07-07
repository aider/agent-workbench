# Generated Agent Support Scripts Template

Use this template when a generated agent needs deterministic checks or repeatable local tooling.

## Purpose

Generated agents should use scripts for work that should not depend on prose instructions.

Examples:

- repository tree audit
- config validation
- trace validation
- diagram validation
- no-hardcoded-values check
- operation-tree final-state check
- generated-agent verification

This template is for agents created by the workbench.

It does not mean the workbench repo itself must always be packaged as a Python project.

## Decision rule

Add support scripts only when the generated agent has deterministic work that can be checked or repeated.

Do not add scripts just because an agent exists.

## Preferred location

For scripts owned by one generated agent:

```text
.claude/agents/<agent-name>.md
.claude/agents/<agent-name>.scripts/<script>.py
```

For scripts owned by one skill:

```text
.claude/skills/<skill-name>/SKILL.md
.claude/skills/<skill-name>/scripts/<script>.py
.claude/skills/<skill-name>/README.md
```

For shared scripts used by multiple generated agents:

```text
tools/<tool-name>.py
```

For installable Python tooling in the generated project:

```text
pyproject.toml
src/<package_name>/<module>.py
```

## Installable script shape

Use this only when the generated project needs command-line scripts.

```toml
[project]
name = "<project-name>"
version = "0.1.0"
requires-python = ">=3.11"

[project.scripts]
<command-name> = "<package_name>.<module>:main"
```

## README requirement

When support scripts are created, update the relevant README with:

```text
Install:
  pip install -e .

Run:
  <command-name>

What it checks:
  <short description>

Used by:
  <generated agent or skill name>
```

## Agent instruction requirement

The generated agent should say:

```text
Use the support script for deterministic checks when available.
If the script is missing or cannot run, report that as not verified.
Do not replace a deterministic script with a prose-only check.
```

## Verification requirement

The verifier should check:

- script exists
- script location matches ownership
- README explains install and run commands
- generated agent references the script when relevant
- skipped script runs are reported as not verified
