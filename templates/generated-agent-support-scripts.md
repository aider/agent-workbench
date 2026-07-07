# Generated Agent Support Scripts Template

Use this template when a generated agent needs prebuilt deterministic actions or repeatable local tooling.

## Purpose

Support scripts prevent generated agents from writing ad hoc helper code while executing a flow.

Generated agents should use prepared scripts for deterministic work instead of creating temporary code during the run.

Examples:

- repository tree audit
- config validation
- trace validation
- diagram validation
- no-hardcoded-values check
- operation-tree final-state check
- generated-agent verification

## Decision rule

Add support scripts only when the generated agent has deterministic work that can be checked, repeated, or reused.

Do not add scripts just because an agent exists.

Do not let the generated agent create helper scripts during its normal flow when the need is predictable.

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

Use this when the generated project needs command-line scripts.

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

What it does:
  <short description>

Used by:
  <generated agent or skill name>
```

## Agent instruction requirement

The generated agent should say:

```text
Use the support script for deterministic work when available.
If the script is missing or cannot run, report that as not verified.
Do not write ad hoc helper code during the flow when a prepared support script should exist.
Do not replace a deterministic script with a prose-only check.
```

## Verification requirement

The verifier should check:

- script exists
- script location matches ownership
- README explains install and run commands
- generated agent references the script when relevant
- generated agent does not create ad hoc helper code during normal flow
- skipped script runs are reported as not verified
