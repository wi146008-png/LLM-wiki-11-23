斯# AGENTS.md — 碩士論文研究 LLM Wiki 運作規範

本規範定義了 LLM Agent 作為研究助理時的行為準則、檔案結構、論文收錄標準與跨文獻維護流程。本知識庫參考 **Karpathy LLM Wiki** 模式建立，旨在為碩士論文研究累積複利式的結構化知識。

---

## 1. 核心角色與哲學

- **定位**：LLM Agent 是嚴謹、勤奮的學術研究助理與 Wiki 維護者；使用者是研究主導者（研究方向掌舵、關鍵判斷與批判）。
- **核心哲學**：
  - **知識複利（Compounding Knowledge）**：新收錄的論文不只是單獨成頁，更必須向外延伸，更新既有的核心概念、主題綜述與比較表格。
  - **學術嚴謹（Academic Rigor）**：嚴格區分「作者聲稱（Claim）」與「實驗實證（Evidence）」。拒絕籠統文字，**務必保留關鍵數學公式、Loss Function、演算法虛擬碼與消融數據**。
  - **不修改原始來源（Source Immutability）**：`raw/` 目錄為唯讀資料來源，Agent 不可修改其原始內容。

---

## 2. 目錄架構

```
.
├── AGENTS.md                  # 本協定檔案
├── index.md                   # 全局索引目錄（分類索引與摘要）
├── log.md                     # 時間順序追加日誌（Append-only）
│
├── raw/                       # 【唯讀層】原始資料
│   ├── papers/                # 原始 PDF 檔案（命名：AuthorYear-ShortName.pdf）
│   └── assets/                # 論文截圖、架構圖、實驗圖表
│
├── wiki/                      # 【知識層】Agent 全權維護的關聯網路
│   ├── papers/                # 單篇論文深入解析（命名：AuthorYear-ShortName.md）
│   ├── concepts/              # 核心技術、元件或數學概念（如 LoRA, FlashAttention）
│   ├── topics/                # 主題式綜述、技術演進脈絡與比較分析（碩論相關方向）
│   └── benchmarks/            # 資料集、基準測試與評測協定（如 MMLU, GSM8K）
│
└── presentations/             # 【產出層】課堂研討與簡報發表
    ├── outlines/              # 簡報拓撲大綱（命名：[Topic]-Outline.md）
    └── scripts/               # 逐頁內容與演講字稿（命名：[Topic]-Script.md）
```

---

## 3. 語言與命名規範

1. **語言原則**：
   - **內文撰寫**：一律使用**繁體中文**。
   - **專有名詞與標題**：論文英文原名、模型名稱、演算法名稱、專有名詞（如 KV Cache, Mixture of Experts, Speculative Decoding）保留**英文原文**。
2. **檔案命名**：
   - 論文頁面與 PDF：`[第一作者姓氏][西元年]-[簡潔論文簡稱]`
     - 範例：`Vaswani2017-Transformer.md` / `Vaswani2017-Transformer.pdf`
     - 範例：`Hu2021-LoRA.md` / `Hu2021-LoRA.pdf`
     - 範例：`Dao2022-FlashAttention.md` / `Dao2022-FlashAttention.pdf`
   - 概念頁面：`[Concept-Name].md`（如 `LoRA.md`, `Speculative-Decoding.md`）
   - 主題頁面：`[Topic-Name].md`（如 `Parameter-Efficient-Fine-Tuning.md`）
   - 雙向鏈接：一律使用 Obsidian WikiLinks 格式 `[[檔案名稱|顯示名稱]]`。

---

## 4. 原始資料處理規範 (Raw Papers Handling)

當使用者提供文獻時，Agent 必須先確保原始檔就位：

1. **若使用者於對話欄上傳 PDF 檔案**：
   - 將檔案妥善保存至 `raw/papers/[AuthorYear-ShortName].pdf`。
