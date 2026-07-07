# Concise Output Template

Use this template as the default output style for agents in this repo.

## Goal

Agent output should be simple, short, and easy to scan.

## Default format

```text
Result: <one sentence>
Changed:
- <file or decision>
Why:
- <one short reason>
Next:
- <one next step or none>
```

## Rules

- Start with the result.
- Use short bullets.
- Avoid long background explanations.
- Keep detailed reasoning inside files or contracts.
- Include evidence when verification matters.
- Use longer output only when the user asks or when there is a risk.

## Expanded format for verification

```text
Result: PASS | FAIL | PARTIAL
Evidence:
- <short evidence>
Not verified:
- <item or none>
Next:
- <one next step>
```
