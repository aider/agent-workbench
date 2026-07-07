# Example: Profile Slow Flow

Use this when something is slow and you want evidence before changing code.

## Example prompt

```text
/profile-slow-flow

Our API endpoint is slow.
Expected: under 300 ms.
Actual: sometimes 2 to 5 seconds.
Environment: test.
Stack: Java, Spring Boot, Maven, AWS ECS.
Observability: Splunk, CloudWatch, Dynatrace.
```

## Expected flow

```text
/profile-slow-flow -> performance-profiler -> verifier if files change
```

## What the profiler should do

| Step | Output |
|---|---|
| Symptom | What is slow, expected time, actual time, environment |
| Path | Endpoint, job, query, downstream call, test, build step, or agent step |
| Evidence | File, log, trace, metric, or command output |
| Bottleneck | CPU, memory, IO, network, database, retry, timeout, downstream, or agent context |
| Next check | One focused check, not a long list |

## Expected short response

```text
Result: Current evidence points to downstream latency, not local CPU.
Evidence:
- Dynatrace trace shows 1.8s spent in downstream call.
Next check:
- Compare timeout and retry settings for that client.
Likely fix:
- Not enough evidence yet.
```

## Example for slow tests

```text
/profile-slow-flow

Maven tests are slow.
Expected: under 2 minutes.
Actual: 8 minutes.
```

Expected next check:

```text
Result: Need test timing first.
Evidence:
- No timing data yet.
Next check:
- Run Maven with timing or inspect slow test reports.
Likely fix:
- Not enough evidence yet.
```

## Example for slow agent workflow

```text
/profile-slow-flow

My agent workflow is slow. It scans too many files and repeats the same verification steps.
```

Expected next check:

```text
Result: Likely context and repeated scan overhead.
Evidence:
- Workflow repeats file discovery and verification in each step.
Next check:
- Move repeated checks into a skill and make one verifier pass at the end.
Likely fix:
- Split explorer, writer, and verifier roles.
```
