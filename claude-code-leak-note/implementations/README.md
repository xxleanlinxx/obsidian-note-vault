---
aliases: [Implementation Notes Guide, 實作筆記指南]
tags: [meta, implementation-reference]
created: 2026-04-06
---

# 實作參考筆記（implementations/）

> 本資料夾收錄基於 [claw-code](https://github.com/ultraworkers/claw-code) Python porting workspace 的最小實作參考筆記。每篇筆記對應一個 claw-code 模組，提供完整程式碼、白話解釋、與 Claude Code 概念筆記的對照分析。

## 資料夾結構

```
implementations/
├── README.md                    ← 本檔案
├── *-implementation.md          ← 根層級模組實作筆記（~34 篇）
└── subfolders/
    └── *-module.md              ← 子資料夾模組實作筆記（~30 篇）
```

## Block ID 命名慣例

每篇實作筆記使用 Obsidian block references 支援跨筆記嵌入。命名規則：

| Block ID | 用途 | 必要性 |
|----------|------|--------|
| `^code-full` | 完整程式碼 | 必要 |
| `^code-core` | 核心抽象段（供 concept note `![[...]]` embed） | **必要** |
| `^code-helper` | 輔助程式碼段 | 選用 |
| `^explanation-structure` | 白話解釋：資料結構 | 必要 |
| `^explanation-method` | 白話解釋：關鍵方法 | 視內容 |
| `^explanation-intent` | 白話解釋：設計意圖 | 必要 |
| `^design-choices` | 關鍵設計抉擇表 | 必要 |
| `^gap-analysis` | 精簡 vs 完整差距分析 | 必要 |

## Enhanced Frontmatter Schema

所有實作筆記的 frontmatter 遵循以下 schema：

```yaml
aliases: [模組 Python 名稱, English alias]
tags: [implementation-reference, claw-code, {domain}]
source: "claw-code:src/path/to/file.py"
complexity: minimal          # minimal | simplified | full
implementation-status: stub  # stub | partial | complete
domain: {domain-name}       # harness-engineering | tool-system | security |
                             # cost-engineering | agent-architecture |
                             # memory-context | prompt-engineering
claw-code-source: src/path/file.py
claw-code-lines: 42         # 檔案行數
claude-code-equivalent: src/typescript-path.ts
related-concept: "[[Concept Note Name]]"
related-pattern: "[[Pattern Note Name]]"
created: 2026-04-06
```

### 欄位說明

- **complexity**：`minimal`（10-50 行 stub）、`simplified`（簡化版）、`full`（完整移植）
- **implementation-status**：`stub`（骨幹）、`partial`（部分功能）、`complete`（完整）
- **domain**：對應 7 個 domain MOC 之一
- **related-concept**：用 `"[[...]]"` 格式，Dataview 可解析為連結

## 設計原則

1. **Single Source of Truth**：程式碼只存在本資料夾的筆記中，concept notes 用 `![[...#^code-core]]` embed 引用
2. **教學導向**：白話解釋闡明 stub 如何體現完整架構的核心抽象
3. **差距分析**：每篇明確說明 stub 捕捉了什麼、省略了什麼
4. **雙向連結**：每篇至少連結 1 個 concept note 和 1 個 MOC

---

> [!tip] 導航
> 返回 [[Implementation Reference MOC]] · [[claw-code 模組對照表]] · [[Claude Code 逆向工程知識庫]]
