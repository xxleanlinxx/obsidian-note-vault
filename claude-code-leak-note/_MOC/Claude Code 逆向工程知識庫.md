---
aliases: [主 MOC, Home, 知識庫首頁]
tags: [moc]
created: 2025-04-05
---

# Claude Code 逆向工程知識庫

> 基於 Claude Code v2.1.88 原始碼（1,884 檔案 / ~92,500 行 TypeScript）的深度逆向分析
> 75 份分析報告 → 74 篇互連原子筆記

---

## 🗺️ 領域導航

| MOC | 概念筆記 | 模式筆記 | 核心主題 |
|-----|---------|---------|---------|
| [[Harness Engineering MOC]] | 7 篇 | 4 篇 | Agent Loop、Context Engineering、Tool Orchestration |
| [[Prompt Engineering MOC]] | 4 篇 | 1 篇 | System Prompt 組裝、Compaction、輔助 Prompt |
| [[Tool System MOC]] | 6 篇 | 2 篇 | 36 工具、BashTool、Skills vs Tools |
| [[Agent Architecture MOC]] | 6 篇 | 1 篇 | 三層架構、Coordinator、Swarm |
| [[Security & Permissions MOC]] | 5 篇 | 2 篇 | 七層防禦、AST 解析、權限引擎 |
| [[Memory & Context MOC]] | 7 篇 | 1 篇 | 五大子系統、Memdir、AutoDream |
| [[Cost Engineering MOC]] | 6 篇 | 1 篇 | 成本追蹤、Cache 策略、Rate Limiting |

---

## 🔑 十大關鍵發現

→ 完整版見 [[十大關鍵發現]]

1. [[Agent Loop 核心執行機制|Agent Loop]] 是精密的有狀態機
2. [[Context Engineering 多層管道|Context Engineering]] 比 Prompt Engineering 更重要
3. 安全架構採用 [[七層縱深防禦模型|七層縱深防禦]]
4. 記憶系統有 [[Memory 五大子系統架構|五個獨立子系統]] 並存
5. [[82 個未公開 Feature Flags]] 揭示產品路線圖
6. [[Coordinator Mode 多 Agent 協調|Coordinator]]/[[Swarm 與 Teammate 多 Agent 協作|Swarm]] 是真正的多 Agent 系統
7. [[成本追蹤架構|成本追蹤]] 達到毫秒級精度
8. [[SkillTool 與 Skills 系統|Skills 系統]] 實現可程式化的 prompt 擴充
9. Prompt 設計有 [[Prompt Engineering 設計模式集|系統化的工程模式]]
10. 系統為企業場景做了 [[Policy Limits 團隊管控|深度設計]]

---

## 📐 跨領域設計模式

| 模式 | 領域 |
|------|------|
| [[Cache 穩定性工程模式]] | Cost + Harness |
| [[並行與 Async Generator 模式]] | Harness + Agent |
| [[Fail-Closed 與 Deny-First 原則]] | Security + Harness |
| [[PII 安全型別系統模式]] | Harness + Observability |
| [[Hook 系統擴展模式]] | Tool + Security |

---

## 📚 參考索引

- [[36 工具完整索引表]] — 所有工具的分類與說明
- [[6 Built-in Agents 索引]] — 內建 Agent 的模型與工具集
- [[16 Bundled Skills 目錄]] — 內建技能的功能與條件
- [[82 個未公開 Feature Flags]] — 隱藏功能完整清單
- [[Codebase 結構對照表]] — 目錄 → 功能對應
- [[外部參考資料集]] — Anthropic 官方、社群、學術資源

---

## 🏗️ Harness Engineering 公式

> **Harness = Tools + Knowledge + Observation + Action Interfaces + Permissions**

→ 詳見 [[Harness Engineering 定義與公式]]

---

## 筆記統計

| 類別 | 數量 |
|------|------|
| MOC（導航頁）| 8 |
| 概念筆記 | 48 |
| 設計模式筆記 | 10 |
| 參考索引筆記 | 7 |
| **總計** | **73** |
