---
aliases: [Harness Principles, Harness 12 原則]
tags: [design-pattern, harness-engineering]
source: "phase-09-harness-engineering/07-harness-design-principles.md"
created: 2025-04-05
---

# Harness Engineering 12 原則

> 從 Claude Code 原始碼提煉的 12 條可遷移 Harness Engineering 設計原則

## 原則 1：Context 管道化

**陳述**：System prompt 的組裝必須是明確的、分層的管道，而非單一模板
**Claude Code 實踐**：靜態/動態分隔、Registry 管理、cache-aware 組裝

→ 詳見 [[Context Engineering 多層管道]]

## 原則 2：多層工具執行管道

**陳述**：工具從呼叫到執行必須經過多層驗證管道
**Claude Code 實踐**：7 層防護（Schema → Validation → Sanitize → Hook → Permission → Execute → Post）

→ 詳見 [[工具執行多層防護管道]]

## 原則 3：Stop Reason 驅動迴圈

**陳述**：Agent Loop 的繼續/終止由 API 回傳的 stop_reason 決定，而非程式邏輯
**Claude Code 實踐**：`end_turn` 結束，`tool_use` 繼續

→ 詳見 [[Agent Loop 核心執行機制]]

## 原則 4：Feedback 即 Message

**陳述**：所有 feedback（工具結果、錯誤、診斷）都以 message 形式回注到對話
**Claude Code 實踐**：`tool_result` as user role message、diagnostic injection

## 原則 5：Cache 穩定性是一等工程需求

**陳述**：所有架構決策都必須考慮對 prompt cache 的影響
**Claude Code 實踐**：Dynamic Boundary、stripCacheControl()、Overage Latch

→ 詳見 [[Cache 穩定性工程模式]]

## 原則 6：安全採用 Fail-Closed

**陳述**：不確定性本身是安全訊號 — 無法解析 → 拒絕（非允許）
**Claude Code 實踐**：Tree-sitter 解析失敗 → ask、Deny-First

→ 詳見 [[Fail-Closed 與 Deny-First 原則]]

## 原則 7：工具偏好寫進 Prompt

**陳述**：工具使用偏好不能靠模型自行判斷 — 必須在 prompt 中明確聲明
**Claude Code 實踐**：偏好金字塔、When to Use / NOT to Use

→ 詳見 [[Tool Prompt 設計模式集]] 模式 1

## 原則 8：記憶只存非可推導資訊

**陳述**：記憶系統不存可從原始碼或環境推導的資訊
**Claude Code 實踐**：四種類型（user/feedback/project/reference）+明確排除清單

→ 詳見 [[Memory 設計原則集]]

## 原則 9：Bootstrap 狀態必須表達啟動階段

**陳述**：State 單例要能追蹤「系統在啟動過程的哪個階段」
**Claude Code 實踐**：startupTimers、Session 狀態機

→ 詳見 [[Bootstrap 啟動流程與生命週期]]

## 原則 10：遙測三層分離

**陳述**：業務事件、分散式追蹤、IDE 診斷必須是獨立的遙測層
**Claude Code 實踐**：Analytics（tengu_）、OpenTelemetry、DiagnosticTracking

→ 詳見 [[Observability 三層可觀測性架構]]

## 原則 11：PII 必須有型別系統保護

**陳述**：分析事件不能包含 PII — 用型別系統而非 code review 保護
**Claude Code 實踐**：`AnalyticsMetadata_I_VERIFIED_THIS_IS_NOT_CODE_OR_FILEPATHS`

→ 詳見 [[PII 安全型別系統模式]]

## 原則 12：Hook 系統為擴展而非修改

**陳述**：使用者自訂行為透過 Hook（Pre/Post）而非修改核心邏輯
**Claude Code 實踐**：PreToolUse / PostToolUse hooks in settings.json

→ 詳見 [[Hook 系統擴展模式]]

---

> [!tip] 導航
> 返回 [[Harness Engineering MOC]] · [[Claude Code 逆向工程知識庫]]
