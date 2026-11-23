---
title: "AgentDojo Benchmark"
short_name: "AgentDojo"
type: "security-benchmark"
introduced_by: "[[Debenedetti2024-AgentDojo]]"
year: 2024
environments: 4
tools_count: 74
tasks_count: 97
security_cases: 629
tags:
  - benchmark
  - prompt-injection
  - agent-security
---

# AgentDojo 評測基準 (AgentDojo Benchmark)

## 1. 概述
**AgentDojo** 是由蘇黎世聯邦理工學院（ETH Zurich）SPY Lab 於 2024 年提出針對 LLM Agent 的**動態提示注入安全評測環境**（發表於 NeurIPS 2024）。其核心宗旨是解決靜態文本評測無法體現多步工具執行與動態狀態轉換的缺陷，並避免傳統使用 LLM Evaluator 進行裁判時自身遭到攻擊劫持的風險。

---

## 2. 核心架構與測試維度

```
AgentDojo (629 Security Cases)
├── Workspace Suite (240 Cases: 40 Tasks × 6 Injections)
├── Slack Suite (105 Cases: 21 Tasks × 5 Injections)
├── Travel Suite (140 Cases: 20 Tasks × 7 Injections)
└── E-banking Suite (144 Cases: 16 Tasks × 9 Injections)
```

- **狀態式環境（Stateful Environment）**：包含 4 大真實模擬情境、74 種常用 API 工具。
- **雙向笛卡爾積測試**：將 97 個正常任務與 27 個惡意注入任務交叉組合，構成 629 個確定性驗證測試案例。
- **確定性狀態評估**：採用 Python 程式化斷言函數 $f(S_{\text{init}}, S_{\text{final}}, y)$ 檢驗系統狀態前後的數據變化（例如特定檔案是否被刪除、資金是否被轉移），無須透過不可信的 LLM 評審。

---

## 3. 核心指標體系

1. **良性任務效用（Benign Utility）**：
   在無任何攻擊介入的情況下，Agent 成功解決正常任務的比例：
   $$ \mathcal{U}_{\text{benign}} = \frac{1}{|U|} \sum_{u \in U} f_u(S_0, S_T, y) $$
2. **受攻擊任務效用（Utility under Attack / DoS Robustness）**：
   在遭受攻擊注入時，Agent 依然能正確完成原任務且未引發惡意副作用的比例：
   $$ \mathcal{U}_{\text{attack}} = \frac{1}{|U \times I|} \sum_{(u, i)} f_u(S_0, S_T, y) \cdot (1 - f_a(S_0, S_T, y)) $$
3. **定向攻擊成功率（Targeted ASR）**：
   Agent 被惡意 Prompt 成功劫持，執行攻擊者目標工具呼叫的比例：
   $$ \text{ASR} = \frac{1}{|U \times I|} \sum_{(u, i)} f_a(S_0, S_T, y) $$

---

## 4. 主流模型 Leaderboard (重要訊息攻擊基準)

| 模型 (Model) | 良性效用 ($\mathcal{U}_{\text{benign}}$) | 受攻擊效用 ($\mathcal{U}_{\text{attack}}$) | 定向 ASR ($\text{ASR}$) | 綜合安全性評估 |
| :--- | :---: | :---: | :---: | :--- |
| **Claude 3.5 Sonnet** | **78.22%** | **51.19%** | 33.86% | 目前總合效用與魯棒性最佳（Pareto Frontier） |
| **GPT-4o** | 69.00% | 50.08% | **47.69%** | 指令遵循性高，但也極度脆弱於間接注入 |
| **Claude 3 Opus** | 66.61% | 52.46% | **11.29%** | 最保守防禦表現，攻擊成功率顯著偏低 |
| **GPT-4 Turbo** | 63.43% | 54.05% | 28.62% | 穩定平衡型 |
| **Gemini 1.5 Pro** | 45.63% | 28.93% | 25.60% | 多步工具調用穩定度有待提升 |
| **Llama 3 70B** | 34.50% | 18.28% | 20.03% | 開源模型領先者，但多輪環境失誤率高 |

---

## 5. 相關文獻與關聯
- 提出文獻：[[Debenedetti2024-AgentDojo]]
- 相關概念：[[Indirect-Prompt-Injection]]、[[Tool-Isolation-Defense]]
- 相關主題：[[LLM-Agent-Security]]
