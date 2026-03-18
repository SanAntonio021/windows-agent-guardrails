# windows-agent-guardrails

面向 AI agents 和具备终端能力助手的更安全的 PowerShell 与 Windows CLI 执行技能。

[English](README.md)

## 这个 Skill 是做什么的

`windows-agent-guardrails` 是一个 Windows 优先的技能，专门处理那些经常因为“命令写法”而失败的终端任务，例如：

- 路径没加引号
- shell 用错
- 没先检查外部工具是否存在
- `python -c` 在 PowerShell 下很脆弱
- 压缩包处理或文件操作命令形态不稳
- 第一次失败后还在重复同一个错误命令

它把两层能力合在一起：

- PowerShell / Windows CLI 的静态护栏
- 面向高风险场景的可复用命令模式库

## 适合哪些 Agent

这个 skill 很适合这类具备终端能力的工作流：

- Codex
- OpenClaw
- Claude Code
- OpenCode
- Cline
- Goose
- 其他能在 Windows 上实际执行终端或工具调用的 agent 系统

如果是带工具能力、并且工作流最终会落到 Windows 终端执行上的 ChatGPT 或 Gemini，也同样相关。

## 什么时候该用它

当 Windows 上的任务涉及这些情况时，适合启用这个 skill：

- 绝对路径里有空格、中文或很深的目录层级
- 需要调用外部 CLI，例如 `git`、`pandoc`、`ffmpeg` 或压缩工具
- PowerShell 命令里包含内联 Python
- 需要递归搜索、遍历目录或做文本检索
- 需要复制、移动、删除、解压、转换等文件操作
- 某个命令已经失败过一次，需要换一种更稳妥的命令形态

## 核心工作流

1. 先判断这是不是高风险的 Windows 命令场景。
2. 再路由到最接近的已验证模式。
3. 只复用命令骨架，不复用旧任务的完整命令历史。
4. 用本次任务的真实工具名和路径替换占位符。
5. 执行前再叠加静态护栏。
6. 如果第一次失败，就改命令形态，不要原样重试。

## 内置模式族

- 带引号的 CLI 路径
- UTF-8 安全的内联 Python
- 搜索与遍历
- 压缩包与原生文件操作
- 执行前工具发现

## 仓库结构

```text
.
├─ SKILL.md
├─ README.md
├─ README.zh-CN.md
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

## 示例场景

- `pandoc` 转换时输入输出路径都带空格
- PowerShell 里运行可能输出中文的内联 Python
- `rg` 不可用或不稳定时做递归文本搜索
- 需要精确路径处理的压缩包解压

可参考：

- [examples/sample-scenarios.md](examples/sample-scenarios.md)
- [examples/before-after-command-repairs.md](examples/before-after-command-repairs.md)

## 安装

```bash
npx skills add SanAntonio021/windows-agent-guardrails@windows-agent-guardrails
```

## 适用环境

- Windows
- PowerShell
- 任务实际需要的外部 CLI

## 设计约束

- Windows 优先，PowerShell 优先
- 不假设固定工作区结构
- 不包含用户私有路径
- 不保存完整命令历史
- `v1` 不覆盖 Bash 或 WSL 场景
- 不面向完全没有终端或工具执行能力的纯网页助手

## 当前状态

这个仓库已经作为公开 `v0.1.0` skill 发布。

当前许可证：[MIT](LICENSE)
