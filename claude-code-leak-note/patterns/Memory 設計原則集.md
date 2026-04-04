---
aliases: [Memory Principles, 記憶設計原則]
tags: [design-pattern, memory-system]
source: "phase-05-memory-context/09-memory-design-principles.md"
created: 2025-04-05
---

# Memory 設計原則集

> 從 Claude Code 記憶系統提煉的 12 條設計原則

## 原則 1：只存非可推導資訊

記憶系統不存可從原始碼、git 歷史、環境推導的資訊。防止記憶膨脹。

**允許**：用戶偏好、回饋、專案目標
**禁止**：架構慣例（可從 code 推導）、git 歷史（可查詢）

## 原則 2：記憶是時間點觀察

記憶是「某時刻的觀察」而非「即時狀態」。模型應理解記憶可能過時。

## 原則 3：Prompt 措辭/位置影響行為

同樣的語意，不同的措辭和在 prompt 中的位置會產生不同效果。需要 A/B 測試驗證。

## 原則 4：增量記憶處理（Cursor 機制）

只處理上次提取之後的新訊息，而非每次重新分析整個對話。O(new) 而非 O(total)。

→ 詳見 [[ExtractMemories 自動記憶提取]]

## 原則 5：背景子系統與主代理互斥

記憶寫入遵循「誰先寫誰優先，另一方退出」。防止主代理和背景子系統雙重寫入。

→ 詳見 [[ExtractMemories 自動記憶提取]]

## 原則 6：工具權限最小化

每個記憶子系統只獲得最少需要的工具。ExtractMemories 只能寫 memory 目錄，不能 grep 原始碼。

→ 詳見 [[SkillTool 與 Skills 系統]] 的 allowedTools

## 原則 7：Prompt Cache 共享

背景子系統使用 `runForkedAgent()` 繼承父對話的 system prompt，最大化 cache 複用。

→ 詳見 [[Cache 穩定性工程模式]]

## 原則 8：避免不必要的探索

明確告知模型目錄已存在、不需要 ls、不需要 grep 驗證。每個多餘的工具呼叫都浪費 turn 和 token。

```
DIR_EXISTS_GUIDANCE: "This directory already exists — write to it directly"
```

## 原則 9：品質優於數量

寧可記少一點但品質高的記憶，也不要大量低品質的記憶。記憶太多 → 注入 system prompt 太長 → 消耗 context。

## 原則 10：優雅失敗設計

記憶失敗不應影響主流程。ExtractMemories 失敗 → 記 log、下次重試。AutoDream 失敗 → 回滾鎖的 mtime。Team Memory 永久失敗 → 抑制重試。

→ 詳見 [[Team Memory 跨用戶共享]]

## 原則 11：記憶信任層級遞減

```
CLAUDE.md（用戶明確寫入）> MEMORY.md（AI 提取）> Session Memory（壓縮快照）
```

信任度隨自動化程度遞減。

## 原則 12：四類型封閉分類

記憶只有 4 種類型：`user`、`feedback`、`project`、`reference`。封閉分類防止類型膨脹。

> [!info] feedback 同時記正反面
> 不只記「不要這樣做」，也記「繼續這樣做」。單向糾正會讓模型逐漸偏離已確認有效的方式。

→ 詳見 [[Memory 五大子系統架構]]

---

> [!tip] 導航
> 返回 [[Memory & Context MOC]] · [[Claude Code 逆向工程知識庫]]
