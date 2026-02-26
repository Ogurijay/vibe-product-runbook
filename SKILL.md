---
name: vibe-product-runbook
description: Project-level workflow skill for AI coding sessions that keeps progress memory in-repo and extracts reusable patterns; use when starting/continuing product development to avoid losing context between sessions. 触发场景：开始新项目、继续开发、项目里程碑交接。
---

# Vibe Project Runbook

这个技能的目标是：让每次开发会议都能“从上一次停的地方接着做”，而不是每次重写背景。

## 你可以把它理解成什么？
这是一个“项目记忆本”。

- 之前会话的记忆放进文件，不放进聊天窗口。
- 下次继续时先读文件，不重复说一遍需求。

## 该技能执行顺序（必须）

1. 检查项目级 .vibe/ 是否存在。
2. 检查 .vibe/project-state.json。
3. 检查 .vibe/session-brief.md。
4. 检查 .vibe/skills-candidates.json。
5. 读到后先做 3 件事：
   - 先复述一次本次目标。
   - 先列出下一步。
   - 再开始当前阶段动作。

## 文件职责（项目级，不是全局）

### /.vibe/project-state.json
机器可读状态。用于自动恢复。

内容字段：
- project_name
- current_goal
- phase: planning / build / validate / release / paused
- progress: 0-100
- completed
- lockers
- 
ext_steps
- ctive_tools
- ccepted_skills
- 
otes
- updated_at

### /.vibe/session-brief.md
给下一个会话看的交接内容，给人读。

至少包含：
- 本次目标
- 已完成
- 下一步
- 注意事项

### /.vibe/skills-candidates.json
只存正向可复用经验（不存反向失败案例）。

每条候选必须有：
- skill_name
- use_case
- inputs
- outputs
- equired_tools
- when_to_use

## 里程碑结束（必须执行）

每次完成里程碑后立即做：
1. 更新 project-state.json。
2. 更新 session-brief.md。
3. 在 skills-candidates.json 新增一条正向经验。

## 安装后默认行为（小白优先）

每次新会话开始时，先读这 3 个文件，再继续。

不要把术语堆给用户：
- 先说结论
- 再说原因
- 再说操作

## 命令模板

- 项目首次启动：
`ash
cp templates/project-state.json .vibe/project-state.json
cp templates/session-brief.md .vibe/session-brief.md
cp templates/skills-candidates.json .vibe/skills-candidates.json
`

- 更新进度（示例）：
`ash
# 按你会话中的实际目标更新文件内容
`

- 继续项目：
`ash
grep -n "next_steps\|current_goal\|progress" .vibe/project-state.json
type .vibe/session-brief.md
`

## 交付检查（每次会话都要）

- 是否有 .vibe/project-state.json？
- current_goal 是否明确？
- 
ext_steps 是否只放下一步？
- 有无新增可复用经验？

## 输出风格

- 每次解释先给一句“最重要结论”。
- 用短句。
- 每步带“为什么”。
- 遇到选项先给默认值。
