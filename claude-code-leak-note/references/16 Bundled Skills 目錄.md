---
aliases: [Bundled Skills, 內建 Skills]
tags: [reference, tool-system]
source: "phase-04-skills-system/03-bundled-skills-catalog.md"
created: 2025-04-05
---

# 16 Bundled Skills 目錄

> `src/skills/bundled/` 下的 16 個內建技能

## 按功能分類

### 程式碼品質類

| Skill | 行數 | 功能 | allowedTools |
|-------|------|------|-------------|
| **simplify** | — | 三個平行 review agents 進行程式碼品質審查 | AgentTool |
| **debug** | 103 | 除錯工作流 | Bash、Read、Grep |
| **stuck** | 79 | 卡住時的診斷指引 | Read、Grep |

### 工作流類

| Skill | 行數 | 功能 | allowedTools |
|-------|------|------|-------------|
| **batch** | 124 | 大規模批次修改 | AgentTool、Bash |
| **loop** | 92 | 迴圈執行（重複性任務）| Bash |
| **scheduleRemoteAgents** | 447 | 遠端 agent 排程 | ScheduleCron |

### 設定/管理類

| Skill | 行數 | 功能 | allowedTools |
|-------|------|------|-------------|
| **updateConfig** | 475 | Claude Code 設定更新（hooks、rules 等）| Edit |
| **keybindings** | 339 | 鍵盤快捷鍵設定指引 | — |
| **skillify** | 197 | 從 session 生成新 Skill | Edit、Write |

### API/整合類

| Skill | 行數 | 功能 | allowedTools |
|-------|------|------|-------------|
| **claudeApi** | 196 | Claude API 使用指引 | Bash |
| **remember** | 82 | 記憶管理 | Edit |

### 其他

| Skill | 行數 | 功能 | allowedTools |
|-------|------|------|-------------|
| **loremIpsum** | 282 | Lorem Ipsum 生成（測試用）| — |

## 條件載入機制

部分 skills 根據 feature flag 或環境條件決定是否載入：

```
if (feature('AGENT_TRIGGERS')) → scheduleRemoteAgents 載入
if (USER_TYPE === 'ant') → 部分 skill 有額外功能
```

## 設計模式觀察

> [!info] Skill 即 Orchestrator
> 高品質 skills 的共同特徵：
> 1. **不直接做事** — 透過 AgentTool 派遣 subagents 並行工作
> 2. **提供明確流程** — step-by-step 指引模型如何完成任務
> 3. **最小工具集** — 只授權必要的工具（Principle of Least Privilege）

→ 詳見 [[SkillTool 與 Skills 系統]]、[[Skills vs Tools 設計哲學]]

---

> [!tip] 導航
> 返回 [[Tool System MOC]] · [[Claude Code 逆向工程知識庫]]
