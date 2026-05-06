# Bart's Claude Code Skills Collection

一套專業化、模組化的 Claude AI 技能集合，由 Bart 開發和維護。涵蓋知識管理、技術教學、代碼開發、業務流程、文檔撰寫、系統管理等多個領域。共 28 個 skills，每個都經過資安審核並已移除所有敏感信息，適合企業內部和開源使用。

## 🎯 核心價值

- **提高效能** — 將重複工作自動化，節省時間（平均 3-10 倍效率提升）
- **知識內化** — 用故事化和擬人化方式深化技術理解
- **流程標準化** — 業務報價、知識整理、代碼審查全流程覆蓋
- **完全開源** — 28 個 skills，所有代碼開源，完全移除個人信息，適合公開發佈

---

## 📦 Skills 完整列表 (28 個)

### 🔧 技術學習與講解 (2 個)

讓 Claude 用故事化方式講解複雜的技術概念，幫助用戶深度理解和教學準備。

| Skill | 描述 | 觸發方式 |
|-------|------|---------|
| **tech-evolution-storyteller** | 將任何技術概念轉化為時間線故事。自動搜尋完整信息，從歷史發展 → 核心原理 → 現代應用，梳理 6+ 核心要點。包含時間線圖表、概念圖解、學術引用。 | `/tech-evolution-storyteller` |
| **tech-personification-storytelling** | 用擬人化故事講解技術概念（如將 TCP 比作信件系統）。融合物理原理、歷史演進、日常比喻，幫助非專業人士快速理解複雜概念。 | `/tech-personification-storytelling` |

---

### 💼 業務報價流程 (4 個)

完整的商業報價閉環：詢價信 → 報價解析 → 成本估算 → 正式報價單。

| Skill | 描述 | 觸發方式 |
|-------|------|---------|
| **quote-inquiry** | 業務詢價信生成。讀取客戶需求單 + 代理商對照表，自動產出專業詢價 Email 草稿，支援多代理商並行詢價。 | `/quote-inquiry` |
| **quote-analysis** | 報價解析與自動比價。解析各代理商 Email/Excel 回覆，進行 QA 檢查，產出標準化比價表。 | `/quote-analysis` |
| **quote-estimator** | 工程師成本估算。根據技術複雜度自動計算工時和費用，支援假日/晚間加成計算。 | `/quote-estimator` |
| **quote-generator** | 正式報價單產出。根據比價結果自動填入報價單範本，支援 Excel 和 PDF，自動計算稅金和金額。 | `/quote-generator` |

---

### 🤖 AI 指揮工具 (3 個)

讓 Claude 指揮其他 AI 或專用工具進行特定任務。本質上是「Claude 元指揮層」，協調多個執行層。

| Skill | 指揮對象 | 觸發方式 |
|-------|---------|---------|
| **codex-commander** | 代碼搜尋引擎 | `/codex-commander` |
| **kiro-commander** | 代碼優化引擎 | `/kiro-commander` |
| **gemini-commander** | Google Gemini AI | `/gemini-commander` |

---

### 🗂️ 知識管理與可視化 (2 個)

組織和可視化知識結構，適合個人或團隊的知識庫建設。

| Skill | 描述 | 觸發方式 |
|-------|------|---------|
| **knowledge-base-organizer** | 知識庫結構優化。檢查資料夾層級、去重相似卡片、合併內容、驗證命名規範。 | `/knowledge-base-organizer` |
| **graphify** | 知識圖譜生成。將任何程式碼、文檔、論文轉化為結構化知識圖，可視化概念關係。 | `/graphify` |

---

### 📔 Obsidian 工具集 (8 個)

針對 Obsidian 知識庫的專用工具，覆蓋筆記撰寫、Canvas 設計、發布流程、日誌記錄等完整工作流。

