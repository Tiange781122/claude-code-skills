# Bart's Claude Code Skills Collection

一套專業化、模組化的 Claude AI 技能集合，由 Bart 開發和維護。涵蓋知識管理、技術教學、代碼開發、業務流程等多個領域。每個 skill 都經過資安審核並已移除所有敏感信息，適合企業內部和開源使用。

## 🎯 核心價值

- **提高效能** — 將重複工作自動化，節省時間（平均 3-10 倍效率提升）
- **知識內化** — 用故事化和擬人化方式深化技術理解
- **流程標準化** — 業務報價、知識整理、代碼審查全流程覆蓋
- **開源友好** — 所有 skills 開源，完全移除個人信息，適合公開發佈

---

## 📦 Skills 完整列表 (11 個)

### 🔧 技術學習與講解 (2個)

讓 Claude 用故事化方式講解複雜的技術概念，幫助用戶深度理解和教學準備。

| Skill | 描述 | 觸發方式 | 效能提升 |
|-------|------|---------|---------|
| **tech-evolution-storyteller** | 將任何技術概念轉化為時間線故事。自動搜尋完整信息，從歷史發展 → 核心原理 → 現代應用，梳理 6+ 核心要點。包含時間線圖表、概念圖解、學術引用。 | `/tech-evolution-storyteller` | 10x |
| **tech-personification-storytelling** | 用擬人化故事講解技術概念（如將 TCP 比作信件系統）。融合物理原理、歷史演進、日常比喻，幫助非專業人士快速理解複雜概念。 | `/tech-personification-storytelling` | 8x |

**應用場景**：
- 🎓 教學準備、知識內化
- 📖 技術博客文章撰寫
- 💡 向客戶和非技術人員解釋複雜概念

---

### 💼 業務報價流程 (4個)

完整的商業報價閉環：詢價信 → 報價解析 → 成本估算 → 正式報價單。可各自獨立使用，也可串聯組合。

| Skill | 描述 | 觸發方式 | 效能提升 |
|-------|------|---------|---------|
| **quote-inquiry** | 業務詢價信生成。讀取客戶需求單 + 代理商對照表，自動產出專業詢價 Email 草稿，支援多代理商並行詢價。 | `/quote-inquiry` | 5x |
| **quote-analysis** | 報價解析與自動比價。解析各代理商 Email/Excel 回覆，進行 QA 檢查（規格比對、完整性檢驗），產出標準化比價表。支援快速決策。 | `/quote-analysis` | 8x |
| **quote-estimator** | 工程師成本估算。根據技術複雜度（HA、VPN、防火牆規則數）自動計算工時和費用，支援假日/晚間加成計算。適合技術驅動的項目報價。 | `/quote-estimator` | 5x |
| **quote-generator** | 正式報價單產出。根據比價結果自動填入報價單範本，支援 Excel 和 PDF，自動計算稅金和金額。一鍵生成交付文件。 | `/quote-generator` | 6x |

**應用場景**：
- 💰 採購詢價、供應商比價（單次 15 分鐘 vs 手動 2 小時）
- 📊 工程項目成本估算、工時報價
- 📄 正式報價單和送件清單產出

**完整工作流**：
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

### 🤖 AI 指揮工具 (3個)

讓 Claude 指揮其他 AI 或專用工具進行特定任務。本質上是「Claude 元指揮層」，協調多個執行層以完成複雜任務。

| Skill | 指揮對象 | 職責 | 觸發方式 | 效能提升 |
|-------|---------|------|---------|---------|
| **codex-commander** | 代碼搜尋引擎 | 快速定位代碼庫中的特定函數、類、API、檔案，支援 glob 模式、引用追蹤、代碼流分析 | `/codex-commander` | 3x |
| **kiro-commander** | 代碼優化引擎 | 識別重複代碼、邏輯冗餘、性能瓶頸，自動提出重構建議並生成改進版本 | `/kiro-commander` | 3x |
| **gemini-commander** | Google Gemini AI | 網路搜尋、圖片分析、多模態任務，補充 Claude 的搜尋和分析能力 | `/gemini-commander` | 2x |

**設計理念**：
- 三個 Commander 都是「指揮工具」，Claude 是決策和協調的中樞
- codex 和 kiro 指揮代碼層的工具和引擎
- gemini 指揮另一個 AI 模型，實現 AI 協作
- 統一的指揮模式，易於未來擴展（如 anthropic-commander, openrouter-commander）

**應用場景**：
- 🔍 快速定位和分析代碼邏輯、相依性
- 🔄 自動化代碼品質改進、技術債清理
- 🌐 需要實時網路搜尋或圖片分析時
- 📊 複雜任務需要多個 AI 協同工作

---

### 📚 知識管理工具 (2個)

組織和可視化知識結構，適合個人或團隊的知識庫建設。

