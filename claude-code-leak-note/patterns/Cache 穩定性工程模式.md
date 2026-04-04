---
aliases: [Cache Stability, Cache 穩定性, Sticky Latch]
tags: [design-pattern, cost-engineering, harness-engineering]
source: "phase-09-harness-engineering/02-context-engineering.md, phase-10-cost-quota/03-prompt-caching-strategy.md"
created: 2025-04-05
---

# Cache 穩定性工程模式

> 跨領域的 Prompt Cache 穩定性設計模式

## 核心問題

Prompt Cache hit 節省 ~90% input cost，cache write 比正常貴 25%。Cache break 不僅浪費之前的 cache write 投資，還需要重新寫入 → **雙重成本損失**。

## 模式 1：Dynamic Boundary（靜態/動態分隔）

```
[靜態部分 — scope: 'global' 緩存]
__SYSTEM_PROMPT_DYNAMIC_BOUNDARY__
[動態部分 — per-session/per-user]
```

靜態部分所有用戶共享同一 hash → hit rate ~100%。

→ 詳見 [[System Prompt 動態組裝邏輯]]

## 模式 2：條件維度分離

**問題**：10 個 boolean 條件在靜態部分 → 2^10 = 1024 種 hash
**解法**：條件內容移到動態邊界之後 → 靜態部分只有 1 個 hash

## 模式 3：stripCacheControl()

```typescript
// 移除 cache_control 後再計算 hash
// cache 設定變更不影響「內容是否相同」
const hash = computeHash(stripCacheControl(content))
```

## 模式 4：Overage State Latching（Sticky Latch）

```
Normal → Overage → [LATCH] → 即使恢復 Normal，也保持 Overage 設定
```

計費狀態影響 prompt 內容。來回切換 → 反覆 cache break。Latch 透過「只進不出」解決。

→ 詳見 [[Prompt Cache 策略與 Break Detection]]

## 模式 5：Agent 列表 Attachment 注入

**問題**：Agent 列表放在 tool schema → MCP/plugin 變更導致 tool schema cache bust
**解法**：Agent 列表改為 messages 中的 system-reminder 注入

**效果**：節省 ~10.2% fleet cache_creation tokens

→ 詳見 [[AgentTool 與 Subagent 派遣]]

## 模式 6：路徑正規化（Sandbox Temp Dir）

```typescript
// 正規化 $TMPDIR 路徑
// 避免 per-uid 路徑不同造成 prompt 差異
const path = (p === claudeTempDir) ? '$TMPDIR' : p
```

→ 詳見 [[Sandbox 沙箱隔離機制]]

## 模式 7：Cache 降級通知

預期的 cache 失效（deletion、compaction）不算 cache break：

```typescript
notifyDeletion()   // messages 刪除 → 標記為預期
notifyCompaction() // context 壓縮 → 標記為預期
```

→ 詳見 [[Prompt Cache 策略與 Break Detection]]

## 設計原則總結

> [!tip] Cache 穩定性 Checklist
> - [ ] 靜態/動態是否有明確分隔？
> - [ ] 條件分支是否在動態邊界之後？
> - [ ] 計費/狀態變更是否用 Latch 保護？
> - [ ] 動態列表是否從 schema 移到 messages？
> - [ ] 路徑是否正規化？
> - [ ] 預期的 cache break 是否已標記？

---

> [!tip] 導航
> 返回 [[Cost Engineering MOC]] · [[Harness Engineering MOC]] · [[Claude Code 逆向工程知識庫]]
