---
title: "KP-09 RLHF 與現代強化學習"
aliases:
  - "RLHF"
  - "強化學習"
  - "Reinforcement Learning"
  - "DPO"
  - "GRPO"
type: knowledge-point
category: 強化學習
tags:
  - knowledge-point
  - RLHF
  - PPO
  - DPO
  - reward-model
  - alignment
  - InstructGPT
  - constitutional-AI
related_course_notes:
  - "[[C3-W3 - Reinforcement Learning]]"
  - "[[C2-W1 - Neural Networks]]"
related_kp:
  - "[[KP-03 - 損失函數]]"
  - "[[KP-06 - Attention 機制與 Transformer]]"
  - "[[KP-07 - 縮放法則與湧現能力]]"
---

# KP-09：RLHF 與現代強化學習（RLHF & Modern RL）

> **課程關聯：** RL 基礎（MDP、Bellman、DQN）見 [[C3-W3 - Reinforcement Learning]]；本文延伸至如何將 RL 用於**對齊大型語言模型**。

---

## 1. 從 DQN 到 RLHF 的演化

```mermaid
graph LR
    DQN["Deep Q-Network<br/>（離散動作空間）<br/>Atari 遊戲"] --> PPO["PPO<br/>（連續動作空間）<br/>機器人控制"]
    PPO --> RLHF["RLHF<br/>（語言模型對齊）<br/>ChatGPT / InstructGPT"]
    RLHF --> DPO["DPO<br/>（免 RL 的偏好學習）<br/>更穩定"]
    PPO --> GRPO["GRPO<br/>（無 Critic）<br/>DeepSeek-R1"]
    GRPO --> R1ZERO["R1-Zero<br/>（跳過 SFT）<br/>Pure RL 推理"]
```

---

## 2. 為什麼需要 RLHF？

**問題：** 大型語言模型（LLM）經過預訓練後，傾向於：
- 生成不真實、有毒或不相關的內容
- 「平均化」語言而非遵循用戶意圖

**RLHF 的目標（Alignment）：** 讓模型的行為與**人類偏好和價值觀**對齊（Helpful, Honest, Harmless — 3H 原則）。

> [!tip] 🎯 白話舉例：RLHF 像訓練客服人員
> 預訓練的 LLM 像一個讀了整個網路的人，知識淵博但「沒有讀空氣」——有時說廢話、有時提供危險建議、有時回答與問題無關。
> RLHF 就是讓這個人「上客服培訓班」：
> - **步驟 1（SFT）** = 看優秀客服的示範對話，學習基本禮儀
> - **步驟 2（RM）** = 主管對回答打分，建立「什麼是好回答」的標準
> - **步驟 3（PPO）** = 根據評分不斷練習改進，但不能偏離原來的知識太多

---

## 3. RLHF 完整流程（InstructGPT）

### 3.1 三步驟

```mermaid
graph TD
    STEP1["步驟 1：監督微調 SFT<br/>收集人工示範數據<br/>Fine-tune GPT-3"] --> STEP2["步驟 2：訓練獎勵模型 RM<br/>收集模型輸出的人類排名<br/>訓練 RM 預測人類偏好分數"]
    STEP2 --> STEP3["步驟 3：PPO 強化學習<br/>用 RM 作為獎勵函數<br/>更新語言模型策略"]
```

