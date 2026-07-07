# Agent Flow Review Entrypoint

Use this file when reviewing an existing external agent system.

Default mode: review only.

A review request means inspect, reconstruct the flow, find issues, and propose an evidence-based improvement plan.

Do not modify the target project unless the user explicitly asks for changes, fixes, refactor, rewrite, or implementation.

## Minimal Prompt

```text
Review <target-project> using REVIEW.md.
```

## Workflow

Follow the full review workflow in:

```text
.claude/agents/agent-flow-reviewer.md
```

## Required Behavior

1. Build a full file inventory first.
2. Do not rely only on README.
3. Reconstruct the actual flow before recommendations.
4. Check role separation, external input boundaries, support scripts, diagrams, profiling, and verification.
5. Check that predictable deterministic actions use prepared support scripts instead of ad hoc helper code during normal flow.
6. Check operation-tree profiling uses `phase -> operation -> optional sub_operation` and final states `END | SKIP | ERROR`.
7. Report evidence for every finding.
8. If something was not verified, say so.

## Output

Return:

```text
Result: PASS | PARTIAL | FAIL
Flow:
- <reconstructed flow or unknown>
Findings:
1. <finding>
   Evidence: <file/path or line>
   Why it matters: <short reason>
   Recommendation: <short recommendation>
   Priority: P1/P2/P3
Plan:
1. <first safe improvement>
2. <second safe improvement>
3. <third safe improvement>
Not verified:
- <item or none>
Next:
- <one next step>
```
