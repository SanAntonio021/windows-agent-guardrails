---
name: windows-agent-guardrails
description: Safer PowerShell and Windows CLI execution for AI agents and terminal-capable assistants. Use this skill when Windows terminal work is likely to fail due to quoting, path handling, shell mismatch, missing tool discovery, inline Python encoding, search and traversal shape, archive operations, or repeated retries of the same broken command.
---

# Windows Agent Guardrails

## Purpose

Standardize terminal execution on Windows so an AI agent or terminal-capable assistant chooses safer command shapes before running them.

This skill combines two layers:

- static guardrails for PowerShell and Windows CLI execution
- reusable validated command patterns for recurring high-risk scenarios

The goal is to reduce mechanical terminal failures, not to replace product logic or domain reasoning.

## Good Fit

This skill fits terminal-capable workflows built around systems such as:

- Codex
- OpenClaw
- Claude Code
- OpenCode
- Cline
- Goose
- other Windows-capable agent systems with terminal or tool execution

It can also be relevant to tool-enabled ChatGPT or Gemini workflows on Windows when the workflow actually reaches terminal execution.

## When To Use

Use this skill when a task on Windows involves:

- file paths that may contain spaces, non-ASCII characters, or deep directories
- external CLIs such as `git`, `pandoc`, `ffmpeg`, or archive utilities
- PowerShell commands that include `python -c`
- recursive search, traversal, and content inspection
- copy, move, remove, archive, or extraction operations
- a similar command that already failed once and needs a safer shape

## Workflow

1. Decide whether the command is a normal command or a high-risk Windows command.
2. If it is high-risk, route to the closest validated pattern in [references/validated-command-patterns.md](references/validated-command-patterns.md).
3. Reuse only the command skeleton: shell, quoting style, preflight checks, environment variables, and placeholder structure.
4. Substitute the current runtime inputs such as tool names and paths. Do not copy old command history verbatim.
5. Apply the static guardrails below before execution.
6. If the command fails once, change the command shape instead of repeating it unchanged.
7. If a repaired command proves broadly reusable, update the closest pattern entry rather than storing a one-off log.

## Static Guardrails

1. Default to PowerShell syntax unless the user explicitly asks for another shell.
2. Use absolute Windows paths when the target is known.
3. Quote every path argument.
4. Run preflight checks such as `Get-Command`, `Test-Path`, or `Get-ChildItem` before the main command when the scenario is high-risk.
5. Prefer native PowerShell file operations when they exist.
6. Prefer `-LiteralPath` when exact path interpretation matters.
7. Do not keep retrying the same broken command shape.
8. Do not read binary files as text just to inspect them.
9. Do not justify destructive commands unless the task really requires them.

## Pattern Routing

- Quoted CLI paths: [references/cli-paths.md](references/cli-paths.md)
- UTF-8-safe inline Python: [references/python-utf8.md](references/python-utf8.md)
- Search and traversal: [references/search-and-traversal.md](references/search-and-traversal.md)
- Archive and native file operations: [references/archive-and-file-ops.md](references/archive-and-file-ops.md)
- Tool discovery before execution: [references/tool-discovery.md](references/tool-discovery.md)
- Pattern index and update rules: [references/validated-command-patterns.md](references/validated-command-patterns.md), [references/pattern-library-maintenance.md](references/pattern-library-maintenance.md)

## Failure Resolution

1. Classify the failure:
   - path parsing
   - quoting
   - missing executable
   - wrong shell syntax
   - encoding
   - permission or lock issue
2. Change the command shape once.
3. If the second attempt still fails, explain the blocker instead of thrashing.
4. If a native CLI is the wrong tool, switch to a script or library-backed path.

## Boundary

- This skill is Windows-first and PowerShell-first. It is not a Bash or WSL skill.
- This skill standardizes command construction. It does not decide product behavior.
- This skill does not store full command history.
- This skill is not a Git advanced workflow pack.
- This skill does not assume a specific workspace layout or username.
- Pure web-only assistants without terminal or tool execution are not the main target.

## Examples

- Scenario examples: [examples/sample-scenarios.md](examples/sample-scenarios.md)
- Before/after command repairs: [examples/before-after-command-repairs.md](examples/before-after-command-repairs.md)

## Maintenance

- Keep the public guidance small and operational.
- Add new pattern entries only when the shape is stable across tasks.
- Prefer updating an existing pattern over adding a near-duplicate entry.
