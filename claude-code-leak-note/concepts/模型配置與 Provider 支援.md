---
aliases: [Model Config, Provider Support, 模型配置]
tags: [concept, api-architecture]
source: "phase-07-api-model/02-model-config.md, 03-provider-support.md, 07-model-routing.md"
created: 2025-04-05
---

# 模型配置與 Provider 支援

## 概述

`src/utils/model/` 下 16 個檔案管理模型的完整配置，包括定價、能力特性、provider 路由。

## 模型配置結構

```typescript
type ModelConfig = {
  name: string
  contextWindow: number         // e.g., 200_000
  maxTokens: number             // e.g., 8192
  inputPrice: number            // $/million input tokens
  outputPrice: number           // $/million output tokens
  cacheReadPrice: number        // $/million cache read tokens
  cacheWritePrice: number       // $/million cache write tokens
  supportsExtendedThinking: boolean
  supportsVision: boolean
  supportsBetaFeatures: string[]
}
```

## 支援的 Provider

| Provider | 設定方式 | 特性 |
|----------|---------|------|
| **Anthropic Direct** | `ANTHROPIC_API_KEY` | 預設，完整功能 |
| **AWS Bedrock** | `AWS_REGION` + model ARN | Cross-region routing，企業合規 |
| **GCP Vertex AI** | `GOOGLE_PROJECT` + region | Region-based，企業合規 |
| **OpenAI-Compatible** | `OPENAI_BASE_URL` | 第三方 / 自建模型 |

## Provider 路由決策

```
if (ANTHROPIC_API_KEY) → Anthropic Direct
else if (AWS_REGION && model.includes('bedrock')) → AWS Bedrock
else if (GOOGLE_PROJECT) → GCP Vertex AI
else if (OPENAI_BASE_URL) → OpenAI-Compatible
else → Error: no provider configured
```

## Bedrock 特殊處理

```typescript
// Bedrock 使用 model ARN 而非 model name
// Cross-region routing 需要特殊 inference profile
const bedrockModelId = `arn:aws:bedrock:${region}:${accountId}:inference-profile/${modelName}`
```

## 模型能力特性矩陣

| 特性 | Haiku | Sonnet | Opus |
|------|-------|--------|------|
| Extended Thinking | ❌ | ✅ | ✅ |
| Vision | ✅ | ✅ | ✅ |
| Tool Use | ✅ | ✅ | ✅ |
| Prompt Cache | ✅ | ✅ | ✅ |
| Beta: computer_use | ❌ | ✅ | ❌ |

## 關聯筆記

- [[API 呼叫層架構]] — API 層使用模型配置
- [[Model Selection 與成本路由]] — 成本導向的模型選擇
- [[Policy Limits 團隊管控]] — 組織級模型限制
- [[Beta Features 與 Feature Flags 系統]] — Beta 功能與模型的關係

---

> [!tip] 導航
> 返回 [[Cost Engineering MOC]] · [[Claude Code 逆向工程知識庫]]
