---
aliases: [Implementation MOC, 實作 MOC, impl-moc]
tags: [moc, implementation-reference]
created: 2026-04-06
---

# Implementation Reference MOC

> 基於 [claw-code](https://github.com/ultraworkers/claw-code) Python porting workspace 的最小實作參考索引。每篇筆記對應一個 claw-code 模組，提供程式碼、白話解釋、與概念筆記的雙向連結。

---

## 📊 Dataview 自動索引

```dataview
TABLE
  related-concept AS "對應概念",
  claw-code-source AS "claw-code 源碼",
  claw-code-lines AS "行數",
  implementation-status AS "狀態"
FROM "implementations"
WHERE contains(tags, "claw-code")
SORT domain, file.name
```

---

## 📋 手動索引（Dataview fallback）

### Harness Engineering

- [[runtime-implementation]] — PortRuntime：Agent Loop 最小實作（`runtime.py`, 200 行）
- [[query-engine-implementation]] — QueryEnginePort：查詢引擎（`query_engine.py`, 180 行）
- [[bootstrap-graph-implementation]] — Bootstrap 圖譜（`bootstrap_graph.py`）⏳
- [[system-init-implementation]] — 系統初始化（`system_init.py`）⏳
- [[context-implementation]] — Context 管道（`context.py`）⏳
- [[session-store-implementation]] — Session 持久化（`session_store.py`）⏳
- [[transcript-implementation]] — Transcript 追蹤（`transcript.py`）⏳
- [[history-implementation]] — 歷史紀錄（`history.py`）⏳

### Tool System

- [[tool-implementation]] — ToolDefinition：工具基礎定義（`Tool.py`, 15 行）
- [[tools-implementation]] — Tools Registry：工具註冊表與篩選（`tools.py`, 90 行）
- [[tool-pool-implementation]] — ToolPool：工具池組裝（`tool_pool.py`, 38 行）
- [[execution-registry-implementation]] — 執行註冊表（`execution_registry.py`）⏳

### Security & Permissions

- [[permissions-implementation]] — 權限過濾（`permissions.py`）⏳

### Cost Engineering

- [[cost-tracker-implementation]] — 成本追蹤（`cost_tracker.py`）⏳
- [[cost-hook-implementation]] — 成本 Hook（`costHook.py`）⏳

### Agent Architecture

- [[task-implementation]] — Task 定義（`task.py`）⏳
- [[tasks-implementation]] — Tasks 管理（`tasks.py`）⏳

### Memory & Context

- [[models-implementation]] — 資料模型（`models.py`）⏳

### CLI & UI

- [[command-graph-implementation]] — 命令圖譜（`command_graph.py`）⏳
- [[commands-implementation]] — 命令註冊表（`commands.py`）⏳
- [[direct-modes-implementation]] — 直接模式（`direct_modes.py`）⏳
- [[dialog-launchers-implementation]] — 對話框啟動器（`dialogLaunchers.py`）⏳
- [[interactive-helpers-implementation]] — 互動輔助（`interactiveHelpers.py`）⏳
- [[ink-implementation]] — Ink 渲染層（`ink.py`）⏳
- [[main-implementation]] — 主入口（`main.py`）⏳
- [[repl-launcher-implementation]] — REPL 啟動器（`replLauncher.py`）⏳
- [[query-implementation]] — 查詢介面（`query.py`）⏳

### Infrastructure

- [[setup-implementation]] — 環境設定（`setup.py`）⏳
- [[remote-runtime-implementation]] — 遠端 Runtime（`remote_runtime.py`）⏳
- [[prefetch-implementation]] — 預取策略（`prefetch.py`）⏳
- [[deferred-init-implementation]] — 延遲初始化（`deferred_init.py`）⏳
- [[parity-audit-implementation]] — Parity 審計（`parity_audit.py`）⏳
- [[port-manifest-implementation]] — 移植清冊（`port_manifest.py`）⏳
- [[project-onboarding-state-implementation]] — 專案上線狀態（`projectOnboardingState.py`）⏳

### 子資料夾模組（subfolders/）

→ 見 `implementations/subfolders/` 或使用 Dataview 查詢篩選

> ⏳ = 尚未建立，預定於後續 Phase 完成

---

## 統計

| 狀態 | 數量 |
|------|------|
| ✅ 已建立 | 5 |
| ⏳ 待建立（根層級）| 29 |
| ⏳ 待建立（子資料夾）| 30 |
| **總計** | **64** |

---

## 關聯 MOC

- [[Claude Code 逆向工程知識庫]] — Home MOC
- [[Harness Engineering MOC]] — 核心 Harness 概念
- [[Tool System MOC]] — 工具系統概念
- [[Security & Permissions MOC]] — 安全概念
- [[Cost Engineering MOC]] — 成本概念
- [[Agent Architecture MOC]] — Agent 架構概念
- [[Memory & Context MOC]] — 記憶系統概念
- [[Prompt Engineering MOC]] — Prompt 概念

## 參考

- [[claw-code 模組對照表]] — claw-code ↔ concept ↔ TypeScript 三向對照

---

> [!tip] 導航
> 返回 [[Claude Code 逆向工程知識庫]]
