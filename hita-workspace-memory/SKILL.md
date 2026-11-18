---
name: hita-workspace-memory
description: Use when a task names a project listed in the workspace index, needs project history, report conventions or past decisions, or the user says 记一下 / 沉淀 / 归档 / 整理进工作区 / 以后复用. Routes reads and writes to the right project memory in a file-based workspace; does not store project facts itself.
---

# Hita Workspace Memory

## Purpose

Use a file-based workspace as the agent's project memory. The skill does not store facts; it tells the agent where to read before working and where to write when asked to remember. It works the same in Claude Code and Codex, and both can share one workspace.

Default workspace (change this line if yours lives elsewhere):

```text
~/.codex/hita_workspace
```

Keep this skill lean. Project facts live in `projects/<项目名称>/`, not here. A blank workspace to start from is in `templates/workspace/` next to this file.

## Read Workflow

When a task mentions a project, past work, a report, a recurring analysis, or a path the index knows about:

1. Read `工作区索引.md` and match the project name, trigger words, or original path.
2. Read the project's `项目卡.md`. **The files the card lists as required reading override the default below.**
3. Read further only as the task needs:

| Task | Also read |
|---|---|
| Writing or revising a report | `报告规则.md`, then the report template |
| Continuing a cycle / comparing with last time | `周期摘要.md` |
| Business context, known false alarms, boundaries | `项目记忆.md` |
| Locating source files | `文件索引.md` |

4. Then read the original project files, templates, or wiki pages the card points to.
5. Treat the current files as primary; use memory for context, comparison, and known pitfalls.

If no project matches, search the index and the paths the user gave, then decide whether to create a new project folder.

### Verify before trusting

Memory reflects when it was written. Before relying on versions, paths, "current mainline", platform state, or counts, check them against the current file or system; if they differ, use the current value and tell the user the card is stale. A conclusion the user confirmed stays valid within its recorded scope and date; do not extend it to other projects or later changes without evidence.

## Write Workflow

When the user says 记一下 / 沉淀 / 归档 / 以后复用, or otherwise asks to save something, decide where it belongs:

| What it is | Where it goes |
|---|---|
| About one project | `projects/<项目名称>/` (the five files below) |
| A general way of working, true across projects | `memory/通用工作流程规则.md` |
| The user's preferences | `memory/用户长期偏好.md` |
| A durable structural decision | `memory/长期决策.md` |
| Something the user corrected | `memory/纠错反馈.md` |
| A reusable template / script | `templates/` / `scripts/` |

A new project gets five Chinese-named files:

| Content | File |
|---|---|
| Identity, trigger words, required reading order | `项目卡.md` |
| Stable facts, business context, recurring false alarms, technical boundaries | `项目记忆.md` |
| Per-cycle summaries, numbers, conclusions, version changes | `周期摘要.md` |
| Report style, delivery rules, the user's wording corrections | `报告规则.md` |
| Original files, reports, spreadsheets, wiki pages, source paths | `文件索引.md` |

Register the new project in `工作区索引.md` (name, trigger words, path to its card).

## Dedupe before writing

Before adding to any file, search it for the same point. If an entry exists, update or merge it in place (keep the newer fact, note the date if it changed); only append when it is genuinely new. One line per fact, no repeated slogans, no restating what the source file already records. When a file grows long, tell the user and propose a consolidation rather than silently trimming.

## Naming Rules

- Chinese names for `.md` files meant for the user to read.
- Keep `AGENTS.md` / `CLAUDE.md` in English; they are agent conventions.
- English or pinyin for script filenames when command-line use matters.

## Safety Rules

- Do not copy raw spreadsheets, documents, logs, licenses, or large datasets into the workspace; index them in `文件索引.md` instead.
- Never record passwords, API keys, cookies, tokens, private keys, or subscription links. For sensitive files, record only the path and purpose, not the contents (密码密钥，只记位置，不记内容).
- If a write was tested against a live system, record the boundary and the result, not the credential.

## Project Memory Rules

- Do not mix projects: a lesson from project A belongs under `projects/项目A/`, even if it was learned while working on B.
- Do not carry old numbers into a new cycle unless the current files support them.
- If the user confirmed something is normal business behaviour or a false alarm, keep that as the project's stable conclusion until new evidence appears.
- Separate "the platform says so" from "verified": a flag, a hit, a success code, or a documented API is not the same as an observed real effect.
