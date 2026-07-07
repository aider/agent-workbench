# Install Agent Workbench

Use this guide to install Agent Workbench into your personal Claude Code configuration.

## Install Globally For Claude Code

Claude Code personal locations:

- agents: `~/.claude/agents/`
- skills: `~/.claude/skills/<skill-name>/SKILL.md`

Agent Workbench installs only its agents and skills globally. Do not install project examples or templates as global agents or skills unless an installed agent or skill explicitly needs them at runtime.

## What Gets Installed

- repo `.claude/agents/*.md` -> `~/.claude/agents/`
- repo `.claude/skills/*` -> `~/.claude/skills/`
- repo `skills/*` -> `~/.claude/skills/` when those folders are Claude Code skills

Preserve skill directory structure, including `SKILL.md` and any supporting files.

## Install Prompt

Paste this into Claude Code:

```text
Install Agent Workbench globally from https://github.com/aider/agent-workbench.
Use official Claude Code personal configuration locations.
Install:
- repo `.claude/agents/*.md` into `~/.claude/agents/`
- repo `.claude/skills/*` into `~/.claude/skills/`
- repo `skills/*` into `~/.claude/skills/` if they are Claude Code skills
Do not overwrite unrelated existing files.
Before copying:
1. inspect the repo layout
2. show what will be installed
3. detect name conflicts
4. back up or skip conflicting files unless they are clearly older Agent Workbench files
After copying:
1. verify installed files exist
2. run a short listing of installed workbench agents and skills
3. tell me whether I need to restart Claude Code
Do not install project examples/templates as global agents or global skills unless they are referenced by an installed skill/agent and needed at runtime.
```

## Manual Install

Prefer the install prompt if you are unsure about conflicts.

These simple commands can overwrite files with the same names:

```bash
git clone https://github.com/aider/agent-workbench /tmp/agent-workbench
mkdir -p ~/.claude/agents ~/.claude/skills
cp /tmp/agent-workbench/.claude/agents/*.md ~/.claude/agents/
cp -R /tmp/agent-workbench/.claude/skills/* ~/.claude/skills/
cp -R /tmp/agent-workbench/skills/* ~/.claude/skills/
```

For a safer manual install, use `cp -n`, compare files first, or copy to a timestamped backup directory before replacing anything.

## Update Existing Install

1. Pull or re-clone the latest Agent Workbench.
2. Compare installed files with the new repo files.
3. Back up changed local files before replacing them.
4. Replace only Agent Workbench-owned agents and skills.
5. Verify installed files after copying.

Do not overwrite unrelated personal agents or skills.

## Verify Install

```bash
find ~/.claude/agents -maxdepth 1 -type f -name '*.md' | sort
find ~/.claude/skills -maxdepth 2 -name SKILL.md | sort
```

Expected Agent Workbench agents include files such as:

- `~/.claude/agents/agent-architect.md`
- `~/.claude/agents/agent-flow-reviewer.md`
- `~/.claude/agents/agent-run-fixer.md`
- `~/.claude/agents/agent-writer.md`
- `~/.claude/agents/verifier.md`

Expected Agent Workbench skills include folders such as:

- `~/.claude/skills/architect-flow/SKILL.md`
- `~/.claude/skills/instrument-generated-agent/SKILL.md`
- `~/.claude/skills/verify-change/SKILL.md`

Claude Code watches existing agent and skill directories. If `~/.claude/agents/` or `~/.claude/skills/` did not exist before the current Claude Code session started, restart Claude Code so the new directory is watched.

## Uninstall

Remove only Agent Workbench-owned files:

```bash
rm -f ~/.claude/agents/agent-architect.md
rm -f ~/.claude/agents/agent-flow-profiler.md
rm -f ~/.claude/agents/agent-flow-reviewer.md
rm -f ~/.claude/agents/agent-run-fixer.md
rm -f ~/.claude/agents/agent-writer.md
rm -f ~/.claude/agents/verifier.md
rm -rf ~/.claude/skills/architect-flow
rm -rf ~/.claude/skills/instrument-generated-agent
rm -rf ~/.claude/skills/verify-change
```

Review each path before deleting if you have customized installed files.

## Safety Rules

- Inspect the source repo before installing.
- Show what will be copied before copying.
- Detect name conflicts.
- Do not overwrite unrelated user files.
- Back up or skip conflicts unless they are clearly older Agent Workbench files.
- Preserve skill directories and supporting files.
- Skills may be symlinked as skill-name directories, but copying is simpler and easier to audit.
- Agent files can be organized recursively, but identity comes from `name` frontmatter; keep names unique.
