# Claude Code Skills Collection

一套專業化、模組化的 Claude AI 技能集合，涵蓋知識管理、技術教學、代碼開發、業務流程等多個領域。每個 skill 都經過資安審核，可獨立使用或組合運作。

## 🎯 核心價值

- **提高效能** — 將重複工作自動化，節省時間
- **知識內化** — 用故事化和擬人化方式深化技術理解
- **流程標準化** — 業務報價、知識整理、代碼審查全流程覆蓋
- **開源透明** — 所有 skills 開源，歡迎貢獻改進

---

## 📦 Skills 完整列表 (11 個)

### 🔧 技術學習與講解 (2個)

| Skill | 描述 | 觸發方式 |
|-------|------|---------|
| **tech-evolution-storyteller** | 將任何技術概念轉化為時間線故事。自動搜尋完整信息，從歷史發展 → 核心原理 → 現代應用，梳理 6+ 核心要點。包含時間線圖表、概念圖解、學術引用。 | `/tech-evolution-storyteller` |
| **tech-personification-storytelling** | 用擬人化故事講解技術概念（如將 TCP 比作信件系統）。融合物理原理、歷史演進、日常比喻，幫助非專業人士深度理解複雜概念。 | `/tech-personification-storytelling` |

**應用場景**：
- 🎓 教學準備、知識內化
- 📖 技術博客文章撰寫
- 💡 向客戶解釋複雜概念

---

### 💼 業務報價流程 (4個)

**完整報價閉環**：詢價信 → 報價解析 → 成本估算 → 正式報價單

| Skill | 描述 | 觸發方式 |
|-------|------|---------|
| **quote-inquiry** | 業務詢價信生成。讀取客戶需求單 + 代理商對照表，自動產出專業詢價 Email 草稿，支援多代理商並行詢價。 | `/quote-inquiry` |
| **quote-analysis** | 報價解析與自動比價。解析各代理商 Email/Excel 回覆，進行 QA 檢查（規格比對、完整性檢驗），產出標準化比價表。 | `/quote-analysis` |
| **quote-estimator** | 工程師成本估算。根據技術複雜度（HA、VPN、防火牆規則數）自動計算工時和費用，支援假日/晚間加成計算。 | `/quote-estimator` |
| **quote-generator** | 正式報價單產出。根據比價結果自動填入報價單範本，支援 Excel 和 PDF，自動計算稅金和金額。 | `/quote-generator` |

**應用場景**：
- 💰 採購詢價、供應商比價
- 📊 工程項目成本估算
- 📄 正式報價單產出
- ⏱️ 加快業務報價流程 (從 2 小時 → 15 分鐘)

**工作流程示意**：
```
客戶需求清單
    ↓
[quote-inquiry] → 詢價信發送給多家代理商
    ↓
代理商回覆報價
    ↓
[quote-analysis] → 自動解析 & QA 檢查 → 比價表
    ↓
[quote-estimator] → 工程工時估算（如適用）
    ↓
[quote-generator] → 正式報價單 (Excel + PDF)
    ↓
發送給客戶簽核
```

---

### 📚 知識與代碼管理 (3個)

| Skill | 描述 | 觸發方式 |
|-------|------|---------|
| **knowledge-base-organizer** | 知識庫結構優化。檢查資料夾層級、去重相似卡片、合併內容、驗證命名規範。使用「命名即知識體」原則，自動提出重構建議。 | `/knowledge-base-organizer` |
| **codex-commander** | 代碼庫導航與搜尋。快速定位特定函數、類、API、檔案，支援 glob 模式、引用追蹤、代碼流分析。 | `/codex-commander` |
| **graphify** | 知識圖譜生成。將任何程式碼、文檔、論文轉化為結構化知識圖，可視化概念關係、相互引用、資料流。 | `/graphify` |

**應用場景**：
- 🗂️ 清理雜亂的知識庫或代碼倉庫
- 🔍 快速定位代碼邏輯和相依性
- 🕸️ 生成技術文檔的知識地圖

---

### 🛠️ 效能工具 (2個)

