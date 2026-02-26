# vibe-product-runbook / 项目开发会话记忆 Skill

## 1) 安装 | Install

### 推荐安装方式 / Recommended install

```bash
npx skills add https://github.com/Ogurijay/vibe-product-runbook --skill vibe-product-runbook
```

### 目的 / Purpose

这个 skill 用于把“项目进度、上下文和里程碑交接”保存在仓库里，避免每次会话都从头讲一遍。

This skill stores project progress, context, and milestone handoff information in-repo so each session can continue from where the previous one left off.

## 2) 你这个 Skill 会做什么 | What this skill does

- 在项目级别管理 `.vibe/*` 状态文件（不会跨项目混淆）。
- 在里程碑结束后自动产出交接信息。
- 从会话中提取“正向可复用经验”，沉淀为候选 skill 模板。
- 统一输出风格：先结论，再原因，再动作；适合新手，也适合你不想反复阅读背景时使用。

- Manage `.vibe/*` state files at project scope (never global, no cross-project mixing).
- Auto-update handoff notes at milestone end.
- Extract positive reusable patterns into candidate skill items.
- Enforce beginner-friendly response style: conclusion first, then why, then action.

### 不会做的事（边界）| Scope limits

- 不会自动替代你写代码和决策。
- 不会保存完整聊天记录（也不依赖它）。
- 不会持久化失败反例，只记录可复用的正向经验。

- Will not replace your actual coding/decision work.
- Will not persist full chat logs (and does not depend on them).
- Will not persist failure anti-patterns, only positive reusable patterns.

## 3) 项目结构 | Project structure

```text
vibe-product-runbook/
├─ SKILL.md                      # 技能定义（触发条件与执行规则）
├─ README.md                     # 当前文件：安装/使用说明
└─ templates/
   ├─ project-state.json         # 默认状态文件模板
   ├─ session-brief.md           # 默认会话交接模板
   └─ skills-candidates.json     # 默认经验候选模板
```

## 4) 安装后会生成的 `.vibe` 结构 | `.vibe` generated in project

在目标项目中，建议创建：

- `./.vibe/project-state.json`
- `./.vibe/session-brief.md`
- `./.vibe/skills-candidates.json`

### 4.1 `project-state.json` / Project state

用途：机器可读的当前进度状态，用于自动恢复。

Fields（字段）:
- `project_name`（项目名）
- `current_goal`（当前目标）
- `phase`（`planning` / `build` / `validate` / `release` / `paused`）
- `progress`（0-100）
- `completed`（已完成清单）
- `blockers`（阻塞项）
- `next_steps`（下一步）
- `active_tools`（使用工具）
- `accepted_skills`（本项目接受的 Skill）
- `notes`（说明）
- `updated_at`（更新时间）

用途: machine-readable progress snapshot for auto-resume.

- `project_name`
- `current_goal`
- `phase` (`planning` / `build` / `validate` / `release` / `paused`)
- `progress` (0-100)
- `completed`
- `blockers`
- `next_steps`
- `active_tools`
- `accepted_skills`
- `notes`
- `updated_at`

### 4.2 `session-brief.md` / Session brief

用途：给下一个会话看的交接页（人类可读）。

Recommended sections:
- 目标 / Goal
- 已完成 / Completed
- 下一步 / Next Steps
- 注意事项 / Notes

### 4.3 `skills-candidates.json` / Reusable candidates

用途：只存“可复用成功经验”，不存失败反例。

每条记录包括：
- `skill_name`
- `use_case`
- `inputs`
- `outputs`
- `required_tools`
- `when_to_use`

## 5) 触发时机 | When this skill should trigger

- 你说“开始一个项目/继续项目/接着开发上次内容”。
- 用户要求“跨会话不丢记忆”。
- 需要把里程碑经验归档为可复用规则。

## 6) 一句话使用方法（新手友好）| Quick usage

1) 第一次用：安装后，在项目根目录创建 `.vibe/*` 三个文件。
2) 继续会话：先读 `.vibe/project-state.json` 和 `.vibe/session-brief.md`，再执行。
3) 里程碑结束：更新三类文件，特别是下一步和新经验。

1. On first use: create `.vibe/*` in the project root.
2. On continuation: read `.vibe/project-state.json` + `.vibe/session-brief.md` first.
3. On milestone end: update all three files, especially next steps and new reusable insight.

## 7) 一些示例命令 | Sample commands

```bash
# 读取交接信息（建议每次会话第一步）
grep -n "current_goal\|next_steps\|progress" .vibe/project-state.json
Get-Content .vibe/session-brief.md

# 写入模板（首次初始化）
New-Item -ItemType Directory -Force -Path .vibe
Copy-Item templates/project-state.json .vibe/project-state.json
Copy-Item templates/session-brief.md .vibe/session-brief.md
Copy-Item templates/skills-candidates.json .vibe/skills-candidates.json
```
