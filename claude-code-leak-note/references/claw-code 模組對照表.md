---
aliases: [claw-code Mapping, 模組對照, claw-code Reference]
tags: [reference, claw-code]
source: "ultraworkers/claw-code"
created: 2026-04-06
---

# claw-code 模組對照表

> [claw-code](https://github.com/ultraworkers/claw-code) 是 Claude Code 的 Python-first porting workspace，提供極簡的參考實作。本表對照 claw-code Python 模組、Claude Code TypeScript 原始碼、與本 vault 的概念/實作筆記。

---

## 📊 Dataview 自動對照表

```dataview
TABLE without id
  file.link AS "實作筆記",
  claw-code-source AS "claw-code",
  related-concept AS "概念",
  claude-code-equivalent AS "Claude Code TS",
  implementation-status AS "狀態"
FROM "implementations"
WHERE contains(tags, "claw-code")
SORT claw-code-source
```

---

## 📋 完整對照表（Dataview fallback）

### Harness Engineering

| claw-code 檔案 | 對應概念筆記 | TypeScript 原始碼 | 實作筆記 |
|---------------|------------|-----------------|---------|
| `src/runtime.py` (200 行) | [[Agent Loop 核心執行機制]] | `src/main.tsx` | [[runtime-implementation]] |
| `src/query_engine.py` (180 行) | [[Agent Loop 核心執行機制]] | `src/services/api/claude.ts` | [[query-engine-implementation]] |
| `src/context.py` | [[Context Engineering 多層管道]] | `src/services/api/claude.ts` | [[context-implementation]] ⏳ |
| `src/session_store.py` | [[Session Memory 即時快照]] | `src/services/SessionMemory/` | [[session-store-implementation]] ⏳ |
| `src/transcript.py` | [[Observability 三層可觀測性架構]] | `src/services/analytics/` | [[transcript-implementation]] ⏳ |
| `src/history.py` | [[Agent Loop 核心執行機制]] | `src/main.tsx` | [[history-implementation]] ⏳ |
| `src/bootstrap_graph.py` | [[Bootstrap 啟動流程與生命週期]] | `src/bootstrap/state.ts` | [[bootstrap-graph-implementation]] ⏳ |
| `src/system_init.py` | [[Bootstrap 啟動流程與生命週期]] | `src/bootstrap/state.ts` | [[system-init-implementation]] ⏳ |

### Tool System

| claw-code 檔案 | 對應概念筆記 | TypeScript 原始碼 | 實作筆記 |
|---------------|------------|-----------------|---------|
| `src/Tool.py` (15 行) | [[36 工具系統總覽]] | `src/tools/` | [[tool-implementation]] |
| `src/tools.py` (90 行) | [[36 工具系統總覽]] | `src/services/tools/toolExecution.ts` | [[tools-implementation]] |
| `src/tool_pool.py` (38 行) | [[Tool Orchestration 調度系統]] | `src/services/tools/toolOrchestration.ts` | [[tool-pool-implementation]] |
| `src/execution_registry.py` | [[工具執行多層防護管道]] | `src/services/tools/toolExecution.ts` | [[execution-registry-implementation]] ⏳ |

### Security & Permissions

| claw-code 檔案 | 對應概念筆記 | TypeScript 原始碼 | 實作筆記 |
|---------------|------------|-----------------|---------|
| `src/permissions.py` | [[權限規則引擎]] | `src/utils/permissions/` | [[permissions-implementation]] ⏳ |

### Cost Engineering

| claw-code 檔案 | 對應概念筆記 | TypeScript 原始碼 | 實作筆記 |
|---------------|------------|-----------------|---------|
| `src/cost_tracker.py` | [[成本追蹤架構]] | `src/cost-tracker.ts` | [[cost-tracker-implementation]] ⏳ |
| `src/costHook.py` | [[成本追蹤架構]] | `src/hooks/` | [[cost-hook-implementation]] ⏳ |

### Agent Architecture

| claw-code 檔案 | 對應概念筆記 | TypeScript 原始碼 | 實作筆記 |
|---------------|------------|-----------------|---------|
| `src/task.py` | [[Task 系統與狀態機]] | `src/tasks/` | [[task-implementation]] ⏳ |
| `src/tasks.py` | [[Task 系統與狀態機]] | `src/tasks/` | [[tasks-implementation]] ⏳ |

### Models & Data

| claw-code 檔案 | 對應概念筆記 | TypeScript 原始碼 | 實作筆記 |
|---------------|------------|-----------------|---------|
| `src/models.py` | [[模型配置與 Provider 支援]] | `src/utils/model/` | [[models-implementation]] ⏳ |

### CLI & UI

| claw-code 檔案 | 對應概念筆記 | TypeScript 原始碼 | 實作筆記 |
|---------------|------------|-----------------|---------|
| `src/command_graph.py` | — | `src/main.tsx` | [[command-graph-implementation]] ⏳ |
| `src/commands.py` | — | `src/main.tsx` | [[commands-implementation]] ⏳ |
| `src/direct_modes.py` | — | `src/main.tsx` | [[direct-modes-implementation]] ⏳ |
| `src/dialogLaunchers.py` | — | `src/screens/REPL.tsx` | [[dialog-launchers-implementation]] ⏳ |
| `src/interactiveHelpers.py` | — | `src/screens/REPL.tsx` | [[interactive-helpers-implementation]] ⏳ |
| `src/ink.py` | — | `src/screens/` | [[ink-implementation]] ⏳ |
| `src/main.py` | — | `src/main.tsx` | [[main-implementation]] ⏳ |
| `src/replLauncher.py` | — | `src/screens/REPL.tsx` | [[repl-launcher-implementation]] ⏳ |
| `src/query.py` | — | `src/services/api/claude.ts` | [[query-implementation]] ⏳ |
| `src/projectOnboardingState.py` | — | `src/bootstrap/` | [[project-onboarding-state-implementation]] ⏳ |

### Infrastructure & Porting

| claw-code 檔案 | 對應概念筆記 | TypeScript 原始碼 | 實作筆記 |
|---------------|------------|-----------------|---------|
| `src/setup.py` | [[Bootstrap 啟動流程與生命週期]] | `src/bootstrap/` | [[setup-implementation]] ⏳ |
| `src/remote_runtime.py` | — | `src/remote/` | [[remote-runtime-implementation]] ⏳ |
| `src/prefetch.py` | — | `src/services/api/` | [[prefetch-implementation]] ⏳ |
| `src/deferred_init.py` | — | `src/bootstrap/` | [[deferred-init-implementation]] ⏳ |
| `src/parity_audit.py` | — | — | [[parity-audit-implementation]] ⏳ |
| `src/port_manifest.py` | — | — | [[port-manifest-implementation]] ⏳ |

### 子資料夾模組（30 個）

| claw-code 目錄 | 模組用途 | 實作筆記 |
|---------------|---------|---------|
| `src/assistant/` | Assistant 子系統 placeholder | [[assistant-module]] ⏳ |
| `src/bootstrap/` | Bootstrap 子系統 | [[bootstrap-module]] ⏳ |
| `src/bridge/` | Bridge 橋接層 | [[bridge-module]] ⏳ |
| `src/buddy/` | Buddy AI 寵物 | [[buddy-module]] ⏳ |
| `src/cli/` | CLI 介面 | [[cli-module]] ⏳ |
| `src/components/` | UI 元件 | [[components-module]] ⏳ |
| `src/constants/` | 系統常數 | [[constants-module]] ⏳ |
| `src/coordinator/` | 多 Agent 協調 | [[coordinator-module]] ⏳ |
| `src/entrypoints/` | 進入點 | [[entrypoints-module]] ⏳ |
| `src/hooks/` | Hook 擴展 | [[hooks-module]] ⏳ |
| `src/keybindings/` | 快捷鍵 | [[keybindings-module]] ⏳ |
| `src/memdir/` | 記憶目錄 | [[memdir-module]] ⏳ |
| `src/migrations/` | 遷移腳本 | [[migrations-module]] ⏳ |
| `src/moreright/` | 額外權限 | [[moreright-module]] ⏳ |
| `src/native_ts/` | 原生 TS 橋接 | [[native-ts-module]] ⏳ |
| `src/outputStyles/` | 輸出樣式 | [[output-styles-module]] ⏳ |
| `src/plugins/` | Plugin 系統 | [[plugins-module]] ⏳ |
| `src/reference_data/` | 參考資料 | [[reference-data-module]] ⏳ |
| `src/remote/` | 遠端執行 | [[remote-module]] ⏳ |
| `src/schemas/` | Schema 定義 | [[schemas-module]] ⏳ |
| `src/screens/` | 畫面 | [[screens-module]] ⏳ |
| `src/server/` | Server 模組 | [[server-module]] ⏳ |
| `src/services/` | 服務層 | [[services-module]] ⏳ |
| `src/skills/` | Skills 系統 | [[skills-module]] ⏳ |
| `src/state/` | 狀態管理 | [[state-module]] ⏳ |
| `src/types/` | 型別定義 | [[types-module]] ⏳ |
| `src/upstreamproxy/` | 上游代理 | [[upstream-proxy-module]] ⏳ |
| `src/utils/` | 工具函式 | [[utils-module]] ⏳ |
| `src/vim/` | Vim 模式 | [[vim-module]] ⏳ |
| `src/voice/` | 語音系統 | [[voice-module]] ⏳ |

> ⏳ = 尚未建立，預定於後續 Phase 完成

---

## 統計

| 類別 | 已建立 | 待建立 | 總計 |
|------|--------|--------|------|
| 根層級實作筆記 | 5 | 29 | 34 |
| 子資料夾模組筆記 | 0 | 30 | 30 |
| **合計** | **5** | **59** | **64** |

---

## 關聯筆記

- [[Implementation Reference MOC]] — 實作筆記的 MOC 導航
- [[Codebase 結構對照表]] — Claude Code TypeScript 目錄結構
- [[Claude Code 逆向工程知識庫]] — Home MOC

---

> [!tip] 導航
> 返回 [[Implementation Reference MOC]] · [[Claude Code 逆向工程知識庫]]