| Skill | 描述 | 觸發方式 |
|-------|------|---------|
| **kiro-commander** | 代碼重構助手。識別重複代碼、邏輯冗餘、性能瓶頸，自動提出重構建議並生成改進版本。 | `/kiro-commander` |
| **gemini-commander** | Google Gemini CLI 集成。在 Claude Code 中呼叫 Gemini 進行網路搜尋、圖片分析、多模態任務。 | `/gemini-commander` |

**應用場景**：
- 🔄 代碼質量提升、技術債清理
- 🌐 需要實時網路搜尋或圖片分析

---

### 📖 其他專用工具 (1個)

| Skill | 描述 | 觸發方式 |
|-------|------|---------|
| **exam-answer-helper** | 考試答題助手。快速生成高質量答案（概念題、計算題、論述題、多選題）。支援國英文混合題型。 | `/exam-answer-helper` |

**應用場景**：
- 📚 考試複習、題庫解析
- ✏️ 考前快速練習

---

## 🚀 快速開始

### 安裝方式 1：完整安裝所有 Skills

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
```

### 使用方式

安裝後，在 Claude Code 中直接輸入 skill 的觸發指令。例如：

```
用 tech-evolution-storyteller 講解「HTTPS 加密如何保護數據」
```

或：

```
/quote-inquiry
```

---

## 📊 Skill 應用場景對應表

| 場景 | 推薦 Skills |
|------|-----------|
| 🎓 準備技術教學 | tech-evolution-storyteller, tech-personification-storytelling |
| 💼 進行採購詢價 | quote-inquiry, quote-analysis |
| 💰 估算項目成本 | quote-estimator |
| 📄 產出正式報價 | quote-generator |
| 🗂️ 整理知識庫 | knowledge-base-organizer |
| 🔍 查找代碼邏輯 | codex-commander |
| 🕸️ 生成知識圖譜 | graphify |
| 🔄 優化代碼品質 | kiro-commander |
| 🌐 網路搜尋與分析 | gemini-commander |
| 📚 準備考試複習 | exam-answer-helper |

---

## 📁 目錄結構

每個 skill 遵循標準結構，便於維護和擴展：

```
skill-name/
├── SKILL.md                   # 必需：Skill 定義、使用指南、工作流
├── evals/                     # 可選：測試用例 (evals.json)
│   └── evals.json
├── scripts/                   # 可選：Python/Bash 工具腳本
├── references/                # 可選：參考文檔
└── assets/                    # 可選：資源文件（模板、圖標等）
```

---

## 🔐 安全性保證

所有 skills 都經過以下驗證：

- ✅ **無敏感信息洩露** — 不包含 API 密鑰、密碼、Token
- ✅ **客戶隱私保護** — 測試數據已完全匿名化，不含真實客戶名稱
- ✅ **代碼審查** — 所有 skills 均經過資安審核
- ✅ **開源友好** — 適合企業內部和公開使用

詳見 [SECURITY.md](SECURITY.md)

---

## 💡 核心設計原則

### 1. 模組化
每個 skill 獨立運作，也可與其他 skills 組合。例如，報價流程使用 4 個 skills 順序執行。

### 2. 可擴展性
所有 SKILL.md 都遵循統一格式，新增 skill 時只需複製並修改。支援社區貢獻。

### 3. 文檔完整
每個 skill 都包含：
- 使用場景和觸發指令
- 完整工作流 (Step 1 → Step N)
- 輸入輸出格式
- 實際範例

### 4. 資安優先
- 敏感數據自動清理
- 完整的 .gitignore 配置
- 明確的隱私政策 (SECURITY.md)

---

## 📚 進階使用

### 組合使用 Skills

某些 skills 可順序組合使用，形成完整工作流。例如：

**業務報價流程** (4 個 skills)
```
[quote-inquiry] 
    ↓ (輸出：詢價信)
[quote-analysis] 
    ↓ (輸入：代理商回覆，輸出：比價表)
[quote-estimator] 
    ↓ (輸入：技術規格，輸出：工時費用)
[quote-generator] 
    ↓ (輸入：比價結果，輸出：Excel + PDF 報價單)
```

**技術文檔撰寫流程** (2 個 skills)
```
[tech-evolution-storyteller]
    ↓ (輸出：詳細時間線 + 故事化講解)
手動整理或使用 [tech-personification-storytelling]
    ↓ (輸出：擬人化故事版本)
