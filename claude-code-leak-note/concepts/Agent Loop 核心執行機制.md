---
aliases: [Agent Loop, 主迴圈, query loop]
tags: [concept, harness-engineering, agent-architecture]
source: "phase-09-harness-engineering/01-agent-loop-analysis.md"
created: 2025-04-05
---

# Agent Loop 核心執行機制

## 概述

Agent Loop 是 Claude Code 的核心驅動機制。它不是簡單的問答循環，而是帶有完整狀態管理、工具執行緩衝、feedback 注入的有狀態執行機。

## 整體流程

```mermaid
flowchart TD
    INPUT[用戶輸入] --> PREP["1. System Prompt 組裝\n2. Messages 正規化\n3. Prompt Cache 對齊"]
    PREP -->|Context Engineering| QUERY["4. queryModel()\nstreaming 接收回應"]
    QUERY -->|claude.ts| STOP{stop_reason?}
    STOP -->|end_turn| OUTPUT[回應輸出]
    STOP -->|tool_use| TOOLS["5. runTools()\n並行/串行策略\n權限檢查 + 安全驗證"]
    TOOLS -->|Tool Orchestration| FEEDBACK["6. tool_result 回注 messages"]
    FEEDBACK -->|Feedback Loop| QUERY
```

## 關鍵入口點

### `queryModel()`
- 位於 `src/services/api/claude.ts`（3419 行）
- 負責組裝 API 請求、streaming 處理、cache header 管理
- 回傳 `stop_reason`：`end_turn`（完成）或 `tool_use`（需要工具）

### `runTools()`
- 位於 `src/services/tools/toolOrchestration.ts`
- 決定並行或串行執行策略
- 走完多層安全防護管道後執行工具

→ 詳見 [[Tool Orchestration 調度系統]]

## Feedback Loop 機制

Agent Loop 的核心價值在於 feedback loop：

1. **工具結果回注**：tool_result 作為 `user` role 訊息回注到 messages
2. **診斷注入**：IDE 的 LSP diagnostics 在檔案編輯後注入
3. **Interrupt 處理**：用戶中斷、token 超限、權限拒絕都能優雅處理
4. **Context 壓縮**：對話過長時觸發 [[Context Compaction 壓縮策略]]

## 中斷機制

| 中斷類型 | 觸發條件 | 處理方式 |
|----------|---------|---------|
| 用戶中斷 | Ctrl+C / ESC | 優雅停止，保留已有結果 |
| Token 超限 | context window 接近上限 | 觸發 compaction |
| 權限拒絕 | 用戶拒絕工具權限 | tool_result 回傳錯誤，模型重新決策 |
| API 錯誤 | 429 / 529 / 網路錯誤 | 指數退避重試 |

## 設計觀察

> [!info] 關鍵設計決策
> - **Streaming**：回應以 streaming 方式接收，提供即時反饋
> - **Async Generator**：整個 loop 使用 async generator 模式，允許中間狀態逐步 yield
> - **Stop Reason 驅動**：loop 的繼續/終止完全由 API 回傳的 stop_reason 決定
> - **Stateful**：每個 turn 的 messages 累積形成完整的對話歷史

## 關聯筆記

- [[Context Engineering 多層管道]] — Agent Loop 的「輸入準備」階段
- [[Tool Orchestration 調度系統]] — Agent Loop 的「工具執行」階段
- [[Observability 三層可觀測性架構]] — Agent Loop 每個階段的追蹤
- [[Harness Engineering 定義與公式]] — Agent Loop 是 Harness 的 Action Interface

---

> [!tip] 導航
> 返回 [[Harness Engineering MOC]] · [[Claude Code 逆向工程知識庫]]
