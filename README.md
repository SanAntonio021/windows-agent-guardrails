# windows-agent-guardrails

Safer PowerShell and Windows CLI execution for coding agents.

## What This Skill Does

`windows-agent-guardrails` is a Windows-first skill for terminal work that tends to fail for mechanical reasons:

- unquoted paths
- shell mismatch
- missing tool discovery
- fragile inline `python -c`
- unsafe archive or file-operation shapes
- repeating the same broken command after the first failure

It combines two layers:

- static guardrails for PowerShell and Windows CLI execution
- reusable validated command patterns for recurring high-risk scenarios

## When To Use It

Use this skill when a task on Windows involves:

- absolute paths with spaces, non-ASCII characters, or deep directory nesting
- external CLIs such as `git`, `pandoc`, `ffmpeg`, or archive utilities
- PowerShell commands that include inline Python
- recursive search, traversal, and content inspection
- copy, move, remove, extract, or conversion operations
- repairing a command that already failed once

## Core Workflow

1. Detect whether the task is a high-risk Windows command scenario.
2. Route to the closest validated pattern.
3. Reuse only the command skeleton, not old command history.
4. Substitute current tool names and paths.
5. Apply static guardrails before execution.
6. If the first attempt fails, change the command shape instead of retrying it unchanged.

## Included Pattern Families

- quoted CLI paths
- UTF-8-safe inline Python
- search and traversal
- archive and native file operations
- tool discovery before execution

## Repository Layout

```text
.
├─ SKILL.md
├─ README.md
├─ references/
│  ├─ validated-command-patterns.md
│  ├─ cli-paths.md
│  ├─ python-utf8.md
│  ├─ search-and-traversal.md
│  ├─ archive-and-file-ops.md
│  ├─ tool-discovery.md
│  └─ pattern-library-maintenance.md
└─ examples/
   ├─ sample-scenarios.md
   └─ before-after-command-repairs.md
```

## Example Scenarios

- `pandoc` conversion where both input and output paths contain spaces
- inline Python in PowerShell that may emit non-ASCII output
- recursive content search when `rg` is unavailable or unreliable
- archive extraction where exact path handling matters

See:

- [examples/sample-scenarios.md](examples/sample-scenarios.md)
- [examples/before-after-command-repairs.md](examples/before-after-command-repairs.md)

## Install

```bash
npx skills add <owner/repo@windows-agent-guardrails>
```

## Environment

- Windows
- PowerShell
- any external CLI the task actually needs

## Design Constraints

- Windows-first, PowerShell-first
- no workspace-specific assumptions
- no user-specific paths
- no full command-history storage
- no Bash or WSL guidance in v1

## Status

This repository is prepared as a public v0.1.0 skill draft.

Before publishing, choose a license and replace the install placeholder with the final GitHub owner and repo name.