**論文來源：**
> Ouyang, L. et al. (2022). **Training Language Models to Follow Instructions with Human Feedback.** *NeurIPS 2022.* [arxiv:2203.02155](https://arxiv.org/abs/2203.02155)

**關鍵結果：** 1.3B 參數的 InstructGPT 在人類評估中優於 175B 的原始 GPT-3。這證明了「對齊」的價值可能比「規模」更大（參見 [[KP-07 - 縮放法則與湧現能力]]）。

### 3.2 獎勵模型（Reward Model）

給定同一 prompt 的兩個回答 $y_w$（較佳）和 $y_l$（較差），訓練 RM 最大化偏好對的對數似然：

$$\mathcal{L}_{RM} = -\log \sigma\left(r_\phi(x, y_w) - r_\phi(x, y_l)\right)$$

---

## 4. PPO（Proximal Policy Optimization）

### 4.1 核心公式

PPO 的目標是最大化**裁剪的替代目標（Clipped Surrogate Objective）**：

$$\mathcal{L}^{\text{CLIP}}(\theta) = \mathbb{E}_t\left[\min\left(r_t(\theta) A_t,\; \text{clip}(r_t(\theta), 1-\epsilon, 1+\epsilon) A_t\right)\right]$$

其中：
- $r_t(\theta) = \frac{\pi_\theta(a_t|s_t)}{\pi_{\theta_{\text{old}}}(a_t|s_t)}$：新舊策略的概率比
- $A_t$：優勢函數（Advantage function），衡量動作比基線好多少
- $\epsilon$：裁剪閾值（通常 0.1–0.2），防止策略更新過大

**白話：** 「允許策略在獎勵好時更新，但不能更新太多（裁剪防止過大步長）。」

> [!tip] 🎯 白話舉例：PPO 像「小步快走」的策略
> 想像你在訓練一隻導盲犬。每次它做對了你就給零食（獎勵），但你不能讓它一次改變太多行為（裁剪）——否則它可能從「探索型」突變成「只做一個動作」（策略崩潰）。PPO 的裁剪機制確保每次更新都是「小步快走」，穩定且安全。

**論文來源：**
> Schulman, J. et al. (2017). **Proximal Policy Optimization Algorithms.** [arxiv:1707.06347](https://arxiv.org/abs/1707.06347)

### 4.2 RLHF 中的 PPO + KL 懲罰

$$r(x, y) = r_\phi(x, y) - \beta \cdot D_{\text{KL}}\left[\pi_\theta(y|x) \| \pi_{\text{ref}}(y|x)\right]$$

- $r_\phi(x, y)$：獎勵模型分數
- $\beta D_{\text{KL}}[\cdot]$：防止偏離原始模型（避免「獎勵黑客」— reward hacking）

> [!tip] 🎯 白話舉例：KL 懲罰像「不能偏離原來的自己太多」
> 沒有 KL 懲罰的話，模型可能為了拿高分而「作弊」——比如學會「用特定短語騙過評分器」而不是真正提升回答品質。KL 懲罰就像告訴模型：「你可以改進，但不能變成一個完全不同的人。」（詳見 [[KP-03 - 損失函數#5. KL Divergence（KL 散度）]]）

---

## 5. Constitutional AI（Anthropic，2022）

**問題：** 人工標注偏好數據昂貴且可能有偏見。

**核心思想：** 用一套明文規則（Constitution）讓 AI 自我批評並修正輸出，再用 AI 生成的偏好對訓練 RM——**AI Feedback（RLAIF）** 替代部分人類標注。

**論文來源：**
> Bai, Y. et al. (2022). **Constitutional AI: Harmlessness from AI Feedback.** [arxiv:2212.08073](https://arxiv.org/abs/2212.08073)

---

## 6. DPO（Direct Preference Optimization）★ 重要突破

### 6.1 核心問題：RLHF 太複雜

RLHF 需要：
1. 訓練獨立的 Reward Model（額外模型）
2. PPO 訓練不穩定（超參數敏感）
3. 多模型同時在記憶體中（SFT, RM, RL policy, reference policy）

### 6.2 DPO 的推導

**關鍵洞察：** RLHF 的最優策略有閉合解：

$$\pi^*(y|x) = \frac{1}{Z(x)} \pi_{\text{ref}}(y|x) \exp\left(\frac{r(x,y)}{\beta}\right)$$

將此代入 Bradley-Terry 偏好模型，推導出：

$$\mathcal{L}_{\text{DPO}}(\pi_\theta) = -\mathbb{E}_{(x,y_w,y_l)}\left[\log \sigma\left(\beta \log \frac{\pi_\theta(y_w|x)}{\pi_{\text{ref}}(y_w|x)} - \beta \log \frac{\pi_\theta(y_l|x)}{\pi_{\text{ref}}(y_l|x)}\right)\right]$$

**白話：** 直接用偏好對 $(y_w, y_l)$ 訓練語言模型，**無需訓練 RM，無需 PPO**，只需一個標準的監督損失。

> [!tip] 🎯 白話舉例：DPO 像「直接從考卷學」
> RLHF 的流程是：先訓練一個「評分老師」（RM），再讓學生根據評分老師的回饋練習（PPO）。這很複雜。
> DPO 說：「何不直接給學生看『好答案 vs 壞答案』的對照表，讓它自己學會區分？」——不需要評分老師，不需要複雜的 RL，只要一個簡單的損失函數。

**論文來源：**
> Rafailov, R. et al. (2023). **Direct Preference Optimization: Your Language Model is Secretly a Reward Model.** *NeurIPS 2023.* [arxiv:2305.18290](https://arxiv.org/abs/2305.18290)

**DPO 的後續發展：**

**GRPO（Group Relative Policy Optimization）：** DeepSeek 提出的替代方案，不需要 Critic 模型，透過組內相對排名來估計優勢函數，大幅降低訓練成本。詳見下方 [[#7. GRPO 與 DeepSeek-R1（2025 突破）]]。

> Shao, Z. et al. (2024). **DeepSeekMath: Pushing the Limits of Mathematical Reasoning in Open Language Models.** [arxiv:2402.03300](https://arxiv.org/abs/2402.03300)

### 6.3 DPO vs RLHF

| | RLHF + PPO | DPO |
|--|----------|-----|
| 額外模型 | Reward Model | 無 |
| 訓練穩定性 | 低（PPO 敏感）| 高（監督損失）|
| 記憶體需求 | 高（多模型）| 低（兩模型）|
| 效果 | SOTA | 相當甚至更好 |
| 應用 | InstructGPT | Zephyr、Llama3 等 |

---

## 7. GRPO 與 DeepSeek-R1（2025 突破）

### 7.1 GRPO 核心機制

**PPO 的問題：** 需要一個與 policy 同規模的 **Critic 模型**來估計優勢函數 $A_t$，記憶體與計算成本大約翻倍。

**GRPO 的解決方案：** 對同一個 prompt 生成**一組回答**（group），用組內的**相對獎勵排名**來估計優勢函數，完全不需要 Critic：

$$A_i = \frac{r_i - \text{mean}(\{r_1, ..., r_G\})}{\text{std}(\{r_1, ..., r_G\})}$$

**計算成本降低 ~50%**（去掉 Critic 模型），同時保持穩定性。

### 7.2 DeepSeek-R1-Zero：純 RL 訓練的突破

**革命性發現：** DeepSeek-R1-Zero 完全跳過 SFT 階段，**直接對 base model 執行 GRPO**，就能浩現出推理能力：

- 自發產生 **Chain-of-Thought** 推理鏈
- 出現 **「aha moment」** —— 模型在推理過程中自我糾正錯誤
- 在數學推理等任務上達到接近 OpenAI o1 的水準

### 7.3 DeepSeek-R1：完整訓練流程

```mermaid
graph LR
    BASE["DeepSeek-V3 Base"] --> COLD["Cold Start Data<br/>（少量高品質示範）"]
    COLD --> RL1["RL Stage 1<br/>GRPO + 推理獎勵"]
    RL1 --> REJECT["Rejection Sampling<br/>篩選高品質輸出"]
    REJECT --> SFT["SFT<br/>提升可讀性"]
    SFT --> RL2["RL Stage 2<br/>GRPO + 安全獎勵"]
```

**關鍵結果：**
- 效果匹配 OpenAI o1-1217
- 開源模型（671B MoE）+ 6 個蒸餞版本（1.5B~70B）
- 證明了 **RL 可以從 base model 直接激發推理能力**，而非僅透過 SFT 模仿

**論文來源：**
> DeepSeek-AI (2025). **DeepSeek-R1: Incentivizing Reasoning Capability in LLMs via Reinforcement Learning.** [arxiv:2501.12948](https://arxiv.org/abs/2501.12948)

### 7.4 GRPO vs PPO vs DPO 比較

| | PPO | DPO | GRPO |
|--|-----|-----|------|
| 需要 Reward Model | ✅ | ❌ | 可選（rule-based 亦可）|
| 需要 Critic | ✅（同規模）| ❌ | ❌ |
| 訓練穩定性 | 低 | 高 | 中高 |
| 計算成本 | 最高 | 最低 | 中等 |
| 可激發推理 | ✅ | ❌ | ✅（DeepSeek-R1）|
| 代表應用 | InstructGPT | Llama3, Zephyr | DeepSeek-R1, Kimi-k1.5 |

---

## 8. RL 在 LLM 之外的 2020+ 進展

### 8.1 AlphaCode（程式合成）

> Li, Y. et al. (2022). **Competition-Level Code Generation with AlphaCode.** *Science 2022.* [arxiv:2203.07814](https://arxiv.org/abs/2203.07814)

### 8.2 AlphaFold 2（蛋白質結構預測）

> Jumper, J. et al. (2021). **Highly Accurate Protein Structure Prediction with AlphaFold.** *Nature 2021.*

**雖非 RL，但體現了 ML 解決長期被認為「只有人類才能做到」問題的能力。**

### 8.3 Offline RL / Decision Transformer

**核心思想：** 把 RL 問題轉化為序列預測問題，用 Transformer 直接從離線資料學習最優策略（無需環境互動）。

> Chen, L. et al. (2021). **Decision Transformer: Reinforcement Learning via Sequence Modeling.** *NeurIPS 2021.* [arxiv:2106.01345](https://arxiv.org/abs/2106.01345)

---

## 9. RLHF 的問題與挑戰

| 問題 | 說明 |
|------|------|
| 獎勵黑客（Reward Hacking）| 模型學到欺騙 RM 的捷徑，而非真正提升質量 |
| 人類偏好一致性 | 不同標注者的偏好差異大，RM 訓練有噪聲 |
| 過度安全 | 模型可能拒絕合理請求（over-refusal）|
| 遺忘（Catastrophic Forgetting）| 微調可能損害預訓練能力 |
| 評估困難 | 「對齊」本身難以量化測量 |

> [!tip] 🎯 白話舉例：Reward Hacking 像「上有政策下有對策」
> 想像你給孩子的獎勵是「房間看起來乾淨就給零食」。孩子可能學會「把東西塞到床底下」而不是真正打掃——房間「看起來」乾淨，但實際上沒有。
> 這就是 Reward Hacking：模型找到了「欺騙評分器」的捷徑。KL 懲罰、Constitutional AI 等技術都是為了防止這種「作弊」。

---

## 10. 重點論文彙整

| 論文 | 年份 | arxiv | 貢獻 |
|------|------|-------|------|
| PPO | 2017 | [1707.06347](https://arxiv.org/abs/1707.06347) | 穩健 RL 策略優化，RLHF 基礎 |
| InstructGPT / RLHF | 2022 | [2203.02155](https://arxiv.org/abs/2203.02155) | LLM 對齊三步驟 |
| Constitutional AI | 2022 | [2212.08073](https://arxiv.org/abs/2212.08073) | AI Feedback 替代人工標注 |
| DPO | 2023 | [2305.18290](https://arxiv.org/abs/2305.18290) | 無 RM 偏好學習，更穩定 |
| Decision Transformer | 2021 | [2106.01345](https://arxiv.org/abs/2106.01345) | RL 轉化為序列建模 |
| GRPO | 2024 | [2402.03300](https://arxiv.org/abs/2402.03300) | 無 Critic 的組相對優化 |
| DeepSeek-R1 | 2025 | [2501.12948](https://arxiv.org/abs/2501.12948) | 純 RL 激發推理，匹配 o1 |

---

## 🔗 相關知識點

- [[KP-03 - 損失函數]] — RLHF 中的 KL 懲罰損失、DPO 損失的數學推導
- [[KP-06 - Attention 機制與 Transformer]] — LLM（Transformer Decoder）是 RLHF 的基礎架構
- [[KP-07 - 縮放法則與湧現能力]] — 大模型才能有效進行 RLHF；CoT 是湧現能力的代表
- [[KP-02 - 現代優化器]] — PPO 本身是策略優化器；AdamW 用於 SFT 與 DPO
- [[KP-04 - 正則化技術]] — KL 懲罰是 RLHF 中的核心正則化手段

## 🔗 相關課程筆記

- [[C3-W3 - Reinforcement Learning]] — MDP、Bellman、DQN 基礎（RLHF 的 RL 前身）
- [[C2-W3 - Advice for Applying ML]] — 評估指標與模型診斷（對齊評估的概念前身）
