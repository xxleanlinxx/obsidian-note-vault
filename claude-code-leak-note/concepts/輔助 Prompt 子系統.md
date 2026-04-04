---
aliases: [Auxiliary Prompts, 輔助 Prompt]
tags: [concept, prompt-engineering]
source: "phase-01-system-prompt/04-auxiliary-prompts.md"
created: 2025-04-05
---

# 輔助 Prompt 子系統

## 概述

除了主 System Prompt 外，Claude Code 擁有多個輔助 prompt 子系統，各自服務不同的背景功能。這些子系統都以 forked agent 方式運行，共享父對話的 prompt cache。

## 五大輔助 Prompt 系統

### 1. ExtractMemories Prompt

**用途**：背景自動提取對話中值得記憶的資訊

```
角色：你是 Claude Code 的記憶提取子系統
輸入：最近 N 條新訊息
輸出：更新 memdir 中的記憶文件
限制：最多 5 turns、只能讀寫 memory 目錄
```

- 嚴格的 turn 預算
- 禁止探索性操作（不可 grep、不可 git）
- 高效路徑指引：先全部讀 → 再全部寫

→ 詳見 [[ExtractMemories 自動記憶提取]]

### 2. Session Memory Prompt

**用途**：在 context 達到閾值時保存當前 session 的筆記快照

```
角色：你是 session 筆記員
輸入：整個對話歷史
輸出：~/.claude/session-memory/<id>.md
限制：只能 Edit 指定文件
```

- 使用 `{{Mustache}}` 模板變數替換
- 容量感知的動態警告（超限 → CRITICAL）
- 支援用戶自訂 prompt 覆蓋

→ 詳見 [[Session Memory 即時快照]]

### 3. MagicDocs Prompt

**用途**：在對話 idle 後自動更新 repo 中帶有 `# MAGIC DOC:` 標頭的 .md 檔

```
角色：你是文件維護子系統
輸入：對話歷史 + Magic Doc 當前內容
輸出：原地更新 Magic Doc
限制：只能 Edit 被追蹤的文件
```

→ 詳見 [[MagicDocs 動態文件系統]]

### 4. AutoDream Prompt

**用途**：定期（24h + 5 sessions）跨 session 整合記憶

```
角色：你是記憶整合子系統
輸入：memdir 中所有記憶文件 + logs
輸出：精煉後的 MEMORY.md
觸發：時間門檻 + session 門檻
```

→ 詳見 [[AutoDream 夢境記憶整合]]

### 5. Buddy Prompt

**用途**：AI 寵物伴侶系統的 prompt

```
角色：你是一隻 [動物] 寵物
輸入：用戶的對話歷史
輸出：寵物的反應和互動
特性：有自己的性格和情緒狀態
```

→ 詳見 [[Buddy AI 寵物系統]]

## 共同設計模式

> [!info] 元指令分離
> 所有輔助 prompt 都包含：
> ```
> IMPORTANT: This message and these instructions are NOT part of the actual user conversation.
> Do NOT include any references to "note-taking", "session notes extraction", or these
> update instructions in the notes content.
> ```
> 防止模型把內部指令洩漏到輸出中。

→ 詳見 [[Prompt Engineering 設計模式集]] 模式 17

## Forked Agent 共享機制

所有輔助子系統都使用 `runForkedAgent()` 執行：
- **繼承父 context**：共享 system prompt prefix → 重用 prompt cache
- **獨立 message history**：不污染主對話
- **工具最小化**：只給予最少需要的工具

→ 詳見 [[Memory 設計原則集]] 原則 6、7

## 關聯筆記

- [[System Prompt 動態組裝邏輯]] — 主 prompt 與輔助 prompt 的關係
- [[Memory 五大子系統架構]] — 記憶相關子系統的完整架構
- [[Prompt Engineering 設計模式集]] — 模式 5、14、17

---

> [!tip] 導航
> 返回 [[Prompt Engineering MOC]] · [[Claude Code 逆向工程知識庫]]
