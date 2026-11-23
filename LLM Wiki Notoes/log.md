# 知識庫演進日誌 (Wiki Evolution Log)

此檔案為時間順序的追加日誌（Append-only），記錄所有 Ingest（文獻收錄）、Query（合成沉澱）與 Lint（健康體檢）操作。

---

## [2026-11-23] init | 論文知識庫初始化
- 建立三層架構：`raw/`、`wiki/`、`AGENTS.md`。
- 初始化 `index.md` 與 `log.md`。
- 確立針對碩士論文研究的專用規範（保留數學公式、Loss Function、演算法虛擬碼、雙向鏈接）。

## [2026-11-23] ingest | Debenedetti2024-AgentDojo - AgentDojo: A Dynamic Environment to Evaluate Prompt Injection Attacks and Defenses for LLM Agents
- 下載原始 PDF 至 `raw/papers/Debenedetti2024-AgentDojo.pdf`。
- 建立論文筆記 `wiki/papers/Debenedetti2024-AgentDojo.md`，完整萃取環境形式化狀態公式、多輪互動虛擬碼與消融實驗數據。
- 建立基準頁面 `wiki/benchmarks/AgentDojo.md`。
- 衍生建立核心概念頁面 `wiki/concepts/Indirect-Prompt-Injection.md` 與 `wiki/concepts/Tool-Isolation-Defense.md`。
- 沉澱主題綜述頁面 `wiki/topics/LLM-Agent-Security.md`，建立 SOTA 模型 Leaderboard 與碩論研究切入點分析。
- 同步更新 `index.md` 全局導航。

## [2026-11-23] presentation | AgentDojo 碩士課堂報告互動投影片
- 生成高科技風格的 HTML 互動簡報 `presentations/agentdojo-presentation.html`。
- 專門針對向教授與課堂同學報告設計，具備 10 頁核心架構、完整圖表數據比對、逆標度分析、碩論啟發切入點，以及內建講稿提示（Speaker Notes）。
- 更新 `index.md` 索引目錄。

## [2026-11-23] presentation | 升級為 Graph 拓撲導航互動投影片
- 嚴格依照「大綱定案 ➔ 逐頁腳本 ➔ 動態實作」三階段流程重構簡報。
- 建立 SVG 動態網絡拓撲（包含 User、Agent、Untrusted Data、Attacker、Tools、State、Defense、Thesis 8 大實體節點）。
- 實作 9 站鏡頭平移放大（Camera Pan & Zoom）、雷射數據流動畫與雙重用途護盾裂解特效。
- 為每站嵌入針對教授的 Speaker Notes（按 N 開啟）與通俗生動的生活化比喻。
- 完成全部檢驗清單驗收。

## [2026-11-23] git | 啟用 Git 版本控制管理
- 建立 `.gitignore` 排除 macOS 系統檔（`.DS_Store`）與編輯器暫存檔。
- 初始化 Git 本地儲存庫並配置本地使用者資訊。
- 完成初始提交（Initial Commit），收錄三層 Wiki 架構、原始論文 PDF、概念與主題筆記、以及科技感 Graph 簡報。

## [2026-11-23] script | 完成 AgentDojo 課堂報告 9 站逐頁字稿與演講小抄
- 大綱經使用者審核同意通過（Gate 1）。
- 依據定案大綱完成 `presentations/scripts/AgentDojo-Graph-Script.md`。
- 涵蓋 9 大站點之圖譜規格、通俗比喻、學術深度公式、逐字演講講稿與教授預期 Q&A 應對。

## [2026-11-23] presentation | 依定案字稿完成最終 Graph 互動簡報實作
- 依據 `presentations/scripts/AgentDojo-Graph-Script.md` 定案字稿，全面升級 `presentations/agentdojo-presentation.html`。
- 100% 同步 9 站圖譜視覺、生活化比喻、學術公式與完整演講逐字小抄（按 `N` 開啟）。
- 新增教授預期答辯錦囊抽屜（按 `Q` 開啟），內建 3 大深度學術問答應對策略。
- 實作雙重用途護盾破裂特效與微核心創新沙盒發光連線。
