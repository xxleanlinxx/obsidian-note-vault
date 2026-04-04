---
aliases: [Voice Mode, 語音系統, Plugin System]
tags: [concept, special-features]
source: "phase-08-special-features/04-voice-system.md, 06-plugin-system.md"
created: 2025-04-05
---

# Voice 語音系統與 Plugin 架構

## Voice 語音系統

### 概述

Voice Mode 讓用戶透過語音與 Claude Code 互動，支援語音輸入和語音輸出。

### 啟用方式

```typescript
if (feature('VOICE_MODE')) {
  // 載入語音系統
}
```

### 架構

```mermaid
flowchart LR
    VOICE["用戶語音"] --> STT["STT\nSpeech-to-Text"]
    STT --> TEXT_IN["文字輸入"]
    TEXT_IN --> AGENT["Agent Loop\n正常處理"]
    AGENT --> TEXT_OUT["文字輸出"]
    TEXT_OUT --> TTS["TTS\nText-to-Speech"]
    TTS --> AUDIO["語音輸出"]
```

### 適用場景

- 手不方便時（walking、eating）
- 需要口頭說明複雜需求
- 程式碼 review 時邊說邊看

---

## Plugin 架構

### 概述

Plugin 系統允許第三方擴充 Claude Code 的能力，類似 VS Code 的 extension marketplace。

### 啟用方式

```typescript
if (feature('PLUGINS')) {
  // 載入 Plugin marketplace
}
```

### Plugin 能力

| 能力 | 說明 |
|------|------|
| **新增工具** | Plugin 可提供自訂工具 |
| **新增 Skills** | Plugin 可提供 Skill 模板 |
| **UI 擴充** | Plugin 可在 UI 中加入新元素 |
| **Event Hook** | Plugin 可監聽系統事件 |

### 與 MCP 的區別

| 特性 | MCP | Plugin |
|------|-----|--------|
| 協議 | MCP 標準 | 自訂 |
| 範圍 | 工具提供 | 完整擴充 |
| 發現 | 手動設定 | **Marketplace** |
| 安全 | MCP 權限模型 | Plugin 沙箱 |

## 關聯筆記

- [[82 個未公開 Feature Flags]] — `VOICE_MODE`、`PLUGINS` flags
- [[36 工具系統總覽]] — Plugin 可擴充工具
- [[SkillTool 與 Skills 系統]] — Plugin 可提供 Skills

---

> [!tip] 導航
> 返回 [[Claude Code 逆向工程知識庫]]