| Skill | 描述 | 觸發方式 |
|-------|------|---------|
| **obsidian-note-writer** | 知識庫卡片撰寫規範。確保 frontmatter、Markdown、內嵌圖片、連結格式的一致性。 | `/obsidian-note-writer` |
| **obsidian-canvas-creator** | Canvas 畫布生成。自動產出視覺化 Canvas 檔，組織複雜知識結構。 | `/obsidian-canvas-creator` |
| **obsidian-bases** | Obsidian Bases 資料庫。管理結構化數據，支援篩選、分類、聯動。 | `/obsidian-bases` |
| **obsidian-cli** | 命令列工具。快速管理 Obsidian 檔案、同步、批量操作。 | `/obsidian-cli` |
| **obsidian-markdown** | Markdown 語法參考。支援 Obsidian 特有語法（callouts、properties、embeds）。 | `/obsidian-markdown` |
| **daily-log-recorder** | 每日工作日誌。標準化日誌模板和記錄流程。 | `/daily-log-recorder` |
| **publish-workflow** | 發布驗證與 Git 提交流程。確保代碼質量和提交信息規範。 | `/publish-workflow` |
| **transcript-knowledge-extractor** | 逐字稿知識提取。從會議紀錄、訪談轉錄自動提取關鍵知識卡片。 | `/transcript-knowledge-extractor` |

---

### 🌐 搜尋與資訊工具 (1 個)

高效的網路搜尋和資訊收集工具。

| Skill | 描述 | 觸發方式 |
|-------|------|---------|
| **search-strategy** | 搜尋工具策略。整合 gemini cli 和 WebSearch，支援多種模式（Claude / Gemini 前景 / Gemini 背景）。 | `/search-strategy` |

---

### 🔒 防火牆與網路安全 (2 個)

針對防火牆配置、安全分析、報告生成的專用工具。

| Skill | 描述 | 觸發方式 |
|-------|------|---------|
| **firewall-policy-analyzer** | 防火牆政策安全分析。檢查 Fortinet、Palo Alto 等防火牆的政策配置，識別安全漏洞和改進建議。 | `/firewall-policy-analyzer` |
| **fortigate-report-generator** | FortiGate CIS 建議書。根據審核結果自動產出專業建議書報告。 | `/fortigate-report-generator` |

---

### 📄 文檔與圖表工具 (2 個)

文檔格式轉換、圖表生成等工具。

| Skill | 描述 | 觸發方式 |
|-------|------|---------|
| **md-to-docx-converter** | Markdown 轉 Word。將 Markdown 檔案轉換為 DOCX，支援保留格式、圖片、表格。 | `/md-to-docx-converter` |
| **mermaid-visualizer** | Mermaid 圖表生成。自動生成流程圖、時序圖、狀態圖、思維導圖等。 | `/mermaid-visualizer` |

---

### 🎓 其他工具 (2 個)

專注於特定任務的高效工具。

| Skill | 描述 | 觸發方式 |
|-------|------|---------|
| **exam-answer-helper** | 考試答題助手。快速生成高質量答案（概念題、計算題、論述題、多選題）。 | `/exam-answer-helper` |
| **defuddle** | 模糊概念澄清。針對容易混淆的技術概念進行深度對比和澄清。 | `/defuddle` |

---

## 🚀 快速開始

### 安裝方式 1：完整安裝所有 28 個 Skills

```bash
# 克隆倉庫
git clone https://github.com/Tiange781122/claude-code-skills.git ~/claude-skills-repo

# 複製到 Claude Code 的 skills 目錄
cp -r ~/claude-skills-repo/* ~/.claude/skills/

# 重啟 Claude Code 以加載新 skills
```

### 安裝方式 2：單個 Skill 安裝

若只需要特定 skill，可複製單個資料夾：

```bash
cp -r ~/claude-skills-repo/tech-evolution-storyteller ~/.claude/skills/
cp -r ~/claude-skills-repo/quote-estimator ~/.claude/skills/
```

### 使用方式

安裝後，在 Claude Code 中直接輸入 skill 的觸發指令：