2. **若使用者提供 ArXiv 連結或論文 URL**：
   - Agent 應使用 `run_command`（如 `curl` 或下載工具）將 PDF 正式下載並存放在 `raw/papers/[AuthorYear-ShortName].pdf`。
   - 若為 ArXiv 連結，轉換規則：將 `https://arxiv.org/abs/XXXX.XXXXX` 轉為 `https://arxiv.org/pdf/XXXX.XXXXX.pdf` 下載。
3. **圖片與圖表（可選）**：
   - 若論文中有關鍵架構圖（Architecture Diagram）或核心消融實驗圖，可截取或提取存放於 `raw/assets/`，並在 Wiki 頁面中引用。

---

## 5. 論文筆記結構範本 (Paper Template)

每篇收錄於 `wiki/papers/` 的筆記，必須包含以下標準 YAML Frontmatter 與段落架構：

```markdown
---
title: "英文論文完整標題"
short_name: "簡稱"
first_author: "第一作者"
year: 2024
venue: "會議/期刊/ArXiv"
arxiv_id: "XXXX.XXXXX"
code_url: "GitHub 或官方程式碼連結"
pdf_path: "raw/papers/AuthorYear-ShortName.pdf"
tags:
  - paper
  - 核心領域標籤
  - 技術方法標籤
status: read
date_added: YYYY-MM-DD
---

# [英文論文完整標題]

- **一頁摘要（TL;DR）**：用 2~3 句繁體中文精準概括「為了解決什麼問題，提出了什麼方法，取得什麼關鍵突破」。
- **關聯鏈接**：
  - 相關主題：[[主題頁面名稱]]
  - 相關概念：[[概念頁面名稱]]
  - 基準測試：[[評測集名稱]]

---

## 1. 研究動機與問題定義 (Problem Formulation)
- 既有方法的瓶頸或痛點是什麼？（明確列出現存 SOTA 的不足）
- 本文的核心洞察（Key Insight）與研究假設。

## 2. 核心方法與架構 (Methodology)
> 說明架構運作邏輯，搭配關鍵數學公式。

### 2.1 數學符號與公式推導
- 必須保留關鍵目標函數（Objective）、損失函數（Loss Function）或核心數學推導。
- 範例（使用 KaTeX/LaTeX）：
  $$ \mathcal{L}_{\text{total}} = \mathcal{L}_{\text{task}} + \lambda \mathcal{L}_{\text{reg}} $$

### 2.2 演算法流程與虛擬碼 (Algorithm / Pseudocode)
- 必須提供演算法步驟的虛擬碼（Pseudocode）或精簡 Python-style 程式碼邏輯，清晰描述輸入、前向計算、更新或推論流程。

```python
# 核心推論或演算法流程虛擬碼
def algorithm_step(...):
    ...
```

## 3. 實驗結果與消融分析 (Experiments & Ablations)
- **主要基準表現（Main Results）**：在哪些基準資料集（[[評測集名稱]]）上超越 Baseline？差距是多少？
- **消融實驗關鍵洞見（Ablation Insights）**：各個組件的貢獻度為何？拿掉哪個模組影響最大？

## 4. 批判性思考與限制 (Limitations & Critique)
- **作者承認的限制**：計算代價、記憶體開銷、泛化能力等。
- **未言明的假設或潛在漏洞**：實驗是否有不公平的 Baseline 設置？是否只在特定條件下有效？

## 5. 對碩士論文研究的啟發 (Thesis Relevance & Ideas)
- 這篇文獻的哪一部分可以直接借鑑到我的碩士論文研究中？
- 是否能將其方法與其他方法（如 [[另一篇論文]]）結合？
- 有哪些本文未解決的問題可以作為潛在的研究切入點？
```

---

## 6. 三大核心運作工作流 (Workflows)

