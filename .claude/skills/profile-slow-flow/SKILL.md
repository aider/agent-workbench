---
description: Profile and investigate something slow before changing code. Use when an app, service, API, test, build, database query, infrastructure flow, or agent workflow is slow, timing out, lagging, or taking too long.
argument-hint: "[what is slow]"
---

# Profile Slow Flow

Use this skill when something is slow and the first step should be profiling, not fixing.

## Goal

Find where time is being spent and decide the next smallest useful check.

## Input

Use `$ARGUMENTS` as the slow symptom.

If the symptom is missing, ask one short question: what is slow and where?

## Process

1. Capture the symptom:
   - what is slow
   - expected time
   - actual time
   - environment
2. Identify the path:
   - endpoint
   - job
   - test
   - build step
   - database query
   - downstream call
   - agent step
3. Ask `performance-profiler` to investigate with evidence.
4. Classify the bottleneck.
5. Recommend one next check.
6. Suggest a fix only when evidence supports it.

## Default flow

```text
/profile-slow-flow -> performance-profiler -> verifier if files change
```

## Output

Keep it short:

```text
Result: <current finding or hypothesis>
Evidence:
- <timing, file, log, metric, or none yet>
Next check:
- <one focused check>
Likely fix:
- <only if supported by evidence>
```

## Rules

- Profile before fixing.
- Do not turn every slow issue into a code change.
- Do not give a long generic checklist.
- If there is no timing data, first propose how to collect it.
- If files are changed later, verify with `verifier`.