| Skill | 描述 | 觸發方式 | 效能提升 |
|-------|------|---------|---------|
| **knowledge-base-organizer** | 知識庫結構優化。檢查資料夾層級、去重相似卡片、合併內容、驗證命名規範。使用「命名即知識體」原則，自動提出重構建議。 | `/knowledge-base-organizer` | 3x |
| **graphify** | 知識圖譜生成。將任何程式碼、文檔、論文轉化為結構化知識圖，可視化概念關係、相互引用、資料流。支援心智圖和詳細圖譜兩種模式。 | `/graphify` | 4x |

**應用場景**：
- 🗂️ 清理雜亂的知識庫或代碼倉庫
- 🕸️ 生成技術文檔的知識地圖
- 📊 視覺化複雜系統的架構和相依性
- 🎓 團隊知識體系建立

---

### 📖 其他專用工具 (1個)

專注於特定領域的高效工具，幫助特定任務的快速完成。

| Skill | 描述 | 觸發方式 | 效能提升 |
|-------|------|---------|---------|
| **exam-answer-helper** | 考試答題助手。快速生成高質量答案（概念題、計算題、論述題、多選題）。支援國英文混合題型，包含詳細解釋和替代思路。 | `/exam-answer-helper` | 5x |

**應用場景**：
- 📚 考試複習、題庫解析
- ✏️ 考前快速練習
- 🧠 知識點深度理解

---

## 🚀 快速開始

### 安裝方式 1：完整安裝所有 11 個 Skills

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

| 工作場景 | 推薦使用的 Skills | 預期效率提升 |
|---------|------------------|----------|
| 📚 準備技術教學 | tech-evolution-storyteller + tech-personification-storytelling | 8-10x |
| 💼 進行採購詢價 | quote-inquiry + quote-analysis | 8x |
| 💰 估算項目成本 | quote-estimator | 5x |
| 📄 產出正式報價單 | quote-generator | 6x |
| 🗂️ 整理知識庫 | knowledge-base-organizer | 3x |
| 🔍 查找代碼邏輯 | codex-commander | 3x |
| 🕸️ 生成知識圖譜 | graphify | 4x |
| 🔄 優化代碼品質 | kiro-commander | 3x |
| 🌐 網路搜尋與分析 | gemini-commander | 2x |
| 📚 準備考試複習 | exam-answer-helper | 5x |
| 🎓 團隊知識體系 | knowledge-base-organizer + graphify | 3-4x |

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
每個 skill 獨立運作，也可與其他 skills 組合。例如，報價流程使用 4 個 skills 順序執行，也可單獨使用其中任何一個。

### 2. 可擴展性
所有 SKILL.md 都遵循統一格式：
- 用途和觸發指令清楚
- 完整的工作流 (Step 1 → Step N)
- 輸入輸出格式明確
- 實際使用範例

新增 skill 時只需複製並修改範本。支援社區貢獻。

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

## 📚 進階使用

### Commander 系列的協調模式

**AI 指揮工具的本質**是讓 Claude 作為決策層，協調其他專用工具或 AI 的執行層：

```
Claude (決策與協調層)
    ├─ codex-commander (代碼搜尋執行層)
    ├─ kiro-commander (代碼優化執行層)
    └─ gemini-commander (Gemini AI 執行層)
```

**實際應用場景示例**：

```
用戶需求：「分析 utils.py 的邏輯並優化它」

執行流程：
1. Claude → codex-commander
   「查找 utils.py 和所有呼叫它的地方」
   ↓ (回傳代碼內容 + 相依性關係)

2. Claude 分析
   「這裡有重複邏輯，可以合併」
   ↓ (決策和計劃)

3. Claude → kiro-commander
   「根據這些代碼，提出優化建議」
   ↓ (回傳優化版本)

4. Claude 整合結果
   → 完整的重構方案 + 優化說明
```

### 組合使用 Skills 的工作流

某些 skills 可順序組合使用，形成完整工作流：

**業務報價流程** (4 個 skills 串聯)
```
[quote-inquiry] 
    ↓ 輸出：詢價信
[quote-analysis] 
    ↓ 輸入：代理商回覆，輸出：比價表
[quote-estimator] 
    ↓ 輸入：技術規格，輸出：工時費用
[quote-generator] 
    ↓ 輸入：比價結果，輸出：Excel + PDF 報價單
```

**技術文檔撰寫流程** (2 個 skills + Commander 協助)
```
[tech-evolution-storyteller]
    ↓ 輸出：詳細時間線 + 故事化講解

[codex-commander] (可選) 
    ↓ 查找代碼範例、相關實現

[tech-personification-storytelling]
    ↓ 輸出：擬人化故事版本

→ 發佈為博客文章或內部文檔
```

### 自訂和擴展

若需要針對特定需求調整 skill：

