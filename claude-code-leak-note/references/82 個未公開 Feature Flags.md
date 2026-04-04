---
aliases: [Feature Flags 清單, Hidden Features]
tags: [reference, special-features]
source: "phase-08-special-features/08-feature-flags.md, 10-unreleased-features-summary.md"
created: 2025-04-05
---

# 82 個未公開 Feature Flags

> 從 `bun:bundle` 的 `feature()` 系統和 GrowthBook/Statsig 整理的完整清單

## 兩層 Feature Flag 系統

### 編譯期 Feature Flags（`bun:bundle feature()`）

在 build 時靜態分析，未啟用的程式碼完全從 bundle 移除（Dead Code Elimination）。

### 運行期 Feature Flags（GrowthBook / Statsig）

動態 kill switch，可即時關閉 fleet 上的功能。以 `tengu_` 前綴命名。

## 重點未公開功能

### 多 Agent 系統

| Flag | 功能 | 狀態 |
|------|------|------|
| `COORDINATOR_MODE` | 主 agent 成為 coordinator，派遣 workers | 完整實作 |
| `FORK_SUBAGENT` | Agent fork 語義（繼承 context + cache）| 完整實作 |
| `UDS_INBOX` | Unix Domain Socket 跨 session 通訊 | 完整實作 |

→ 詳見 [[Coordinator Mode 多 Agent 協調]]

### 主動模式

| Flag | 功能 | 狀態 |
|------|------|------|
| `KAIROS` | 主動模式（持續監控並主動行動）| 完整實作 |
| `KAIROS_BRIEF` | KAIROS 簡報模式 | 完整實作 |

### 電腦控制

| Flag | 功能 | 狀態 |
|------|------|------|
| `COMPUTER_USE` | 代號 Chicago — macOS 螢幕控制 | 完整實作 |

→ 詳見 [[Computer Use 電腦控制整合]]

### 記憶系統

| Flag | 功能 |
|------|------|
| `tengu_passport_quail` | ExtractMemories 啟用 |
| `tengu_session_memory` | Session Memory 啟用 |
| `tengu_herring_clock` | Team Memory 啟用 |
| `tengu_onyx_plover` | AutoDream 啟用 + 排程 |
| `tengu_moth_copse` | skipIndex 模式 |
| `tengu_bramble_lintel` | ExtractMemories 節流 |

→ 詳見 [[Memory 五大子系統架構]]

### 成本/性能

| Flag | 功能 |
|------|------|
| `tengu_1h_cache_ttl` | 1 小時 prompt cache TTL |
| `CACHED_MC` | Cached Microcompact |
| `MONITOR_TOOL` | Monitor tool 替代 sleep |

→ 詳見 [[Prompt Cache 策略與 Break Detection]]

### 特殊功能

| Flag | 功能 |
|------|------|
| `VOICE_MODE` | 語音互動模式 |
| `BUDDY` | Buddy AI 寵物系統 |
| `ULTRAPLAN` | 遠端 Opus 規劃 |
| `PLUGINS` | Plugin marketplace |
| `tengu_scratch` | Scratchpad 跨 worker 共享目錄 |

→ 詳見 [[Buddy AI 寵物系統]]、[[UltraPlan 遠端規劃機制]]、[[Voice 語音系統與 Plugin 架構]]

## Feature Flag 設計模式

> [!info] 兩層設計的意義
> - **編譯期**：確保未啟用功能的程式碼不進入 production bundle（安全 + 效能）
> - **運行期**：允許即時 kill switch 而不需重新部署（靈活性）
> - 兩者配合實現「完整實作但可控釋出」的開發模式

→ 詳見 [[Beta Features 與 Feature Flags 系統]]、[[Tool Prompt 設計模式集]]

---

> [!tip] 導航
> 返回 [[Claude Code 逆向工程知識庫]]
