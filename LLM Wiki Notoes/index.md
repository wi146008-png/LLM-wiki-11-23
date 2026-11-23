# 論文知識庫索引 (Paper Wiki Index)

本索引是整個知識庫的全局導航目錄，由 LLM Agent 在每次 Ingest、Query 沉澱或 Lint 時自動維護與更新。

---

## 📚 論文文獻 (Papers)
> 格式：`[[檔案名|簡稱]]` - 英文完整標題 (年份, 會議/期刊) — *一句話核心貢獻*

<!-- papers:start -->
- [[Debenedetti2024-AgentDojo|AgentDojo]] - AgentDojo: A Dynamic Environment to Evaluate Prompt Injection Attacks and Defenses for LLM Agents (2024, NeurIPS) — *提出首個包含 4 大環境與 629 個測試案例的動態 Agent 提示注入評測基準，揭示逆標度現象並驗證工具隔離防禦*
<!-- papers:end -->

---

## 💡 核心概念與方法 (Concepts & Methods)
> 關鍵技術、架構組件、損失函數或演算法概念。

<!-- concepts:start -->
- [[Indirect-Prompt-Injection|間接提示注入 (Indirect Prompt Injection)]] — *第三方不可信資料劫持 LLM 工具呼叫與執行邏輯的對抗攻擊*
- [[Tool-Isolation-Defense|工具權限隔離防禦 (Tool Isolation Defense)]] — *基於最小權限原則在規劃階段預先篩選可用工具白名單的防禦機制*
<!-- concepts:end -->

---

## 🧭 主題綜述與技術脈絡 (Topics & Synthesis)
> 跨論文的比較、技術演進圖、針對碩論特定方向的綜合分析。

<!-- topics:start -->
- [[LLM-Agent-Security|LLM Agent 安全性與對抗防禦]] — *涵蓋提示注入、資訊外洩、狀態篡改與權限沙盒防禦的技術綜述與碩論切入點*
<!-- topics:end -->

---

## 📊 評測基準與資料集 (Benchmarks & Datasets)
> 常用 Benchmark、評測指標、Leaderboard 與 SOTA 演進。

<!-- benchmarks:start -->
- [[AgentDojo|AgentDojo 評測基準]] — *NeurIPS 2024，包含 74 個工具、97 個任務、629 個安全案例的動態確定性評測環境*
<!-- benchmarks:end -->

---

## 🖥️ 簡報與專題報告 (Presentations)
> 課堂研討報告、主題 Slide Decks 與成果展示。

<!-- presentations:start -->
- [AgentDojo 課堂拓撲圖譜簡報 (Graph-driven Interactive Presentation)](file:///Users/liyachen/Downloads/LLM%20Wiki%20Notoes/presentations/agentdojo-presentation.html) — *基於 8 核心節點動態拓撲圖譜與 9 站鏡頭導航設計的科技感簡報，內建逐頁講稿、逆標度分析與碩論創新沙盒*
<!-- presentations:end -->
