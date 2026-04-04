---
aliases: [Security Patterns, 安全設計模式]
tags: [design-pattern, security]
source: "phase-06-security-permissions/08-security-design-patterns.md"
created: 2025-04-05
---

# Security 設計模式集

> 從 Claude Code 安全架構提煉的 10 個可遷移安全設計模式

## 模式 1：縱深防禦（Defense-in-Depth）

每一層假設其他層可能失效，獨立提供安全保障。Claude Code 實作了 7 層。

→ 詳見 [[七層縱深防禦模型]]

## 模式 2：Fail-Closed

不確定性 = 不安全。無法解析 → 拒絕（非允許）。

```
if (parseResult === 'too-complex') → ask（不是 allow）
if (subcommands > MAX) → ask
```

→ 詳見 [[Fail-Closed 與 Deny-First 原則]]

## 模式 3：Deny-First

deny 規則永遠在 allow 之前檢查。即使有 allow 匹配，deny 仍勝出。

→ 詳見 [[權限規則引擎]]

## 模式 4：非對稱 Env Var 剝除

Allow 路徑只剝除已知安全的 env vars（保守）。
Deny/Ask 路徑剝除所有 env vars（激進）。

```
安全列表（30+）：LANG, LC_ALL, TERM, HOME, USER, ...
絕對禁止：PATH, LD_PRELOAD, PYTHONPATH, NODE_OPTIONS
```

→ 詳見 [[權限規則引擎]]

## 模式 5：Parser Differential 防禦

偵測不同 parser 對同一輸入的解釋差異：

```
shell-quote 看到  |  bash 看到  |  風險
"safe_cmd"        |  evil_cmd    |  繞過安全檢查
```

23 個 validators 中有專門的 misparsing validators。

→ 詳見 [[Bash 命令安全過濾與 AST 解析]]

## 模式 6：多版本 Quote Tracker

同一命令需要多個「視角」來偵測不同攻擊向量。每個 validator 使用最適合的去引號版本。

→ 詳見 [[Bash 命令安全過濾與 AST 解析]]

## 模式 7：白名單旗標驗證（Allowlist-only Flag Validation）

唯讀命令不只驗證命令名稱，還精確列舉每個允許的 flag 及其引數類型。

→ 詳見 [[唯讀模式與檔案系統權限]]

## 模式 8：可疑路徑模式偵測

偵測可疑路徑模式（NTFS ADS、8.3 短名、UNC）並要求確認，而非嘗試正規化。

> [!info] 為什麼偵測優於正規化
> - 正規化依賴檔案系統狀態
> - 有 TOCTOU 競爭條件
> - 跨平台行為不一致

→ 詳見 [[唯讀模式與檔案系統權限]]

## 模式 9：Speculative / Race 許可模式

三個決策源（Hook、AI Classifier、用戶點擊）同時賽跑，第一個完成的生效。

```
ResolveOnce: Promise.race([hookResult, classifierResult, userClick])
```

兼顧安全（所有源都是合法決策者）和體驗（減少等待）。

→ 詳見 [[權限規則引擎]]

## 模式 10：複合命令分層驗證

Allow 的前綴匹配不匹配複合命令（防止 `cd /path && rm -rf /`）。
Deny 規則能匹配複合命令中的個別子命令。

→ 詳見 [[權限規則引擎]]

## 安全設計 Checklist

> [!tip] 設計新 Agent 工具安全系統時
> - [ ] 是否有 2+ 層獨立防禦？
> - [ ] 不確定時是否 Fail-Closed？
> - [ ] Deny 規則是否優先於 Allow？
> - [ ] 是否考慮了 parser differential？
> - [ ] 白名單是否到 flag 級別？
> - [ ] 路徑是否偵測而非正規化？
> - [ ] 複合命令是否獨立驗證？

---

> [!tip] 導航
> 返回 [[Security & Permissions MOC]] · [[Claude Code 逆向工程知識庫]]
