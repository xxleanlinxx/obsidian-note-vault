---
aliases: [PII Safety Type, PII 型別保護]
tags: [design-pattern, harness-engineering]
source: "phase-09-harness-engineering/04-observability-telemetry.md"
created: 2025-04-05
---

# PII 安全型別系統模式

> 用 TypeScript 型別系統防止 PII 洩漏到遙測資料

## 問題

分析事件（analytics events）傳送到外部服務（GrowthBook/Statsig）。如果事件中包含 PII（個人可識別資訊）、程式碼或檔案路徑，就構成隱私問題。

## 解法：自文件化型別名稱

```typescript
type AnalyticsMetadata_I_VERIFIED_THIS_IS_NOT_CODE_OR_FILEPATHS = {
  [key: string]: string | number | boolean | undefined
}
```

### 這個超長名稱的設計意圖

1. **開發者在寫程式碼時**：必須用這個型別標記分析資料
2. **Code Review 時**：reviewer 看到這個型別會立刻注意「這個資料真的不含 PII 嗎？」
3. **搜尋時**：全專案搜尋這個型別就能找到所有分析事件

### 效果

- 型別名稱本身就是一個 assertion
- 不需要 runtime 檢查
- Code review 自然聚焦到 PII 安全問題
- 違規在 TypeScript 編譯時就被捕獲

## 設計模式特徵

| 特徵 | 說明 |
|------|------|
| **零 Runtime 開銷** | 純型別系統，編譯後消失 |
| **自文件化** | 名稱即文件 |
| **強制 Review** | 無法忽略的超長名稱 |
| **可搜尋** | 全域搜尋即可審計 |

## 類似應用

- `DANGEROUS_uncachedSystemPromptSection` — 同樣的自文件化命名策略
- `SystemPrompt` Branded Type — 型別系統防誤用

## 適用場景

> [!tip] 何時使用此模式
> - 資料會離開系統邊界（外部 API、日誌服務）
> - PII 保護是合規要求
> - 團隊需要在 code review 中強制檢查
> - 希望零 runtime 開銷

## 關聯筆記

- [[Observability 三層可觀測性架構]] — 分析事件的 PII 保護
- [[Harness Engineering 12 原則]] — 原則 11
- [[Prompt Engineering 設計模式集]] — 模式 7（Branded Type）、模式 8（DANGEROUS_ 前綴）

---

> [!tip] 導航
> 返回 [[Harness Engineering MOC]] · [[Claude Code 逆向工程知識庫]]
