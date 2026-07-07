# Generated Agent Config Template

Use this template for project-specific values that should not be embedded in a reusable generated agent.

## Purpose

Generated agents should contain behavior.

Configuration files should contain project-specific values.

## Agent

```text
agent: <generated-agent-name>
version: <config-version>
```

## Scope

```text
project_name: <optional project label>
repo_paths:
- <path or glob>
include_patterns:
- <pattern>
exclude_patterns:
- <pattern>
```

## Domain vocabulary

```text
terms:
- <term>
aliases:
- <alias>
```

## Signals to inspect

```text
log_patterns:
- <pattern>
trace_fields:
- <field>
metric_names:
- <metric>
```

## Commands

```text
safe_read_commands:
- <command>
check_commands:
- <command>
```

## Output preferences

```text
summary_level: short | normal | detailed
include_trace_summary: true | false
```

## Rules

- Keep reusable agent instructions generic.
- Put project-specific paths, names, patterns, and commands here.
- Keep secrets out of this file.
- If a value changes by project, it belongs in config, not in the generated agent.
