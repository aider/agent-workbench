# Workbench Verification Contract

Use this contract to verify the core workbench.

## Change

The repository should provide a small system for turning messy workflows into reusable generated agents, reviewing existing external agent flows, and fixing failed previous generated-agent runs with explicit verification, external input boundaries, optional support scripts for deterministic generated-agent checks, flow diagrams, and operation-tree profiling.

## Required core flow

```text
/architect-flow -> agent-architect -> agent-writer -> verifier
```

For a slow or stuck generated agent:

```text
generated agent trace -> agent-flow-profiler
```

For an existing external agent system that needs review before rewrite:

```text
existing agent system -> agent-flow-reviewer -> evidence-based improvement plan
```

For a failed, interrupted, or incorrect previous generated-agent run:

```text
visible chat context + repo evidence -> agent-run-fixer -> smallest correct fix
```

## Acceptance criteria

| ID | Criterion | Required evidence |
|---|---|---|
| AC1 | README describes the small core flow | `README.md` shows the required core flow |
| AC2 | README layout includes all repository examples and templates | README lists config, support scripts, diagram, trace, profiling, concise-output, messy-flow, first-request, and hang-debug files |
| AC3 | AGENTS includes the core architecture standards | `AGENTS.md` mentions external input boundary, flow diagrams, and operation-tree profiling |
| AC4 | Architect owns decomposition | `agent-architect.md` says the user does not provide decomposition rules |
| AC5 | Architect owns external input boundaries | `agent-architect.md` defines external input boundary decisions |
| AC6 | Architect owns support-script decisions | `agent-architect.md` says support scripts are for generated agents when deterministic checks are useful |
| AC7 | Architect owns flow diagrams | `agent-architect.md` requires a small flow diagram for non-trivial generated agents |
| AC8 | Writer creates specific artifacts after architecture is known | `agent-writer.md` says to hand off to architect when architecture is unclear |
| AC9 | Writer keeps reusable logic separate from external inputs | `agent-writer.md` includes an external input boundary |
| AC10 | Writer adds support scripts only when useful | `agent-writer.md` includes support script rules and avoids packaging by default |
| AC11 | Writer adds flow diagrams to non-trivial generated agents | `agent-writer.md` includes flow diagram rules |
| AC12 | Writer adds operation-tree profiling to generated agents | `agent-writer.md` includes generated-agent profiling rules |
| AC13 | Generated-agent instrumentation skill exists | `instrument-generated-agent/SKILL.md` defines operation-tree profiling |
| AC14 | Agent-flow profiler analyzes generated-agent traces | `agent-flow-profiler.md` analyzes operation-tree traces |
| AC15 | Every planned operation has a final-state rule | relevant files say planned operations must end as `END`, `SKIP`, or `ERROR`, skipped operations need a reason, and the stuck point is the last `START` without a final state |
| AC16 | Trace template exists and is generic | `templates/agent-run-trace.md` uses operation-tree trace format without project-specific examples |
| AC17 | Config template exists | `templates/generated-agent-config.md` exists for values supplied outside reusable agents |
| AC18 | Support scripts template exists and is bounded | `templates/generated-agent-support-scripts.md` exists, prevents ad hoc helper code, documents owner locations, and does not imply every agent needs scripts or Python packaging |
| AC19 | Flow diagram template exists | `templates/generated-agent-flow-diagram.md` exists |
| AC20 | Verifier checks the new architecture | `verifier.md` checks external input boundary, flow diagram, and operation-tree profiling |
| AC21 | Examples follow the current architecture | examples start from architect and include config boundary, diagram, profiling, and verification where relevant |
| AC22 | Output remains short | core agents or templates require short output |
| AC23 | README describes the existing-flow review use case | `README.md` mentions `agent-flow-reviewer` and says review does not rewrite by default |
| AC24 | Review artifact inventories before judging | `agent-flow-reviewer.md` requires file inventory and says not to rely only on README |
| AC25 | Review artifact reconstructs actual flow before recommendations | `agent-flow-reviewer.md` requires flow reconstruction before recommending changes |
| AC26 | Review artifact checks workbench standards | `agent-flow-reviewer.md` checks external input boundary, support scripts, ad hoc helper code, diagrams, operation-tree profiling, verifier separation, and evidence-based recommendations |
| AC27 | Review artifact is read-only by default | `agent-flow-reviewer.md` has no write/edit tools and says not to rewrite by default |
| AC28 | Short review entrypoint exists | `REVIEW.md` exists, points to `agent-flow-reviewer.md`, says review-only by default, says modification requires an explicit request for changes, fixes, refactor, rewrite, or implementation, requires inventory first, and requires flow reconstruction before recommendations |
| AC29 | Existing-flow review is automatic from repo instructions | `AGENTS.md` routes review requests to `agent-flow-reviewer.md`, says the user does not need to mention `REVIEW.md`, and preserves review-only default behavior |
| AC30 | README shows the minimal review prompt | `README.md` shows `Review <target-project>.` as the primary prompt and says `REVIEW.md` is optional reference only |
| AC31 | Context feasibility note exists | `docs/context-feasibility.md` explains what was checked, does not claim unsupported VS Code chat storage access, and states Option A/B/C |
| AC32 | Fixer workflow exists | `agent-run-fixer.md` identifies the previous agent/run before fixing, uses visible chat context if available, verifies against repository evidence, and does not rely only on hidden chat history |
| AC33 | Fixer routing is documented | `AGENTS.md` routes "Fix this" style generated-agent failures to `agent-run-fixer.md` and `README.md` documents the simple `Fix this.` UX |
| AC34 | Fixer handles command and script failures | `agent-run-fixer.md` captures exact command and error when visible, classifies command usage failures, checks source of truth before fixing, avoids guessing command options, fixes the smallest root cause, and reruns the narrowest safe verification command |
| AC35 | Fixer checks root-cause artifacts before downstream fixes | `agent-run-fixer.md` requires inspecting likely upstream/root-cause artifacts before proposing or applying fixes, before calling fixes safe, ready, proven, or low-risk, and before editing downstream implementation files |
| AC36 | Fixer prefers producer/root-cause fixes | `agent-run-fixer.md` prefers producer/root-cause fixes over downstream patches and allows downstream guards only with explicit evidence and reason |
| AC37 | Fixer responds to root-cause pushback | `agent-run-fixer.md` treats user pushback such as "did you read the agent?" as a signal to stop and inspect the likely root-cause artifact |
| AC38 | Install guide exists | `INSTALL.md` exists and `README.md` points to it |
| AC39 | Install prompt exists | `INSTALL.md` includes a ready-to-paste prompt for installing globally from the GitHub repo |
| AC40 | Install paths use official Claude Code personal locations | `INSTALL.md` uses `~/.claude/agents/` and `~/.claude/skills/<skill-name>/SKILL.md` |
| AC41 | Install safety is documented | `INSTALL.md` warns about conflicts and overwrites, requires inspection, conflict detection, backup or skip behavior, and verification after copying |
| AC42 | Install lifecycle is documented | `INSTALL.md` includes update, verify, uninstall, and restart guidance |
| AC43 | Install scope is bounded | `INSTALL.md` says not to install examples or templates globally as agents or skills unless needed at runtime |
| AC44 | Update prompt exists | `INSTALL.md` includes a ready-to-paste prompt for updating an Agent Workbench global install |
| AC45 | Update scope is bounded | `INSTALL.md` says updates only replace Agent Workbench-owned files and directories |
| AC46 | Update safety is documented | `INSTALL.md` says not to delete the whole `~/.claude/agents` or `~/.claude/skills`, backs up changed files before replacing, and stops when ownership is unclear |
| AC47 | README mentions update prompt | `README.md` mentions `Update Agent Workbench global install.` |

## Manual checks

Check these files:

```text
README.md
AGENTS.md
INSTALL.md
REVIEW.md
.claude/agents/agent-architect.md
.claude/agents/agent-flow-reviewer.md
.claude/agents/agent-run-fixer.md
.claude/agents/agent-writer.md
.claude/agents/agent-flow-profiler.md
.claude/agents/verifier.md
.claude/skills/architect-flow/SKILL.md
.claude/skills/instrument-generated-agent/SKILL.md
skills/verify-change/SKILL.md
templates/agent-template.md
templates/agent-run-trace.md
templates/concise-output.md
templates/generated-agent-config.md
templates/generated-agent-support-scripts.md
templates/generated-agent-flow-diagram.md
templates/flow-architecture.md
templates/verification-contract.md
templates/workbench-verification-contract.md
docs/context-feasibility.md
examples/agent-flow-hang-debug.md
examples/first-agent-request.md
examples/generated-agent-profiling.md
examples/messy-workflow-to-agent-flow.md
```

## Expected verifier result

```text
Result: PASS | FAIL | PARTIAL
Evidence:
- <short evidence for each criterion>
Not verified:
- <item or none>
Next:
- <one next step or none>
```
