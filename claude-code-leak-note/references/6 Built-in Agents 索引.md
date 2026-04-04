---
aliases: [Built-in Agents, 內建 Agents]
tags: [reference, agent-architecture]
source: "phase-03-agent-architecture/02-built-in-agents.md"
created: 2025-04-05
---

# 6 Built-in Agents 索引

> `src/tools/AgentTool/built-in/` 下的 6 個 BuiltInAgentDefinition

## 概覽表

| Agent | 檔案 | 模型 | 用途 | 工具集 |
|-------|------|------|------|--------|
| **general-purpose** | `generalPurposeAgent.ts` | 繼承主 agent | 通用全工具 agent | 全部工具 |
| **explore** | `exploreAgent.ts` | Haiku (快速) | 唯讀探索 codebase | 只讀工具 |
| **plan** | `planAgent.ts` | 繼承主 agent | 架構規劃 | 全部工具 |
| **verification** | `verificationAgent.ts` | 繼承主 agent | 測試驗證 | 全部工具 |
| **claude-code-guide** | `claudeCodeGuideAgent.ts` | Haiku (快速) | Claude Code 文件查詢 | 只讀工具 |
| **statusline-setup** | `statuslineSetup.ts` | Sonnet | 狀態列設定 | 限定工具集 |

## Agent 特性詳解

### General-Purpose Agent
- 最通用的 subagent，繼承父 agent 的所有工具和模型
- 適用於需要完整能力的子任務

### Explore Agent
- 使用 Haiku 模型（成本低、速度快）
- 只有唯讀工具（Read、Grep、Glob、唯讀 Bash）
- 適合快速探索 codebase 而不修改任何東西

### Plan Agent
- 專門用於架構規劃和設計
- 有 92 行的詳細 prompt 指導規劃方法論
- 產出結構化的規劃文件

### Verification Agent
- 152 行 prompt 定義了嚴格的驗證合約
- **核心合約**：non-trivial implementation 必須經過獨立對抗性驗證
- FAIL → 修復 → 重新驗證 → 直到 PASS

### Claude Code Guide Agent
- 專門查詢 Claude Code 自身的使用文件
- 使用 Haiku 快速回答關於 Claude Code 功能的問題
- 205 行 prompt 包含豐富的使用指南

### Statusline Setup Agent
- 設定 terminal 狀態列顯示
- 使用 Sonnet 模型（需要更高理解能力）
- 144 行 prompt 包含各 terminal emulator 的設定方式

## 模型選擇策略

```
探索/查詢類 → Haiku（便宜、快速）
設定/修改類 → Sonnet（平衡）
繼承父能力 → inherit（通用/規劃/驗證）
```

→ 詳見 [[Agent 系統三層架構]]、[[Agent 生命週期]]、[[Model Selection 與成本路由]]

---

> [!tip] 導航
> 返回 [[Agent Architecture MOC]] · [[Claude Code 逆向工程知識庫]]
