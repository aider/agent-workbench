# Context Feasibility Note

Decision: Option B - partially reliable.

Same-chat context is documented as part of the current VS Code request context, but full prior history after agent or model switching is not documented as guaranteed complete.

## What Was Checked

- VS Code context docs: https://code.visualstudio.com/docs/agents/concepts/context
- VS Code custom agents docs: https://code.visualstudio.com/docs/agent-customization/custom-agents
- VS Code custom instructions docs: https://code.visualstudio.com/docs/agent-customization/custom-instructions
- GitHub Copilot CLI context docs: https://docs.github.com/en/copilot/concepts/agents/copilot-cli/context-management
- GitHub Copilot agent sessions docs: https://docs.github.com/en/copilot/how-tos/copilot-on-github/use-copilot-agents/manage-and-track-agents
- Codex manual sections for IDE context and `AGENTS.md`: https://developers.openai.com/codex/codex-manual.md

## What Is Documented

- VS Code says context for a request can include current-session conversation history, file contents, tool outputs, custom instructions, explicit references, and implicit context such as active files, selected text, visible errors, and git state.
- VS Code custom agents can be switched by selecting another agent, and handoffs can move between agents with relevant context and a pre-filled prompt.
- VS Code documents repository instructions, including `AGENTS.md`, as repository-level custom instructions.
- Codex documents that it reads `AGENTS.md` before work and that the IDE extension can use open files, selected text, file contents, tool output, and an ongoing record of work as context.
- GitHub Copilot CLI documents a context window, compaction, and checkpoints. It warns that long sessions can lose important context through summarization.

## What Is Not Confirmed

- I did not find official documentation that a newly selected VS Code/Copilot/Codex agent reliably receives the full untruncated prior chat history.
- I did not find official documentation that agents may directly read VS Code internal chat history files.
- If internal chat files exist, they are not a documented stable interface for this workflow.

## Reliable Sources For A Fixer

- visible current chat context, when available
- open files and selected text, when available
- workspace files
- `git status` and `git diff`
- changed files
- generated-agent instructions and skills
- `.agent-runs/*.md`, handoff notes, or trace files when present
- command, script, or test output visible in chat or files

## Not Reliable Enough To Depend On

- hidden chain of thought
- VS Code internal chat storage
- full prior tool-call logs
- full untruncated prior chat history

## Workbench Design

The fixer should use visible chat context first, but treat repository evidence as the source of truth.

It should not try to read VS Code internal chat history files unless official documentation later makes that supported.
