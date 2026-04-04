---
aliases: [Bash Security, AST Parse, bashSecurity]
tags: [concept, security]
source: "phase-06-security-permissions/02-bash-security-rules.md"
created: 2025-04-05
---

# Bash 命令安全過濾與 AST 解析

## 概述

Claude Code 使用 Tree-sitter 對 bash 命令進行 AST（抽象語法樹）解析，比傳統的字串匹配或正則表達式更準確地理解命令結構。這是 [[七層縱深防禦模型]] 的 Layer 2-3。

## Layer 2: Tree-sitter AST 解析

### 解析結果分類

| 結果 | 含義 | 處理 |
|------|------|------|
| `simple` | 命令結構清晰，可靜態分析 | 繼續後續檢查 |
| `too-complex` | 包含命令替換、控制流等 | **強制 ask** |
| `semantic-fail` | eval、危險 builtins | **ask** |

### too-complex 觸發條件
- 命令替換：`$(...)` 或 `` `...` ``
- 控制流：複雜的 if/for/while
- 無法靜態分析的動態模式

## Layer 3: 23 個靜態 Validators

分為兩大類：

### Misparsing Validators（語法差異攻擊）

偵測不同 parser 對同一輸入的解釋差異：

| 攻擊 | shell-quote 看到 | bash 看到 | 風險 |
|------|-----------------|-----------|------|
| `TZ=UTC\recho cmd` | `TZ=UTC` + `echo cmd` | `TZ=UTC\recho` + `cmd` | 執行 `cmd` |
| `cat x \; echo /etc/passwd` | `cat x ; echo ...` | `cat x` + `\; echo ...` | 路徑驗證失效 |
| `echo\ test/../../../usr/bin/touch` | `echo test/.../touch` | `/usr/bin/touch` | 路徑遍歷 |

→ 詳見 [[Security 設計模式集]] 模式 5（Parser Differential 防禦）

### Non-Misparsing Validators

| Validator | 偵測目標 |
|-----------|---------|
| `validateDangerousPatterns` | `$()`, backtick, process substitution |
| `validateRedirections` | 危險的重導向（覆寫系統檔案）|
| `validateBraceExpansion` | Brace expansion 攻擊 |
| `validateMidWordHash` | `'x'#comment` 混淆 |
| `validateEncoding` | 非 ASCII 字元混淆 |

## 多版本 Quote Tracker

同一命令需要多個「視角」來偵測不同攻擊：

```typescript
type ValidationContext = {
  originalCommand: string         // 原始命令
  unquotedContent: string         // 保留雙引號內容
  fullyUnquotedContent: string    // 完全去引號
  fullyUnquotedPreStrip: string   // 去引號不 strip redirections
  unquotedKeepQuoteChars: string  // 去內容保留引號符
}
```

每個 validator 使用最適合的「視角」：
- `validateDangerousPatterns` → `unquotedContent`（雙引號內的 `$()` 也危險）
- `validateRedirections` → `fullyUnquotedContent`
- `validateMidWordHash` → `unquotedKeepQuoteChars`

→ 詳見 [[Security 設計模式集]] 模式 6

## Fail-Closed 原則

> [!important] 核心原則
> 解析失敗 ≠ 安全。不確定性本身就是安全訊號。
> ```
> if (parseResult.kind === 'too-complex') → ask
> if (subcommands.length > MAX_SUBCOMMANDS) → ask
> ```

## 關聯筆記

- [[七層縱深防禦模型]] — Layer 2-3
- [[BashTool 深度剖析]] — BashTool 的完整安全架構
- [[Security 設計模式集]] — 模式 2、5、6
- [[Fail-Closed 與 Deny-First 原則]] — 跨領域的安全原則

---

> [!tip] 導航
> 返回 [[Security & Permissions MOC]] · [[Claude Code 逆向工程知識庫]]