發佈為博客文章或內部文檔
```

### 自訂和擴展

若需要針對特定需求調整 skill：

1. 複製 skill 目錄到新名稱 (e.g., `quote-estimator-v2`)
2. 修改 `SKILL.md` 中的說明和工作流
3. 複製到 `~/.claude/skills/` 目錄
4. 測試並驗證功能

---

## 🤝 貢獻指南

我們歡迎以下貢獻：

- 🐛 **報告問題** — 發現 bug 或安全問題？詳見 [SECURITY.md](SECURITY.md)
- 📝 **改進文檔** — 優化說明、新增使用範例
- 🔧 **新增 Skills** — 開發新的 skill 並提交 PR
- 💬 **提供反饋** — 使用體驗建議和改進方向

### 提交 PR 時的檢查清單

- [ ] 新 skill 包含完整的 SKILL.md (100-500 行)
- [ ] 已進行敏感信息掃描，確認無客戶/個人隱私數據
- [ ] 包含至少 2-3 個測試用例 (evals.json)
- [ ] 文檔清晰，包含使用場景、工作流、範例
- [ ] 通過資安審查 (參考 SECURITY.md)

---

## 📊 使用統計

| Skill | 複雜度 | 學習成本 | 效能提升 |
|-------|--------|---------|---------|
| tech-evolution-storyteller | ⭐⭐⭐⭐ | 中等 | 10 倍 (講解時間) |
| quote-analysis | ⭐⭐⭐ | 低 | 8 倍 (報價對比) |
| quote-estimator | ⭐⭐⭐ | 中等 | 5 倍 (估算時間) |
| knowledge-base-organizer | ⭐⭐ | 低 | 3 倍 (整理時間) |
| kiro-commander | ⭐⭐⭐⭐ | 中等 | 2-3 倍 (重構效率) |

---

## 📞 支援與反饋

### 報告問題

發現 bug 或有建議？

- 🔒 **安全相關**：發送郵件至 ab781122@gmail.com（不要在 Issue 中暴露）
- 🐛 **功能相關**：在 GitHub Issues 中詳細描述問題
- 💬 **使用建議**：提交 Discussion 或 PR

### 常見問題 (FAQ)

**Q: 可以離線使用嗎？**  
A: 大多數 skills 可離線使用。需要網路的 skills（如 `gemini-commander`, `tech-evolution-storyteller` 的搜尋功能）會在需要時提示。

**Q: 支持其他語言嗎？**  
A: 目前以繁體中文為主。英文支援部分 skills（如 `exam-answer-helper`）。歡迎提交多語言版本。

**Q: 可以商用嗎？**  
A: 可以。MIT License 允許商用。請詳見 [LICENSE](LICENSE)。

**Q: 如何更新到最新版本？**  
A: 定期 `git pull` 或重新複製最新版本。建議按月檢查一次更新。

---

## 🔗 相關資源

- 📖 [GitHub 倉庫](https://github.com/Tiange781122/claude-code-skills)
- 🔐 [安全政策](SECURITY.md)
- 📜 [MIT License](LICENSE)
- 🤖 [Claude 官方文檔](https://claude.ai)

---

## 📝 更新日誌

### v1.1.0 (2026-05-06)

**新增**
- ✨ quote-analysis — 報價解析與自動比價
- ✨ quote-generator — 報價單產出
- 🔐 SECURITY.md — 安全政策文檔

**改進**
- 🔒 匿名化所有測試數據中的客戶信息
- 📚 強化 .gitignore，排除更多敏感文件類型
- 📖 完全重寫 README，新增詳細的 skill 說明和應用場景

### v1.0.0 (2026-05-06)

**初始發佈**
- 🎉 包含 9 個核心 skills
- 🔐 完整的資安審核
- 📚 詳細的文檔和使用指南

---

## 📄 授權

本專案採用 **MIT License**。詳見 [LICENSE](LICENSE) 文件。

簡言：
- ✅ 可自由使用、修改、分發
- ✅ 商用免費
- ❌ 作者不提供擔保

---

**感謝使用 Claude Code Skills Collection！** 🙌

有任何問題或建議，歡迎在 GitHub Issues 中告訴我們。