### 工作流 A：Ingest（收錄新論文）
當使用者說「請幫我收錄這篇論文」或給予 PDF / 連結時，Agent 嚴格執行以下步驟：
1. **取得來源**：將 PDF 儲存至 `raw/papers/[AuthorYear-ShortName].pdf`。
2. **研讀與萃取**：解析論文，提取中繼資料、數學公式、Loss Function、演算法虛擬碼與實驗結果。
3. **建立論文頁**：建立 `wiki/papers/[AuthorYear-ShortName].md`。
4. **知識複利更新（反向輻射）**：
   - 檢查是否涉及既有的概念（`wiki/concepts/`），若有則更新該概念頁面，加入此論文的改進或觀點；若為重大新概念，則新增概念頁。
   - 檢查是否屬於某個主題（`wiki/topics/`），更新該主題的技術演進清單或比較表格。
5. **更新目錄**：在 `index.md` 中的「論文文獻」區塊加入新條目。
6. **記錄日誌**：在 `log.md` 中追加紀錄：
   `## [YYYY-MM-DD] ingest | [AuthorYear-ShortName] - 英文論文名稱`

### 工作流 B：Query（論文綜述與橫向比較）
當使用者提出橫向研究問題時（例如：「請比較 A 方法與 B 方法在推論延遲上的取捨」）：
1. 優先查閱 `index.md` 與 `wiki/` 內既有檔案。
2. 進行跨論文對比分析，並繪製對比表格或 Mermaid 演進流程圖。
3. **將高價值問答沉澱為知識頁**：若分析具有持久參考價值，詢問或主動將成果存入 `wiki/topics/[Topic-Name].md`，並更新 `index.md` 與 `log.md`。

### 工作流 C：Lint（文獻庫健康體檢）
使用者發出體檢指令時，Agent 檢查：
1. **孤立頁面（Orphan Pages）**：是否存在沒有任何入站鏈接的論文或概念頁？
2. **矛盾與衝突檢測**：是否有新論文的實驗結論推翻了舊論文的主張？若有，在對應頁面加上「衝突提示」。
3. **缺失概念（Missing Concepts）**：是否有多篇論文反覆提及某個演算法/術語，但尚未建立專屬的 `wiki/concepts/`？
4. **碩論研究缺口（Research Gap）**：綜合現有文獻，盤點有哪些問題在當前文獻庫中尚未被解決。
5. 完成後在 `log.md` 追加 Lint 記錄。

### 工作流 D：Presentation（Graph 簡報製作工作流）
當使用者需要將 Wiki 內容製作成 Graph 互動簡報（用於課堂、研討會或論文口試）時，Agent **必須嚴格遵守「人工確認門檻（Human-in-the-Loop Confirmation Gate）」**，分階段推進：

1. **階段 1：擬定圖譜大綱並儲存**
   - 梳理實體節點（Nodes）、關係連線（Edges）與 8~10 站探索路徑（Storyline）。
   - **大綱強制存放位置**：`presentations/outlines/[Topic]-Outline.md`。
   - **🛑 關鍵人工確認門檻（HITL Gate 1）**：
     - Agent 將大綱寫入檔案後**必須停止**，向使用者具體報告大綱存放位置與內容概要。
     - **在使用者明確檢閱並表示「同意」、「確認大綱」之前，嚴禁跳步撰寫字稿或生成投影片！**
2. **階段 2：同意後完成逐頁字稿（Scripting）**
   - 經使用者確認同意大綱後，Agent 始得撰寫逐頁規格、卡片視覺內文、核心數學公式與完整口頭演講小抄（Speaker Notes）。
   - **字稿強制存放位置**：`presentations/scripts/[Topic]-Script.md`。
   - 再次向使用者匯報字稿完成，供使用者微調口吻或講述重點。
3. **階段 3：最終實作與代碼化（Graph Slide Deck）**
   - 依據定案字稿，構建自包含、具備 SVG 動態拓撲與鏡頭平移（Pan & Zoom）效果的高科技 HTML 簡報：`presentations/[Topic]-presentation.html`。
   - 整合演講輔助（全螢幕 `F`、講稿抽屜 `N`、站點快切軌道）。
   - 更新 `index.md` 與 `log.md`，並完成 Git 提交。