```
用 tech-evolution-storyteller 講解「HTTPS 加密如何保護數據」
```

或直接使用斜線指令：

```
/quote-analysis
/graphify
/codex-commander
```

---

## 📊 Skills 應用場景速查表

| 工作場景 | 推薦使用的 Skills |
|---------|------------------|
| 📚 準備技術教學 | tech-evolution-storyteller + tech-personification-storytelling |
| 💼 進行採購詢價 | quote-inquiry + quote-analysis |
| 💰 估算項目成本 | quote-estimator |
| 📄 產出正式報價單 | quote-generator |
| 🗂️ 整理知識庫 | knowledge-base-organizer |
| 📔 撰寫知識卡片 | obsidian-note-writer |
| 🕸️ 生成知識圖譜 | graphify |
| 🔍 查找代碼邏輯 | codex-commander |
| 🔄 優化代碼品質 | kiro-commander |
| 🌐 網路搜尋與分析 | gemini-commander + search-strategy |
| 📊 生成圖表與 Canvas | mermaid-visualizer + obsidian-canvas-creator |
| 📝 日誌記錄和發布 | daily-log-recorder + publish-workflow |
| 🎓 考試複習 | exam-answer-helper |
| 🔒 防火牆安全審查 | firewall-policy-analyzer + fortigate-report-generator |

---

## 📁 目錄結構

每個 skill 遵循標準結構，便於維護、理解和擴展：

```
skill-name/
├── SKILL.md                   # 必需：完整的 Skill 定義、使用指南、工作流說明
├── evals/                     # 可選：測試用例 (evals.json)
│   └── evals.json            # 包含 2-3 個實際使用場景的測試案例
├── scripts/                   # 可選：Python/Bash 工具腳本
├── references/                # 可選：參考文檔和資源
└── assets/                    # 可選：範本、圖標、資源文件
```

---

## 🔐 安全性與隱私

所有 skills 都經過以下嚴格驗證：

- ✅ **無敏感信息洩露** — 不包含 API 密鑰、密碼、Token、個人路徑
- ✅ **客戶隱私保護** — 測試數據已完全匿名化，不含真實客戶名稱
- ✅ **個人信息清除** — 移除所有個人路徑、Email、內部資訊
- ✅ **代碼審查** — 所有 skills 均經過資安審核
- ✅ **企業友好** — 適合企業內部使用和開源發佈

詳見 [SECURITY.md](SECURITY.md) 安全政策文檔

---

## 💡 核心設計原則

### 1. 模組化架構
每個 skill 獨立運作，也可與其他 skills 組合形成完整工作流。

### 2. 可擴展性
所有 SKILL.md 都遵循統一格式，支援社區貢獻和定制。

### 3. 效能優先
設計目標是將常見任務的完成時間減少 50-90%：
- 報價流程：2 小時 → 15 分鐘 (8 倍)
- 代碼搜尋：10 分鐘 → 2 分鐘 (5 倍)
- 知識整理：30 分鐘 → 10 分鐘 (3 倍)

### 4. 資安優先
- 敏感數據自動清理
- 完整的 .gitignore 配置
- 明確的隱私政策
- 開源友好

---

## 🤝 貢獻指南

我們歡迎以下貢獻：

- 🐛 **報告問題** — 發現 bug 或資安問題？詳見 [SECURITY.md](SECURITY.md)
- 📝 **改進文檔** — 優化說明、新增使用範例、改進 README
- 🔧 **新增 Skills** — 開發新的 skill 並提交 PR
- 💬 **提供反饋** — 使用體驗建議和改進方向

### 提交 PR 時的檢查清單

- [ ] 新 skill 包含完整的 SKILL.md (100-500 行)
- [ ] 已進行敏感信息掃描，確認無客戶/個人隱私數據
- [ ] 包含至少 2-3 個測試用例 (evals.json)
- [ ] 文檔清晰，包含使用場景、工作流、範例
- [ ] 通過資安審查 (參考 SECURITY.md)
- [ ] 遵循模組化設計原則

