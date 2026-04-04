---
aliases: [Session Memory, Session 記憶, 即時快照]
tags: [concept, memory-system]
source: "phase-05-memory-context/05-session-memory.md"
created: 2025-04-05
---

# Session Memory 即時快照

## 概述

Session Memory 在 context 達到閾值時保存當前 session 的筆記快照，補充 [[ExtractMemories 自動記憶提取|ExtractMemories]] 的增量記憶。

## 觸發時機

- Context token 使用量達到預設閾值
- Feature flag `tengu_session_memory` 啟用

## 存儲位置

```
~/.claude/session-memory/<session-id>.md
```

## Prompt 特性

### Mustache 模板變數

```typescript
function substituteVariables(template, variables) {
  return template.replace(/\{\{(\w+)\}\}/g, (match, key) =>
    Object.prototype.hasOwnProperty.call(variables, key)
      ? variables[key]!
      : match  // 找不到 key → 保留原始 {{variable}}
  )
}
```

- Single-pass 替換防止 `$` 反向引用問題
- 找不到的變數保留原始 `{{...}}`（防止靜默失敗）

→ 詳見 [[Prompt Engineering 設計模式集]] 模式 13

### 容量感知動態警告

```typescript
if (overBudget) {
  `CRITICAL: The session memory file is currently ~${totalTokens} tokens,
   which exceeds the maximum of ${MAX_TOTAL_SESSION_MEMORY_TOKENS} tokens.
   You MUST condense the file to fit within this budget.`
}
```

- 根據「當前狀態有多糟糕」動態調整警告嚴重程度
- 超限 → CRITICAL；個別 section 超限 → IMPORTANT

→ 詳見 [[Prompt Engineering 設計模式集]] 模式 14

### 用戶可自訂 Prompt

```
~/.claude/session-memory/config/prompt.md
```

用戶可覆蓋預設的 session memory prompt。

## 工具權限

只允許 Edit 指定文件（最小權限原則）。

## 與 Context Compaction 的互補

| 機制 | 保留粒度 | 持久性 |
|------|---------|--------|
| **Session Memory** | 高（結構化筆記）| 跨 session |
| **Context Compaction** | 中（摘要）| 僅當次 session |

Session Memory 在 compaction 發生前保存更詳細的筆記，確保重要資訊不因壓縮而丟失。

## 關聯筆記

- [[Memory 五大子系統架構]] — 在記憶體系中的位置
- [[Context Compaction 壓縮策略]] — 互補的 context 管理
- [[Memory 設計原則集]] — 設計原則
- [[Prompt Engineering 設計模式集]] — 模式 13、14

---

> [!tip] 導航
> 返回 [[Memory & Context MOC]] · [[Claude Code 逆向工程知識庫]]