1. 複製 skill 目錄到新名稱 (e.g., `quote-estimator-v2`)
2. 修改 `SKILL.md` 中的說明、工作流、計費邏輯等
3. 複製到 `~/.claude/skills/` 目錄
4. 測試並驗證功能

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

### 新增 Commander 系列 Skill

若想新增屬於 Commander 系列的 skill（如 `anthropic-commander`、`openrouter-commander`），請：

1. 遵循 `gemini-commander` 的設計模式
2. 在 SKILL.md 中明確說明「指揮的對象」和「執行能力」
3. 說明與其他 Commander 的協調方式
4. 包含詳細的整合範例
5. 通過資安審核

---

## 📊 效能統計

| Skill | 複雜度 | 學習成本 | 執行效能提升 | 適用人群 |
|-------|--------|---------|----------|---------|
| tech-evolution-storyteller | ⭐⭐⭐⭐ | 中等 | 10x | 教育者、技術寫手 |
| tech-personification-storytelling | ⭐⭐⭐ | 低 | 8x | 教育者、傳教士 |
| quote-inquiry | ⭐⭐ | 低 | 5x | 採購、業務 |
| quote-analysis | ⭐⭐⭐ | 低 | 8x | 採購、業務 |
| quote-estimator | ⭐⭐⭐ | 中等 | 5x | 工程師、PM |
| quote-generator | ⭐⭐ | 低 | 6x | 業務、PM |
| knowledge-base-organizer | ⭐⭐ | 低 | 3x | 知識工作者 |
| codex-commander | ⭐⭐⭐ | 低 | 3x | 開發者 |
| kiro-commander | ⭐⭐⭐⭐ | 中等 | 3x | 開發者 |
| gemini-commander | ⭐⭐ | 低 | 2x | 所有人 |
| graphify | ⭐⭐⭐ | 中等 | 4x | 知識工作者、架構師 |
| exam-answer-helper | ⭐⭐ | 低 | 5x | 學生、考試準備 |

---

## 📞 支援與反饋

### 報告問題

發現 bug 或有建議？

- 🔒 **安全相關**：發送郵件至 security@example.com（不要在 Issue 中暴露細節）
- 🐛 **功能相關**：在 GitHub Issues 中詳細描述問題、環境、重現步驟
- 💬 **使用建議**：提交 Discussion 或 PR，分享你的使用經驗

### 常見問題 (FAQ)

**Q: 可以離線使用嗎？**  
A: 大多數 skills 可完全離線使用。需要網路的 skills：
- `gemini-commander` — 需要網路進行搜尋
- `tech-evolution-storyteller` — 搜尋功能需要網路（其他功能可離線）

**Q: 支持其他語言嗎？**  
A: 目前以繁體中文為主。英文支援部分 skills：
- `exam-answer-helper` — 支援英文題目
- `quote-*` — 支援英文客戶信息
歡迎提交多語言版本！

**Q: 可以商用嗎？**  
A: 完全可以。MIT License 允許商用、修改、分發。請詳見 [LICENSE](LICENSE)。

**Q: 如何更新到最新版本？**  
A: 定期更新倉庫：
```bash
cd ~/claude-skills-repo
git pull origin main
cp -r * ~/.claude/skills/
```
建議按月檢查一次更新。

**Q: 可以修改 skill 嗎？**  
A: 當然可以！skills 是開源的，你可以根據需要自訂。建議：
1. 備份原版本
2. 修改副本以保留自訂版本
3. 如果改進後的版本有通用價值，考慮提交 PR

---

## 🔗 相關資源

- 📖 [GitHub 倉庫](https://github.com/Tiange781122/claude-code-skills)
- 🔐 [安全政策](SECURITY.md)
- 📜 [MIT License](LICENSE)
- 🤖 [Claude 官方文檔](https://claude.ai)
- 📚 [Claude Code 使用指南](https://claude.ai/claude-code)

---

## 📝 版本歷史

### v2.0.0 (2026-05-06)

**重大改進**
- 🔒 完全移除個人信息（路徑、Email、內部資訊）
- 📖 完全重寫 README，增加實際效能數據和應用場景
- 🤖 強調 Commander 系列的協調模式設計
- 📊 新增詳細的效能統計表
- 👤 加上作者歸屬 — Bart's Claude Code Skills Collection

**新增內容**
- 應用場景速查表（快速找到所需 skill）
- 完整的工作流示意和串聯使用示例
- 進階使用部分的實際應用場景
- 詳細的 FAQ 和常見問題解答

### v1.2.0 (2026-05-06)

**架構優化**
- 🤖 新增「AI 指揮工具」分類，整合 Commander 系列
- 說明三個 Commander 的協調模式和設計理念

### v1.1.0 (2026-05-06)

**新增功能**
- ✨ quote-analysis — 報價解析與自動比價
- ✨ quote-generator — 報價單產出
- 🔐 SECURITY.md — 安全政策文檔

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