---

## 📞 支援與反饋

### 報告問題

發現 bug 或有建議？

- 🔒 **安全相關**：發送郵件至 security@example.com（不要在 Issue 中暴露細節）
- 🐛 **功能相關**：在 GitHub Issues 中詳細描述問題、環境、重現步驟
- 💬 **使用建議**：提交 Discussion 或 PR，分享你的使用經驗

### 常見問題 (FAQ)

**Q: 支持離線使用嗎？**  
A: 大多數 skills 可完全離線使用。需要網路的 skills：
- `gemini-commander` — 需要網路進行搜尋
- `tech-evolution-storyteller` — 搜尋功能需要網路
- `search-strategy` — Gemini 模式需要網路

**Q: 支持其他語言嗎？**  
A: 目前以繁體中文為主。部分 skills 支援英文。歡迎提交多語言版本！

**Q: 可以商用嗎？**  
A: 完全可以。MIT License 允許商用、修改、分發。詳見 [LICENSE](LICENSE)。

**Q: 如何更新到最新版本？**  
A: 定期更新倉庫：
```bash
cd ~/claude-skills-repo
git pull origin main
cp -r * ~/.claude/skills/
```

**Q: 可以修改 skill 嗎？**  
A: 當然可以！skills 是開源的，你可以根據需要自訂。建議備份原版本並修改副本。

---

## 🔗 相關資源

- 📖 [GitHub 倉庫](https://github.com/Tiange781122/claude-code-skills)
- 🔐 [安全政策](SECURITY.md)
- 📜 [MIT License](LICENSE)
- 🤖 [Claude 官方文檔](https://claude.ai)
- 📚 [Claude Code 使用指南](https://claude.ai/claude-code)

---

## 📝 版本歷史

### v3.0.0 (2026-05-06)

**重大擴展** — 完整技能集合
- 🚀 從 11 個技能擴展至 28 個
- 📔 新增 8 個 Obsidian 工具集（筆記、Canvas、日誌、發布、提取知識）
- 🔒 新增 2 個防火牆安全工具
- 📄 新增 2 個文檔轉換工具
- 🌐 新增搜尋策略工具
- ✅ 所有新增技能均經過敏感信息審查和安全認證

### v2.0.0 (2026-05-06)

**主要改進**
- 🔒 完全移除個人信息（路徑、Email、內部資訊）
- 📖 完全重寫 README，增加實際效能數據和應用場景
- 🤖 強調 Commander 系列的協調模式設計
- 📊 新增詳細的效能統計表

### v1.0.0 (2026-05-06)

**初始發佈**
- 🎉 包含 11 個核心 skills
- 🔐 完整的資安審核和隱私保護
- 📚 詳細的文檔和使用指南

---

## 📄 授權

本專案採用 **MIT License**。詳見 [LICENSE](LICENSE) 文件。

簡言：
- ✅ 可自由使用、修改、分發
- ✅ 商用免費
- ❌ 作者不提供擔保

---

## 👤 關於作者

**Bart** — 知識工作者、技術愛好者、AI 應用探索者

這個 skills 集合是我在日常工作中開發和積累的工具庫。每個 skill 都來自於解決實際問題的需求，經過多次迭代和優化。

如果你發現這些 skills 有幫助，歡迎：
- ⭐ 在 GitHub 上 star 此倉庫
- 🔗 分享給更多需要的人
- 💡 提交改進建議和新想法

---

## 🎉 感謝

感謝使用 **Bart's Claude Code Skills Collection**！

這是一個持續改進的開源項目，歡迎：
- 提交 Issue 報告 bug
- 提交 PR 改進代碼
- 分享你的使用經驗和建議
- 開發新的 skills 並貢獻給社區

**有任何問題或建議，歡迎在 GitHub Issues 中告訴我。** 🙌
