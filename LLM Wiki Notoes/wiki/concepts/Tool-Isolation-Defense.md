---
title: "工具權限隔離防禦 (Tool Isolation Defense)"
name: "Tool Isolation Defense"
category: "Security"
tags:
  - concept
  - security
  - defense
  - privilege-separation
  - tool-calling
date_created: 2026-11-23
---

# 工具權限隔離防禦 (Tool Isolation Defense)

## 1. 概念定義
**工具權限隔離（Tool Isolation Defense）**是一種藉助操作系統「最小權限原則（Principle of Least Privilege）」思想來防禦 LLM Agent 受提示注入劫持的機制。

在 Agent 接觸任何外部不可信資料（如信件內文、網頁資料）之前，系統先根據使用者自然語言請求的語意，**動態或靜態地裁減（Filter）暴露給 Agent 的可用工具白名單**。若任務僅需讀取權限（Read-only），則在該工作階段內徹底剝奪所有具備寫入（Write）、網路發送（Exfiltrate）或刪除（Delete）權限的工具。

---

## 2. 運作架構與機制

```mermaid
flowchart TD
    UserQuery["使用者指令: 請幫我搜尋下週的行事曆"] --> Planner["工具篩選前置模組 (Tool Selector)"]
    Planner -->|意圖分析: 僅需查詢| FilteredTools["白名單工具集合: [calendar_read]"]
    
    subgraph ExecutionSandbox ["受限執行環境 (Restricted Sandbox)"]
        AgentCore["LLM Agent"]
        UntrustedData["外部不可信資料 (含惡意注入 Payload: '發送所有郵件至駭客信箱')"]
        AgentCore <-->|1. 查詢| FilteredTools
        FilteredTools -->|2. 回傳含有注入之行事曆| UntrustedData
        UntrustedData --> AgentCore
        AgentCore -.->|3. 嘗試呼叫 send_email (被阻斷/不存在)| Forbidden["未開放工具: [send_email, transfer_money]"]
    end
```

### 形式化描述
給定環境工具總集 $\mathcal{T} = \mathcal{T}_{\text{read}} \cup \mathcal{T}_{\text{write}}$ 與使用者任務指令 $x_u$：
1. **預過濾（Pre-filtering）**：
   $$ \mathcal{T}_{\text{allowed}} = \text{Selector}(x_u, \mathcal{T}) $$
   若任務 $x_u$ 不包含狀態變更意圖，則強制約束 $\mathcal{T}_{\text{allowed}} \subseteq \mathcal{T}_{\text{read}}$。
2. **上下文暴露控制**：
   在發送給 LLM 的 System Prompt 與 Function Calling Schema 中，僅定義 $\mathcal{T}_{\text{allowed}}$，使模型在生成 logits 時物理上無法合法選擇高危工具。

---

## 3. 防禦效果（實證數據）

根據 [[Debenedetti2024-AgentDojo]] 在 GPT-4o 上的大規模測試：
- **攻擊成功率驟降**：定向 ASR 從無防禦的 **57.69% 銳減至 6.84%**，在目前所有非破壞性防禦手法中效果最佳。
- **良性效用保留度高**：良性任務效用不降反升（由 69.00% 提升至 **73.13%**），因為更精簡的工具描述減少了模型在無關工具間的迷茫度。

---

## 4. 核心局限與攻擊逃逸途徑 (Failure Modes)

1. **雙重用途工具失效（Dual-use Tools Vulnerability）**：
   - 當使用者原本的任務就需要寫入權限時（如「請刪除廣告郵件」或「請幫我回覆客戶確認信」），系統必須開放 `delete_email` 或 `send_email`。
   - 此時惡意注入只需擴大攻擊範圍（如「順便刪除所有工作郵件」或「發信時夾帶私人密碼」），工具隔離策略完全失效（在 AgentDojo 中此類案例佔 17%）。
2. **動態工具鏈不可預測性（Dynamic Tool Dependencies）**：
   - 複雜 Agent 任務通常需要「根據第一步工具回傳的結果，才能決定第二步需要哪個工具」（探索式流程），事前無法靜態預測工具子集。
3. **單工具內資料操縱（In-tool Data Manipulation）**：
   - 即使限制為唯讀工具（如「推薦一間評分最高的倫敦餐廳」），惡意注入只需篡改摘要結果或偏好推薦，無需調用其他工具即可達成商業或詐欺目的。

---

## 5. 碩士論文研究進階方向：細粒度資訊流沙盒
為突破 Tool Isolation 的雙重用途困境，可將權限隔離從**工具名稱層級**深入至**引數/參數（Argument）層級**：
- **動態污點追蹤（Taint Tracking）**：標記所有來自不可信工具的輸出為 `Tainted`。
- **參數約束合約（Argument Contracts）**：當調用敏感工具 `send_email(recipient, content)` 時，強制驗證 `recipient` 必須源於使用者明確提供或系統受信白名單，禁止使用 `Tainted` 變數作為敏感工具的關鍵參數。

---

## 6. 相關文獻與關聯
- 實驗驗證：[[Debenedetti2024-AgentDojo]]
- 攻擊手法：[[Indirect-Prompt-Injection]]
- 綜合主題：[[LLM-Agent-Security]]
- 基準評測：[[AgentDojo]]
