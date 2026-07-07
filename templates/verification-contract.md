# Verification Contract

Use this template before asking an agent to verify work.

## Change

Describe the intended change in one or two sentences.

```text
<what should be true after the change>
```

## Scope

Files or areas that are in scope:

- `<path>`

Files or areas that are out of scope:

- `<path or topic>`

## Acceptance criteria

The change is complete only if all required criteria pass.

| ID | Criterion | Required evidence |
|---|---|---|
| AC1 | <criterion> | <file, command, output, or observation> |
| AC2 | <criterion> | <file, command, output, or observation> |
| AC3 | <criterion> | <file, command, output, or observation> |

## Checks to run

Use the narrowest meaningful checks first.

```bash
<command 1>
<command 2>
```

If this is a documentation-only change, checks may be file inspection, markdown structure review, and link/path verification.

## Done definition

The work is done when:

- all required acceptance criteria pass
- relevant checks have been run or explicitly marked as not applicable
- skipped checks are listed with reasons
- remaining risks are documented

## Verifier output required

The verifier must return:

```text
Result: PASS | FAIL | PARTIAL

Evidence:
| Criterion | Result | Evidence |
|---|---|---|
| <criterion> | PASS/FAIL/SKIPPED | <evidence> |

Commands run:
- <command> -> <result>

Not verified:
- <item or none>

Recommended fixes:
- <fix or none>
```
